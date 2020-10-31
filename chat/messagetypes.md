
# Chat Message Types (`io.keyfield.chat.*`)

Clearly modern chat channels don't just send strings of text, so we need support for media! (And some other message types)

## Base Message Type (`io.keyfield.chat.message`)

A base message just says which ChatChannel it belongs to and which category it's in. Homeservers reject contentful (non-status) messages from clients who don't have write permission for a channel, and reject all messages tagged for a channel the user is not a member of.

These two pieces of information are all the server needs to know about a message, it uses the ChatChannel model to federate this message across any nonlocal homeservers of users.

When the server receives a message it will also apply a timestamp of receipt, which is what is used for sorting the messages in time for clients to retrieve on-demand. These timestamps are only guaranteed to be precise to 1 second, more precise timestamps should be embedded in the encrypted message.

If a client notices that timestamps within the encrypted portion of the message are majorly different from the server's timestamps, there might be severe latency problems in the federation. (Thinking about what happens when a homeserver goes down for maintenance.)

## Status Messages (`io.keyfield.chat.status`)

Status messages are only kept on the server for a short duration, they are intended to represent states of being online, away, typing, voice connected, offline, busy, etc...

The actual status types aren't known to the server, the clients determine which status types are used and how, so some clients might support typing status for example and some might not, but the same information is relayed.

Extremely large ChatChannels might disable status type messages to avoid flooding clients with useless information. Optionally clients can also ask their homeservers not to forward status messages for situations like data restrictions.

## Printable Message Type (`io.keyfield.chat.message.text`)

A standard message composed of text. Hoorah.

## Multimedia Message Type (`io.keyfield.chat.message.multimedia`)

This is a multipart message that contains an optional text component along with the media. (Any generic data, really.)

The reason this is separate from Printable is so that a client can ask a server not to send multimedia message types immediately in data restricted situations.

## Initialize Voice (`io.keyfield.chat.voice.init`)

Used to set up peer-to-peer voice or video, or initiate a VoiceTunnel session. Clients need to add enough data for all other ChatChannel members to connect. (Unless their client doesn't support it)

This message type is normally only sent after the user constructing the VoiceTunnel or p2p socket has done so, otherwise receivers of the message will be unable to connect.
