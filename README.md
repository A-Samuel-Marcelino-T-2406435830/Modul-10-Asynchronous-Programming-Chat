# Module 10 - Asynchronous Programming

![2-1](assets/images/2-1.png)

Here, the server infinitely loops to receive messages from clients. When a new client connects, the server doesn't stop its process just to talk to that specific client. Instead, it spawns an async block that handles the connection and gives it to the Tokio executor. This allows the server to go back and listen for new connections while the said client's task is bing handles asynchronously. 

Before making the async task, the server first creates a single channel: `let (bcast_tx, _) = channel(16);` which is going to be used by every client, since the server hands a clone of this channel to the async task. When a client connects to the server, the client is automatically subcribed to that channel: `bcast_tx.subscribe()`. So, when 3 clients connects to the server, there are 3 subscribers connected to this broadcast channel.

When any client sends a message to the server, the message is duplicated and every copy of it is sent to every active subscriber. This means, this specific client's message will also appear in other active clients too, in the form of `From server: [the message]`.