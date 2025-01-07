---
layout: post
title: "URLError codes returned by URL loading APIs"
description: "When something goes wrong during a network task, URLSession provides an NSError object that can contain URLError codes"
tag:
  - swift
  - URLSession
---

`URLError` are part of the Foundation framework, commonly used for network requests. When something goes wrong during a network task, `URLSession` provides an `NSError` object that can contain `URLError` codes. These error codes belong to the `URLError.Code` enumeration.

Below is a list of `URLError` error codes with their meanings:

| Error category  | Error                                          | Code  | Meaning                                                   |
| --------------- | ---------------------------------------------- | ----- | --------------------------------------------------------- |
| Connection      | `cancelled`                                    | -999  | The task was explicitly canceled, usually by the app      |
|                 | `badURL`                                       | -1000 | The provided URL is malformed or invalid.                 |
|                 | `unsupportedURL`                               | -1002 | The URL scheme is not supported                           |
|                 | `cannotFindHost`                               | -1003 | The host name could not be resolved                       |
|                 | `cannotConnectToHost`                          | -1004 | The connection to the host failed                         |
|                 | `networkConnectionLost`                        | -1005 | The network connection was lost during the request        |
|                 | `dnsLookupFailed`                              | -1006 | The DNS lookup failed                                     |
|                 | `httpTooManyRedirects`                         | -1007 | Too many HTTP redirects were encountered                  |
|                 | `resourceUnavailable`                          | -1008 | The requested resource is unavailable                     |
|                 | `notConnectedToInternet`                       | -1009 | The device is not connected to the Internet               |
|                 | `redirectToNonExistentLocation`                | -1010 | The request redirected to a nonexistent location          |
|                 | `badServerResponse`                            | -1011 | The server returned an invalid or unexpected response     |
|                 | `userCancelledAuthentication`                  | -1012 | The user canceled authentication                          |
|                 | `userAuthenticationRequired`                   | -1013 | Authentication is required to access the resource         |
|                 | `zeroByteResource`                             | -1014 | The resource's data is zero bytes                         |
|                 | `cannotDecodeRawData`                          | -1015 | The raw data could not be decoded                         |
|                 | `cannotDecodeContentData`                      | -1016 | The content data could not be decoded                     |
|                 | `cannotParseResponse`                          | -1017 | The response data could not be parsed                     |
| Certificate     | `appTransportSecurityRequiresSecureConnection` | -1022 | App Transport Security (ATS) requires a secure connection |
|                 | `secureConnectionFailed`                       | -1200 | The SSL/TLS handshake failed                              |
|                 | `serverCertificateHasBadDate`                  | -1201 | The server’s certificate has an invalid date              |
|                 | `serverCertificateUntrusted`                   | -1202 | The server’s certificate is untrusted                     |
|                 | `serverCertificateHasUnknownRoot`              | -1203 | The server’s certificate has an unknown root authority    |
|                 | `serverCertificateNotYetValid`                 | -1204 | The server’s certificate is not yet valid                 |
|                 | `clientCertificateRejected`                    | -1205 | The server rejected the client certificate                |
|                 | `clientCertificateRequired`                    | -1206 | The server requires a client certificate                  |
| Download        | `cannotLoadFromNetwork`                        | -2000 | The resource cannot be loaded from the network            |
|                 | `cannotCreateFile`                             | -3000 | Failed to create a file on disk                           |
|                 | `cannotOpenFile`                               | -3001 | Failed to open a file on disk                             |
|                 | `cannotCloseFile`                              | -3002 | Failed to close a file on disk                            |
|                 | `cannotWriteToFile`                            | -3003 | Failed to write to a file on disk                         |
|                 | `cannotRemoveFile`                             | -3004 | Failed to remove a file on disk                           |
|                 | `cannotMoveFile`                               | -3005 | Failed to move a file on disk                             |
|                 | `downloadDecodingFailedMidStream`              | -3006 | Download decoding failed during the stream                |
|                 | `downloadDecodingFailedToComplete`             | -3007 | Download decoding failed to complete                      |
| Timeout         | `timedOut`                                     | -1001 | The request timed out                                     |
| Background Task | `backgroundSessionRequiresSharedContainer`     | -995  | A background session requires a shared container          |
|                 | `backgroundSessionInUseByAnotherProcess`       | -996  | The background session is in use by another process       |
|                 | `backgroundSessionWasDisconnected`             | -997  | The background session was disconnected                   |
