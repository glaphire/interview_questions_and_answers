# HTTP Streaming

[source www.pubnub.com](https://www.pubnub.com/learn/glossary/what-is-http-streaming/)


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

## HTTP Live Streaming

[wikipedia](https://en.wikipedia.org/wiki/HTTP_Live_Streaming)

HTTP Live Streaming (also known as HLS) is an HTTP-based adaptive bitrate 
streaming communications protocol developed by Apple Inc. and released in 2009. 
Support for the protocol is widespread in media players, web browsers, mobile devices, 
and streaming media servers. 

HLS resembles MPEG-DASH in that it works by breaking the overall stream into a sequence 
of small HTTP-based file downloads, each downloading one short chunk of an overall 
potentially unbounded transport stream. A list of available streams, encoded at different bit rates, 
is sent to the client using an extended M3U playlist.

Based on standard HTTP transactions, HTTP Live Streaming can traverse any firewall 
or proxy server that lets through standard HTTP traffic, 
unlike UDP-based protocols such as RTP. 
This also allows content to be offered from conventional HTTP servers and delivered 
over widely available HTTP-based content delivery networks.
The standard also includes a standard encryption mechanism and secure-key distribution using HTTPS, 
which together provide a simple DRM system. 
Later versions of the protocol also provide for trick-mode fast-forward and 
rewind and for integration of subtitles.

Apple has documented HTTP Live Streaming as an Internet Draft (Individual Submission),
the first stage in the process of publishing it as a Request for Comments (RFC).

## Comparisons

[stackoverflow answer](https://stackoverflow.com/a/14711517/5371978)

- __TCP__: low-level, bi-directional, full-duplex, and guaranteed order transport layer. 
No browser support (except via plugin/Flash).
- __HTTP 1.0__: request-response transport protocol layered on TCP. 
The client makes one full request, the server gives one full response, 
and then the connection is closed. The request methods (GET, POST, HEAD) 
have specific transactional meaning for resources on the server.
- __HTTP 1.1__: maintains the request-response nature of HTTP 1.0, 
but allows the connection to stay open for multiple full requests/full 
responses (one response per request). Still has full headers in the request 
and response but the connection is re-used and not closed. HTTP 1.1 
also added some additional request methods (OPTIONS, PUT, DELETE, TRACE, CONNECT) 
which also have specific transactional meanings. However, as noted in the introduction 
to the HTTP 2.0 draft proposal, HTTP 1.1 pipelining is not widely deployed so this greatly 
limits the utility of HTTP 1.1 to solve latency between browsers and servers.
- __Long-poll__: sort of a "hack" to HTTP (either 1.0 or 1.1) where the server 
does not respond immediately (or only responds partially with headers) 
to the client request. After a server response, the client immediately 
sends a new request (using the same connection if over HTTP 1.1).
- __HTTP streaming__: a variety of techniques (multipart/chunked response) 
that allow the server to send more than one response to a single 
client request. The W3C is standardizing this as Server-Sent Events using 
a text/event-stream MIME type. The browser API (which is fairly similar 
to the WebSocket API) is called the EventSource API.
- __Comet/server push__: this is an umbrella term that includes 
both long-poll and HTTP streaming. Comet libraries usually 
support multiple techniques to try and maximize cross-browser and cross-server support.
- __WebSockets__: a transport layer built-on TCP that uses an HTTP friendly Upgrade handshake. 
Unlike TCP, which is a streaming transport, WebSockets is a message based transport: 
messages are delimited on the wire and are re-assembled in-full before delivery to the application. 
WebSocket connections are bi-directional, full-duplex and long-lived. 
After the initial handshake request/response, there is no transactional semantics 
and there is very little per message overhead. The client and server may 
send messages at any time and must handle message receipt asynchronously.
- __SPDY__: a Google initiated proposal to extend HTTP using a more efficient 
wire protocol but maintaining all HTTP semantics (request/response, cookies, encoding). 
SPDY introduces a new framing format (with length-prefixed frames) and specifies 
a way to layering HTTP request/response pairs onto the new framing layer. 
Headers can be compressed and new headers can be sent after the connection 
has been established. There are real world implementations of SPDY in browsers and servers.
- __HTTP 2.0__: has similar goals to SPDY: reduce HTTP latency and overhead while 
preserving HTTP semantics. The current draft is derived from SPDY and defines 
an upgrade handshake and data framing that is very similar the the WebSocket 
standard for handshake and framing. An alternate HTTP 2.0 draft proposal 
(httpbis-speed-mobility) actually uses WebSockets for the transport layer 
and adds the SPDY multiplexing and HTTP mapping as an WebSocket extension 
(WebSocket extensions are negotiated during the handshake).
- __WebRTC/CU-WebRTC__: proposals to allow peer-to-peer connectivity between browsers. 
This may enable lower average and maximum latency communication because as 
the underlying transport is SDP/datagram rather than TCP. 
This allows out-of-order delivery of packets/messages which avoids 
the TCP issue of latency spikes caused by dropped packets which delay 
delivery of all subsequent packets (to guarantee in-order delivery).
- __QUIC__: is an experimental protocol aimed at reducing web latency over that of TCP. 
On the surface, QUIC is very similar to TCP+TLS+SPDY implemented on UDP. 
QUIC provides multiplexing and flow control equivalent to HTTP/2, 
security equivalent to TLS, and connection semantics, reliability, 
and congestion control equivalentto TCP. Because TCP is implemented 
in operating system kernels, and middlebox firmware, making significant 
changes to TCP is next to impossible. However, since QUIC is built on top of UDP, 
it suffers from no such limitations. QUIC is designed and optimised for HTTP/2 semantics.
