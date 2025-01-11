---
layout: post
title: "XcodeGen - say goodbye to *.xcodeproj git conflicts"
description: "XcodeGen is a command-line tool that was created to simplify the usage, definition and properties of our lovely Xcode projects, thereby avoiding git conflicts and dealing with a non-human-readable file like the xcodeproj!"
tag:
  - swift
  - tool
  - xcodegen
---

# XcodeGen

To avoid copy and paste, the below definition comes from XcodeGen's github repository ([source](https://github.com/yonaskolb/XcodeGen)).

> XcodeGen is a command line tool written in Swift that generates your Xcode project using your folder structure and a project spec.

> The project spec is a YAML or JSON file that defines your targets, configurations, schemes, custom build settings and many other options. All your source directories are automatically parsed and referenced appropriately while preserving your folder structure. Sensible defaults are used in many places, so you only need to customize what is needed. Very complex projects can also be defined using more advanced features.

# How to start?

### First, you need to install it:

As it usually is in the IT world this can be achieved in many different ways but I list the most common ones used by me (more ways, can be checked on the official project page to which the link was shared at the beginning of this page).

#### Homebrew

```
brew install xcodegen
```

#### Mint

```
mint install yonaskolb/xcodegen
```

### Define your project configuration using configuration file/s

1. Create a project's root YML configuration file.
   By default it's called `project.yml`. Your whole project configuration can be done using this single file what may be enough for small project but usually it's much better to treat this file as an entry point of your project configutation and define smaller 'sub-project' config files for your apps, frameworks etc.

   ```
   ###########
   ## Project
   ###########
   name: MyProjectName

   ############
   ## Options
   ############

   ## Here we can override some project properties

   options:

   # You can define here more like tvOS etc but in this particular case I will be use iOS. only.
   deploymentTarget:
      iOS: "18.0"

   ############
   # Settings
   ############

   settings:
   MARKETING_VERSION: "0.0.1"
   CURRENT_PROJECT_VERSION: "1"

   ############
   ## Targets
   ############

   # Lets define our targets. We will have
   # - An app target
   # - A service package
   # - A test target for this service package

   targets:

   # Name for our service package
   ServiceCore:
      type: framework # this package its a framework
      platform: iOS   # for iOS only

      # configure bundle and info.plist
      settings:
         PRODUCT_BUNDLE_IDENTIFIER: "com.myapp.core.service"
         INFOPLIST_FILE: service/Info.plist

      # Point where the service core sources are. Support wildcards and "exclude" options.
      # we use path and group keys to generate a refence folder name "ServiceCore"
      # where all files related with ServiceCore will be added
      sources:
         - path: core/service/sources
         group: Modules/ServiceCore
         - path: core/service/resources
         group: Modules/ServiceCore

      # Set our dependencies from external libraries
      dependencies:
         - package: SwiftAlgorithms

      # Link with its target
      scheme:
         gatherCoverageData: true
         testTargets:
         - name: ServiceCore_Tests
            parallelizable: true
            randomExecutionOrder: true

   # Name for our test core package
   ServiceCore_Tests:
      type: bundle.unit-test
      platform: iOS
      settings:
         INFOPLIST_FILE: service/ServiceCoreTests-Info.plist
      sources:
         - path: core/service/tests
         group: Modules/ServiceCore

      # Link with ServiceCore target
      dependencies:
         - target: ServiceCore

      # Create a new scheme that will be linked and configured for test purposes
      scheme:
         gatherCoverageData: true
         testTargets:
         - name: ServiceCore_Tests
            parallelizable: true
            randomExecutionOrder: true

   # Name for our app target
   MyApp:
      type: application
      platform: iOS    # choose destination platform
      deploymentTarget: 16.0
      sources:
         # Where app files will be found.
         - path: app/sources
         group: app
      settings:
         # Select the bundle identifier
         PRODUCT_BUNDLE_IDENTIFIER: "com.myapp.xcodegen"
         # Select where Info.plist will be found
         INFOPLIST_FILE: app/Info.plist
      dependencies:
         # Dependencies. Here only local one (ServiceCore)
         - target: ServiceCore
      entitlements:
         path: app/Debug.entitlements
         properties:
         com.apple.security.application-groups: group.com.myapp

   ############
   ## Packages
   ############

   # Define the Swift Package Manager external libraries that the app will use
   packages:
   SwiftAlgorithms:
      url: https://github.com/apple/swift-algorithms
      exactVersion: 1.2.0
   ```

   NOTE: You need to create the source directories by your own, otherwise you will see errors like below:

   ```
   1 Spec validations errors:
      - Target "MyApp" has a missing source directory "xcodegen-base-setup/app/sources"
   ```

   [Here](https://github.com/yonaskolb/XcodeGen/issues/1211) someone created an improvement request to allow XcodeGen to create them but it's quite old. Maybe it's waiting for you to introduce a PR with required changes ;).

2. Run command to generate or update xcodeproj directory

   ```
   xcodegen -s project.yml
   ```

   NOTE: You can even omitt the `-s project.yml` as by default it is looking for such file

   and you should see the success response like below what means that your project file was created and you can open it

   ```
   xcodegen -s project.yml
   ⚙️  Generating plists...
   ⚙️  Generating project...
   ⚙️  Writing project...
   Created project at ../xcodegen-base-setup/MyProjectName.xcodeproj
   ```

   In Xcode you will see this

   ![BaseXcodeProjectStructure](/images/2025-01-11-XcodeGen-introduction/empty-xcodegen-project-structure.png)

   As we defined in the yaml file, we have:

   a) An app folder with the defined sources.

   b) A `Module` group which contains local dependency `SerivceCore` folder with its code and the unit tests.

   c) Three targets

   - MyApp
   - ServiceCore
   - ServiceCore_Tests

   ![BaseXcodeProjectStructure](/images/2025-01-11-XcodeGen-introduction/xcode-targets.png)

