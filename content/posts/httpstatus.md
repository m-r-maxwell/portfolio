+++ 
draft = false
date = 2024-05-29T12:12:43-04:00
title = "HTTP Status Codes Cheat Sheet"
description = "HTTP Status Codes"
slug = ""
authors = []
tags = ["Web Dev"]
categories = ["Web Dev"]
externalLink = ""
series = ["Web Dev"]
+++

## Information Responses - 1xx
| Status Code | Description |
| --- | --- |
|100 - Continue | Indicates that the client should continue the request |
|101 - Switching Protocols | Indicates the protocol the server is switching to |
|103 - Early Hints | Primarily intended to be used with the link header|

## Successful Responses - 2xx
| Status | Description |
| --- | --- |
|200 - OK | Successful request |
|201 - Created | Added a new resource |
|202 - Accepted | Received but not acted upon |
|203 - Non-Authoriative Info| Metadata is not exactly the same as is available on the server| 
|204 - No Content | No contend to send for this request |
|205 - Reset Content | Reset the document which sent the request |
|206 - Partial Content | Sent with Range header for partial resources |
|207 - Multi-Status | Conveys information about multiple resources |
|208 - Already Reported | Response element to avoid repeated enumerating of a collection |
|226 - IM Used | Fulfilled request to represent results of one or more instance-manipulations applied |

## Redirection Messages - 3xx
| Status | Description |
| --- | --- |
|300 - Multiple Choices | One or more possible response |
|301 - Moved Permenantly | Returns a new URL for the resource that has been moved |
|302 - Found | URL of requested resource has been changed temporarily |
|303 - See Other | Get requested resource at another URL with a GET request |
|304 - Not Modified | Caching, response not modified |
|305 - Use Proxy | Deprecated |
|306 - Unused | Not used |
|307 - Temporary Redirect | Sends the client to get requested resource at another URI with the same method |
|308 - Permenant Redirect | Resource is now permanently located at another URI |

## Client Error Responses - 4xx
| Status | Description |
| --- | --- |
|400 - Bad Request | Cannot or will not process the request|
|401 - Unauthorized | Must authenticate with the server |
|402 - Payment Required | Meant for future use with digital payment systems |
|403 - Forbiddin | Does not have access rights to the content |
|404 - Not Found | Resource not found |
|405 - Method Not Allowed | Request is valid, but not with HTTP Action Type |
|406 - Not Acceptable | Content does not conform to the criteria given |
|407 - Proxy Authentication Required | Similar to 401 but done via a proxy |
|408 - Request Timeout | Server would like to shutdown unusued connection |
|409 - Conflict | Request content conflicts with the current state of the server |
|410 - Gone | Requested content has been permanently deleted |

**There are a lot more than these but these are the most common ones that I have found.**

## Server Error Response - 5xx
| Status | Description |
| --- | --- |
|500 - Internal Server Error | Server encountered something it does not know how to handle |
|501 - Not Implemented | Requested method is not supported by the server |
|502 - Bad Gateway | Server working as a gateway and got an invalid response |
|503 - Service Unavailable | The service is not ready to handle the request |
|504 - Gateway Timeout | Server is acting as a gateway and cannot get a response |

**There are a few more than these but these are generally the most common**