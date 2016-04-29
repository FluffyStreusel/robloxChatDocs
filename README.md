# ROBLOX Webchat Documentation
A documentation of ROBLOX's webchat system written and researched by Stormersoul.

## Before we begin...
Please note this information **may not be 100% accurate**!
I cannot emphasize this enough. If you find any errors, please message me on ROBLOX (Stormersoul).

# On the sending end of your chats
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

**These requests can be seen in Fiddler, and when you view the request containing `POST /v1.0/send-message HTTP/1.1`, (to [chat.roblox.com](https://chat.roblox.com).)**

## A little trick (Can be used with WireShark with some tinkering)
ROBLOX's chat sends **JSON** code to (chat.roblox.com?) when you send a message and if viewed from Fiddler, you can see the message sent.

### Vulnerability tips
When ROBLOX sends the message to it's server **(chat.roblox.com)**, it does sends cookies from ROBLOX's main site.

### Miscellaneous
When messages are intercepted, they may refer to where your message was sent from (http://www.roblox.com/home, etc.).

# When you read your messages
ROBLOX's systems automagically mark your messages as read as soon as you see them, by making an outbound request as follows;
`POST /v1.0/mark-as-read HTTP/1.1`

There are two parameters in the JSON code for this request and they are StatusMessage and Success.
**Success** should always return `True`, unless you're having internet issues (or something else I don't know yet).
**StatusMessage** should return `Successfully marked the messages as read` if successful. If an error, I'm not sure yet.

For this request above (`POST /v1.0/mark-as-read HTTP/1.1`), you can send two different methods.
These methods are `POST` and `OPTIONS`.
ROBLOX sends two outgoing requests to `/v1.0/mark-as-read`. After this delicate dance of marking-as-read, the message returns to ROBLOX.com as read.

`OPTIONS /v1.0/mark-as-read HTTP/1.1` and `POST /v1.0/mark-as-read HTTP/1.1` are these two outgoing requests.
### Other Miscellaneous Discoveries
The chat domain does not have a *robots.txt*.

## When you create a party
ROBLOX sends one outgoing request that says `OPTIONS /v1.0/party/create HTTP/1.1`. This creates the party, but I don't know the parameters at this moment.

Secondly, it creates the party based on a second request that contains the conversationId and the invited user's Id.

Thirdly, a request is sent that says `GET /v1.0/party/get-current HTTP/1.1`. This contains a huge group of information readable in JSON.
