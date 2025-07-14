# Streamable HTTP Transport.

The streamable HTTP transport enables MCP clients to connect to remotely hosted servers over HTTP connections. Unlike the standard I/O transport that requires both client and server on the same machine, this transport opens up possibilities for public MCP servers that anyone can access.

However, there's an important caveat: some configuration settings can significantly limit your MCP server's functionality. If your application works perfectly with standard I/O transport locally but breaks when deployed with HTTP transport, this is likely the culprit.

## Configuration Settings That Matter

Two key settings control how the streamable HTTP transport behaves:
* stateless_http - Controls connection state management
* json_response - Controls response format handling


Revisiting the 4 type of communications in MCP which we saw in stdio,

## 4 type of communications
* Initial Request from Client to Server
* Response from Server to Client
* Initial Request from server to Client
* Response from Client to Server

In case of http communication, Http severs cannot easilty initiate communication with a client. Because clients don't have a known URL (The is due to the way http works). So in traditional http its very challenging for the server to request down to the client.

Hence in HTTP world some messsage types are tough to implement with normal HTTP.

* sampling requests
* listing roots
* progress notifications
* logging notifications

![alt text](different_comms.png)


## MCP Message Types Affected
This HTTP limitation impacts specific MCP communication patterns. The following message types become difficult to implement with plain HTTP:

Server-initiated requests: Create Message requests, List Roots requests
Notifications: Progress notifications, Logging notifications, Initialized notifications, Cancelled notifications

These are exactly the features that break when you enable the restrictive HTTP settings. Progress bars disappear, logging stops working, and server-initiated sampling requests fail.

## The Streamable HTTP Solution

The streamable HTTP transport does provide a clever solution to work around HTTP's limitations, but it comes with trade-offs. When you're forced to use stateless_http=True or json_response=True, you're essentially telling the transport to operate within HTTP's constraints rather than working around them.

Understanding these limitations helps you make informed decisions about:

* Which transport to use for different deployment scenarios
* How to design your MCP server to gracefully handle HTTP constraints
* When to accept reduced functionality for the benefits of remote hosting

The key is knowing that these restrictions exist and planning your MCP server architecture accordingly. If your application heavily relies on server-initiated requests or real-time notifications, you may need to reconsider your transport choice or implement alternative communication patterns.