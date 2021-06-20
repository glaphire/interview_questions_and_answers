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

