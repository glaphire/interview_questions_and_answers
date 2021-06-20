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

Alternatively, the data may be streamed via the 
[Server-Sent Events (SSE)](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events) method, 
for which there is a standardized HTML5 API called EventSource. With SSE, 
data is encoded as __text/event-stream__ in the header.