3. Update your `.gitignore` file to ignore your `*.xcodeproj` directory

   ```
   *.xcodeproj
   ```

4. If you change anything in your YML files configuration, you need to re-invoke command from point 2 again and again

   Please note that this is just a base example and most likely you will need more stuff for your projects and Xcodegen's Github repository will become your friend.

# Summary

For me, after many years as iOS developer, the XcodeGen is my best friend. It greatly enhances project management making it a powerful and efficient tool for iOS developers.

### Advantages

1. Source of Truth

   The `project.yml` file becomes the single source of truth for your project configuration.
   This reduces the risk of inconsistencies caused by manual edits to the `.xcodeproj` file.

2. Avoid Merge Conflicts

   Xcode project files are XML-based and prone to merge conflicts when multiple developers modify them simultaneously.
   With XcodeGen, the project file is regenerated from `project.yml`, eliminating the need to resolve conflicts manually.

3. Readable and Maintainable Configuration

   The `project.yml` file is easier to read and maintain compared to the `.xcodeproj` file.
   Developers can quickly understand and modify project settings, build configurations, and dependencies.

4. Version Control Friendly

   Since the `.xcodeproj` file is regenerated, you don't need to commit it to version control.
   This reduces repository size and ensures the project file is always up-to-date with the project.yml.

5. Automation and Reproducibility

   The ability to regenerate the project file on demand simplifies CI/CD pipelines.
   Teams can ensure consistent project files across environments by using the same project.yml.

6. Customizable and Scalable

   XcodeGen supports advanced features like:

   - Custom build settings
   - Schemes
   - Targets with specific configurations
   - Groups and file structures

7. Faster Iteration

   Making changes in the `project.yml` file is faster than editing the .xcodeproj in Xcode.
   This accelerates iterations on project settings.

8. Supports Modular Development

   XcodeGen is well-suited for modular apps with multiple frameworks or targets.
   You can define dependencies between modules and manage them in the YAML file.

9. Platform-Independent

   XcodeGen can run on any machine with macOS and does not require Xcode to be installed for generating .xcodeproj files. This makes it easy to set up projects on new developer machines or CI systems.

10. Extensibility

    XcodeGen can be extended through scripts or custom configurations, making it adaptable to unique project requirements.

11. Open Source and Community Support

    XcodeGen is open-source, meaning it's free to use and has an active community contributing to its development.
    The community provides support and examples for implementing best practices.

Life is not perfect, so there are some disadventages too

### Disadvantages

1. Learning Curve

   Developers unfamiliar with XcodeGen need to learn how to write and maintain the `project.yml` file, which can be an additional burden, especially for teams new to YAML/JSON syntax or declarative project management tools.

2. Lack of Real-Time Updates

   Changes made in the `project.yml` file require regenerating the `.xcodeproj` file using the xcodegen generate command.
   Developers might forget to regenerate the project file or fail to notice that changes in Xcode have been overridden, leading to confusion.

3. Manual Configuration of Some Features

   XcodeGen requires explicit declarations for all targets, schemes, and settings. Features like custom build phases, `Info.plist` configurations, and framework search paths must be manually added. This may increase the time spent configuring the `project.yml` file for complex projects.

4. Dependency on a Separate Tool

   XcodeGen adds an external dependency to your project setup. Team members and CI/CD systems must install and maintain the correct version of XcodeGen. However, You can always include the binary file in your project.

5. Compatibility Issues with Xcode Updates

   When Apple introduces new features or updates the Xcode project format, XcodeGen might require updates to support them.Maybe even I should put it as 1.

# When XcodeGen may not be ideal

- Small teams or solo developers: manual .xcodeproj management might be simpler.
- Heavily customized Xcode setups: complex configurations might require more manual intervention than XcodeGen allows.

# Bibliography

- XcodeGen documentation ([link](https://github.com/yonaskolb/XcodeGen/blob/master/Docs/Usage.md))

- Github repository with the implemented example ([link](https://github.com/polok/xcodegen-showcase))
