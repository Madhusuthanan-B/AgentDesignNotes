# MCP Transports

MCP clients and servers communicate by exchanging JSON messages, but how do these messages actually get transmitted? The communication channel used is called a transport, and there are several ways to implement this - from HTTP requests to WebSockets to even writing JSON on a postcard (though that last one isn't recommended for production use).

## Stdio
A commonly used transport when developming MCP client or server in the beginning.

* Client launches the MCP server as a subprocess
* Client sends messages to the MCP server using the server's 'stdin'
* Server responds by writing to 'stdout'
* Critical: Either the server or the client can send a message at any time.
* Only appropriate when the client and server are running on the same machine.


## MCP Connection Sequence
1) Initialize Request - Client sends this first
2) Initialize Result - Server responds with capabilities
3) Initialized Notification - Client confirms (no response expected)

## Why understanding stdio Matters
Understanding stdio transport is crucial because it represents the "ideal" case where bidirectional communication is seamless. When we move to other transports like HTTP, we'll encounter limitations where the server cannot always initiate requests to the client. The stdio transport serves as our baseline for understanding what full MCP communication looks like before we tackle the constraints of other transport methods.

For development and testing, stdio transport is perfect. For production deployments where client and server need to run on different machines, you'll need to consider other transport options with their own trade-offs.