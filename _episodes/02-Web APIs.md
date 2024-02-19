---
title: "Web APIs"
teaching: 15
exercises: 5
questions:
- "How do Web APIs facilitate communication between software systems over the internet?"
- "What role does HTTP play in Web API interactions?"
- "How are requests and responses structured to exchange data effectively?"
objectives:
- "Understand Web API communication via client-server requests and responses."
- "Explore HTTP's role in standardizing Web API interactions and security with HTTPS."
- "Learn the structure of HTTP requests and responses for effective data exchange."
keypoints:
- "Web APIs enable client-server interactions over the internet through a structured exchange of requests and responses, allowing for seamless data retrieval and submission."

- "The Hypertext Transfer Protocol (HTTP) is crucial for Web API operations, outlining how messages are formatted and transmitted, ensuring consistent communication across different platforms."

- "In HTTP, the communication involves requests initiated by clients (with methods like GET, POST) and responses from servers, each containing headers and potentially a body with the relevant data."

- "HTTPS adds a layer of encryption to HTTP, enhancing security by protecting data in transit, making it essential for sensitive transactions."
---

As described previously, Web APIs enable communication between different software systems over the web. Typically, a _request_ is made by a client, which could be an application on your local laptop, to the API server that understands and processes the _request_ and retrieves the data requested and sends it back as a _response_ to the client.

## How APIs work

![How APIs work in relation to a web-server](../assets/img/APIServer.png)

This process is governed by a set of predefined rules and protocols that ensure seamless communication, regardless of the diversity in programming languages and hardware platforms involved. The cornerstone protocol in Web API interactions is HTTP (Hypertext Transfer Protocol), which outlines how messages are formatted and transmitted over the web, and how web servers and browsers should respond to various commands.

To make effective use of web APIs, we need to understand a little more about how the Web works than a typical Web user might. This lesson will focus on _clients_&mdash;computers and software applications that make requests to other computers or applications, and receive information in response. Computers and applications that respond to such requests are referred to as _servers_.

## World Wide Web
At its core, the initial World Wide Web concept brought together three key ideas:

1. The use of HTML (Hypertext Markup Language) documents which could contain hyperlinks to other documents (or different parts of the same document). These could reference documents located on any web server in the world.
2. That every file on the world wide web would have a unique URL (Uniform Resource Locator).
3. The Hypertext Transfer Protocol (HTTP) that is used to transfer data from the web server to the requesting client.


## URLs

A URL (also sometimes known as a URI or Uniform Resource Indicator) consists of two or three parts: the protocol followed by ://, the server name or IP address and optionally the path to the resource we wish to access. For example the URL http://carpentries.org means we want to access the default location on the server carpentries.org using the HTTP protocol. The URL https://carpentries.org/contact/ means we want to access the contact location on the carpentries.org server using the secure HTTPS protocol.

## Protocols, HTTP & HTTPS

You may (or may not) have wondered how it is that different web browsers, written independently by different companies and running on different operating systems, are able to talk to the same web servers using the same addresses, and get the same web pages back. This is because all web browsers implement the _HyperText Transfer Protocol_, or HTTP.

A protocol is nothing more than a system of rules that allow for communication between computers (or other devices). Much like a (human) language, it defines rules and syntax that when all parties follow, allow information to be transmitted from one device to another.

HTTPS is a protocol closely related to HTTP; it follows many of the same conventions as HTTP, particularly in the way client and server code is written, but includes additional encryption to ensure that untrusted third parties can't read or modify data in transit.

## Requests and responses

The two main objects in HTTP are the _request_ and the _response_. Each HTTP connection is initiated by sending a request, and is replied to with a response. Both the request and response have a _header_, that defines metadata about what is requested and what is included in the response, and both can also have a _body_, containing data.

An HTTP _request_ typically includes:

- HTTP Method: This indicates the action to be performed on the resource and includes verbs like GET (retrieve data), POST (submit data), PUT (update data), DELETE (remove data), among others.

- Headers: These provide essential information about the request or the client itself, such as content type, authentication details, etc.

- Body: Not present in all types of requests, the body contains data sent by the client to the server, commonly seen in POST or PUT requests.

An HTTP _response_ from a server typically consists of the following key components:

- Status Line: Includes the HTTP version, a status code, and a status message. The status code is a three-digit number that indicates the outcome of the request, with common codes including:

200 OK: The request was successful, and the response body contains the requested data.
404 Not Found: The requested resource could not be found on the server.
500 Internal Server Error: A generic error message indicating that something went wrong on the server.

- Headers: Provide metadata about the response, similar to request headers. Important response headers to understand are:

- Content-Type: Specifies the type of data in the response body (e.g., application/json, text/html).

- Content-Length: The length of the response body in octets (8-bit bytes).

- Body: Contains the data being sent back to the client. This could be the requested resource, confirmation of an action taken, or an error message. The presence and format of the body depend on the request type and the status code. For example:

In a GET request for a webpage, the body would contain the HTML of the page.
In a POST request that submits data (like a form submission), the response body might contain a confirmation message or the details of the created resource.

## Example HTTP request and response in bash
To look at these in more detail, we can use the `curl` command. Specifically, to see the request headers, we can use `curl -i` followed by the URL we wish to request.

~~~
$ curl -i http://carpentries.org
~~~
{: .language-bash}

~~~
HTTP/1.1 301 Moved Permanently
server: envoy
date: Fri, 16 Feb 2024 02:58:34 GMT
content-type: text/html
content-length: 167
location: https://carpentries.org/
x-cache: Redirect from cloudfront
via: 1.1 50b82c7b764423c87ef333da04c77668.cloudfront.net (CloudFront)
x-amz-cf-pop: MEL52-P2
x-amz-cf-id: wD4E6tTNSW3i-Y9OBxEQPvhXRONa0IXspKqOHxzJM_FxUpiCttBRTA==
x-xss-protection: 1; mode=block
x-frame-options: SAMEORIGIN
referrer-policy: strict-origin-when-cross-origin
content-security-policy: default-src https: 'unsafe-eval' 'unsafe-inline'; object-src 'none'
x-content-type-options: nosniff
vary: Origin
x-envoy-upstream-service-time: 19

<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>CloudFront</center>
</body>
</html>
~~~
{: .output}

Noteworthy here is the first header `content-type: text/html`; this indicates that the response body is an HTML document (also known as a web page). HTML, the HyperText Markup Language, is the language that all web pages are written in; while we won't write any today, we will look a little more at how to read it (and get your code to read it) in a later episode.

Next, we will explore how data is managed and transmitted over the web, which commonly involves sending the data as part of the response body in HTTP transactions.

> ## HyperText?
>
> Both HTTP and HTML refer to HyperText. This was a popular buzzword in the 1990s, and refers to the Web's ability to include not only text, but also cross-references in the form of links (_hypertext links_, or _hyperlinks_) to other documents stored elsewhere, which the user can immediately access.
>
> While this seems entirely obvious and second-nature today, it was revolutionary when it was first introduced, hence the name appearing prominently in technologies that supported it.
{: .callout}