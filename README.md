# ROBLOX Webchat Documentation
A documentation of ROBLOX's webchat system written and researched by Stormersoul.

## Before we begin...
Please note this information **may not be 100% accurate**!
I cannot emphasize this enough. If you find any errors, please message me on ROBLOX (Stormersoul).

### What happens when you send a message
When you send a message through ROBLOX's chat system, it goes through 

### How ROBLOX's chat checks for messages
ROBLOX's chat system checks for (unread) messages every time you open the chatbox.

`https://chat.roblox.com/v1.0/get-unread-conversation-count` is the URL the system uses to check for how many messages are available.
If you have someone send you a message, you can see the number changes.

#### ROBLOX makes a request for the actual message(s) as well
`GET /v1.0/get-messages?conversationId=IDGOESHERE&pageSize=PAGESIZE (DEFAULT 30?) HTTP/1.1`

Please note that conversationId doesn't take userIds as parameters.
I currently don't know what pageSize is, or how conversationIds are found.

### How sending messages works
When you send a message, ROBLOX sends out **(3?)** different requests to their domains and these domains are
`OPTIONS /v1.0/send-message HTTP/1.1`,
`POST /v1.0/send-message HTTP/1.1`,
and once again, it checks for messages at `GET /v1.0/get-messages?conversationId=IDGOESHERE&pageSize=PAGESIZE HTTP/1.1`.
