# Sampling

## Summary
Sampling allows a server to access a language model like Claude through a connected MCP client. Instead of the server directly calling Claude, it asks the client to make the call on its behalf. This shifts the responsibility and cost of text generation from the server to the client.

* Allows the server to request the client to run any LLM
* Server can request a LLM call by using the 'create_message()' function on the context
* It reduces the amount of configuration we need to provide to the server
* Shifts the cost and config burden to the client.

## How sampling works?
The flow is straightforward:

* Server completes its work (like fetching Wikipedia articles)
* Server creates a prompt asking for text generation
* Server sends a sampling request to the client
* Client calls Claude with the provided prompt
* Client returns the generated text to the server
* Server uses the generated text in its response

## Benefits of Sampling
* Reduces server complexity: The server doesn't need to integrate with language models directly
* Shifts cost burden: The client pays for token usage, not the server
* No API keys needed: The server doesn't need credentials for Claude
* Perfect for public servers: You don't want a public server racking up AI costs for every user

## When to Use Sampling
Sampling is most valuable when building publicly accessible MCP servers. You don't want random users generating unlimited text at your expense. By using sampling, each client pays for their own AI usage while still benefiting from your server's functionality.

The technique essentially moves the AI integration complexity from your server to the client, which often already has the necessary connections and credentials in place.