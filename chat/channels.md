
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
readers = ['userFkey'] # receive content but cannot send anything to the channel (sends rejected by homeserver)
writers = ['userCkey', 'userBkey'] # receive and send content
owners = ['userAkey'] # modify the channel model itself
joinable = false
join_role = writers
```

Note: `read` does not mean messages are decipherable, it simply means that the homeserver will allow retrieval of the encrypted messages by that user, and send that content to other homeservers if it knows the other user is there.

If `private` is `false` then the homeserver allows retrieval of the channel content by any user.

If `joinable` is true, and `private` is false, then any user may request the homeserver to add or remove themselves from the chat channel, they will be granted the `join_role`.

If `private` is true and `joinable` is true, any writer of the channel can add new users to the channel `join_role`. Otherwise only owners may add members.

## Caveats

The server does not store the private key for decrypting any of the chat channels.

If all readers of the chat channel simultaneously lose all their devices then the channel content is irrecoverable, even if it's public and open-join. Acceptable risk.

Using this model for public, open-join, open-writer channels requires at least one of the current members of the channel to re-encrypt the channel private key for the newly joined user.

Potential solution: join the homeserver user to the channel and have it emulate a passive user device.
