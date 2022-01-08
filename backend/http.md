# HTTP
Hypertext Transfer Protocol (often abbreviated to HTTP) is a communications protocol. It is used to send and receive webpages and files on the internet.
### Client / Server
* **Client** - Browser, Python, or JS app
* **Server** - HTTP Web Server

### HTTP Request
* URL
* Method Type
* Headers
* Body

### HTTP Response
* Status Code
* Headers
* Body

### HTTP 1.0
* New TCP connection with every request
* Slow
* Buffering

### HTTP 1.1
* Persisted TCP connection
* Low latency
* Streaming with chunked tranfer
* Pipelining (disabled by default)

### HTTP/2
* Compression
* Multiplexing - Client will join multiple requests into one TCP request
* Server Push
* SPDY
* Secure by default
* Protocol negotiation during TLS (NPN/ALPN)