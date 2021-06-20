# HTTP Streaming

[source](https://www.pubnub.com/learn/glossary/what-is-http-streaming/)

https://en.wikipedia.org/wiki/HTTP_Live_Streaming

https://ru.wikipedia.org/wiki/HLS

https://facecast.net/ru/blog/http-live-streaming/

HTTP Streaming is a push-style data transfer technique that 
allows a web server to continuously send data to a client 
over a single HTTP connection that remains open indefinitely. 
Technically, this goes against HTTP convention, but HTTP Streaming 
is an efficient method to transfer all kinds of dynamic or 
otherwise streamable data between the server and client 
without reinventing HTTP.

## How it works
The server is configured to hold connection with specific client.
When the new updates appear, the server sends a response through the request-response channel.
Connection can be closed only by specific request. Main benefits from this approach are lack of
overhead to open/close connections. It also eliminates the need of polling.

T server must respond to client requests by specifying __Transfer Encoding: chunked__ in the header.
This sets up a persistent connection from server 
to client and allows the server to send response data in chunks of newline-delimited strings. 
These chunks of data can then be received and processed on-the-fly by the client.

[HTTP header - chunked transfer encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding)

Alternatively, the data may be streamed via the 
[Server-Sent Events (SSE)](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events) method, 
for which there is a standardized HTML5 API called EventSource. With SSE, 
data is encoded as __text/event-stream__ in the header.

## HTTP persistent connections
[wikipedia](https://en.wikipedia.org/wiki/HTTP_persistent_connection)

HTTP persistent connection, also called _HTTP keep-alive_, 
or HTTP connection reuse, is the idea of using a single TCP connection 
to send and receive multiple HTTP requests/responses, as opposed to opening 
a new connection for every single request/response pair. 
The newer HTTP/2 protocol uses the same idea and takes it further to allow 
multiple concurrent requests/responses to be multiplexed over a single connection.

### Implementations
__HTTP 1.0__

Under HTTP 1.0, connections are not considered persistent unless a keep-alive header is included,
although there is no official specification for how keepalive operates.
It was, in essence, added to an existing protocol. 
If the client supports keep-alive, it adds an additional header to the request

	Connection: keep-alive
	
Then, when the server receives this request and generates a response, 
it also adds same "Connection: keep-alive" header to the response.
Following this, the connection is not dropped, but is instead kept open. 
When the client sends another request, it uses the same connection. 
This will continue until either the client or the server decides that the conversation is over, 
and one of them drops the connection.

__HTTP 1.1__

In HTTP 1.1, all connections are considered persistent unless declared otherwise.
The HTTP persistent connections do not use separate "keep-alive" messages, 
they just allow multiple requests to use a single connection. 
However, the default connection timeout of Apache httpd 1.3 and 2.0 is as little as 15 seconds
and just 5 seconds for Apache httpd 2.2 and above.
The advantage of a short timeout is the ability to deliver multiple components 
of a web page quickly while not consuming resources 
to run multiple server processes or threads for too long.

__Keepalive with chunked transfer encoding__

Keepalive makes it difficult for the client to determine 
where one response ends and the next response begins, 
particularly during pipelined HTTP operation.
This is a serious problem when Content-Length cannot be used due to streaming.
To solve this problem, HTTP 1.1 introduced a chunked transfer coding that defines a last-chunk bit.
_The last-chunk bit is set at the end of each response so that the client knows where the next response begins._


__HTTP 2.0__

HTTP 2.0 Allows predicting pushing before content is requested.
TODO: add more info

### Advantages
- Reduced latency in subsequent requests (no handshaking).
- Reduced CPU usage and round-trips because of fewer new connections and TLS handshakes.
- Enables HTTP pipelining of requests and responses.
- Reduced network congestion (fewer TCP connections). (network congestion - перегрузка сети)
- Errors can be reported without the penalty of closing the TCP connection.

### Disadvantages
- Reduced server availability due to keeping connections to "old" clients
- Race conditions: when the client sends a request to the server at 
the same time that the server closes the TCP connection.
A server should send a 408 Request Timeout status code to the 
client immediately before closing the connection. Client can open a new connection when
request is idempotent.

## HTTP Pipelining

[wikipedia](https://en.wikipedia.org/wiki/HTTP_pipelining)

HTTP pipelining is a feature of HTTP/1.1 which allows multiple HTTP requests 
to be sent over a single TCP connection without waiting for the corresponding responses.
HTTP/1.1 specification requires servers to respond to pipelined requests correctly, 
sending back non-pipelined but valid responses even if server does not support HTTP pipelining. 
Despite this requirement, many legacy HTTP/1.1 servers do not support pipelining correctly, 
forcing most HTTP clients to not use HTTP pipelining in practice.

The technique was replaced by multiplexing via HTTP/2, which is supported by most modern browsers.

![HTTP pipelining illustration](https://en.wikipedia.org/wiki/File:HTTP_pipelining2.svg)

### Pipelining pros and cons

The pipelining of requests results in a dramatic improvement in the loading times of HTML pages, 
especially over high latency connections such as satellite Internet connections. 
The speedup is less apparent on broadband connections, as the limitation of HTTP 1.1 still applies: 
the server must send its responses in the same order that the requests were 
received—so the entire connection remains first-in-first-out and HOL blocking can occur. 
The asynchronous operation of HTTP/2 and SPDY are solutions for this.
Browsers ultimately did not enable pipelining by default, 
and by 2017 most browsers supported HTTP/2 by default which used multiplexing instead.

Non-idempotent requests, like those using POST, should not be pipelined.
Sequences of GET and HEAD requests can always be pipelined. 
A sequence of other idempotent requests like PUT and DELETE can be pipelined 
or not depending on whether requests in the sequence depend on the effect of others.

HTTP pipelining requires both the client and the server to support it. 
HTTP/1.1 conforming servers are required to support pipelining. 
This does not mean that servers are required to pipeline responses, 
but that they are required not to fail if a client chooses to pipeline requests.

### Head-of-line blocking

HOL blocking in computer networking is a performance-limiting phenomenon
that occurs when a line of packets is held up by the first packet.
Examples include input buffered network switches, 
out-of-order delivery and multiple requests in HTTP pipelining.

One form of HOL blocking in HTTP/1.1 is when the number of allowed parallel 
requests in the browser is used up, and subsequent requests need to wait 
for the former ones to complete. 
HTTP/2 addresses this issue through request multiplexing, 
which eliminates HOL blocking at the application layer, 
but HOL still exists at the transport (TCP) layer.

