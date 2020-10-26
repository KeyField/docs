
# Chat Channels

For messages to be organized and correctly federated by the homeservers, some information about chat channels must be shared with them.

Chat channel model (visible to server):

```toml
# fields permanent for a channel:
private = true # only users specified are allowed to read content
created = 1603732571507 # millis since epoch
created_by = 'userAkey'
created_on = 'serverAkey'
# fields which can change after creation:
readers = ['userAkey', 'userCkey', 'userFkey'] # receive content but cannot send anything to the channel (sends rejected by homeserver)
writers = ['userAkey', 'userCkey', 'userBkey'] # can send content (
owners = ['userAkey'] # modify the channel model itself
joinable = false
join_role = 'writers'
banned = ['userDkey']
```

In this example:
 - User A can manage the channel and users
 - User C can send and receive messages (a normal human user)
 - User B can only send messages (think of webhooks or announcement/notification use cases)
 - User F can only read messages (think of an announcement channel in a community)

Note: `read` does not mean messages are decipherable, it simply means that the homeserver will allow retrieval of the encrypted messages by that user, and send that content to other homeservers if it knows the other user is there.

If `private` is `false` then the homeserver allows retrieval of the channel content by any user.

If `joinable` is true, and `private` is false, then any user may request the homeserver to add or remove themselves from the chat channel, unless they are in `banned`.

If `private` is true and `joinable` is true, any writer AND reader of the channel can add new users to the channel as any combination of reader and writer. Otherwise only owners may add members.

## Direct Messages

DMs are just channels where both users are `writers` and neither is an `owner`.

This creates a channel where neither user can add other users and neither user can remove the other user from the channel.

Conversion from a DM to any other channel type is impossible intentionally so that no previous messages can be unintentionally leaked, and neither being owners is so that one user cannot delete or hide the entirety of the chat history against the other user's will.

## Caveats

The server does not store the private key for decrypting any of the chat channels.

If all readers of the chat channel simultaneously lose all their devices then the channel content is irrecoverable, even if it's public and open-join. Acceptable risk.

Using this model for public, open-join, open-writer channels requires at least one of the current members of the channel to re-encrypt the channel private key for the newly joined user.

Potential solution: join the homeserver user to the channel and have it emulate a passive user device. Originally I wanted to avoid the homeserver having full device capabilities, but perhaps 'bots' will make an appearance on KeyField just as they did on Discord.
