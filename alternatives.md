
# Why not use Keybase?

Keybase is a pretty good platform, but is not decentralized or self-hostable. Once a username like "Joe" is taken nobody else in the world can use that name, I think that's kind of dumb, lots of people are called "Joe".

Instead KeyField promotes usernames only as a reminder / note to the humans. A full user identity is always identified unquestionably by their public verification key.

Since having multiple "Joe"s is still confusing to people (and usernames should be searchable on the same homeserver), the default homeserver implementation only allows one registered username. Clients can then choose how they present the user-friendly name discriminator, either by including the other homeserver's address, or by including a subset of the public key.

Eventually the plan is to be feature-complete against Keybase, including social proofs, file storage, and encrypted, shareable git. (This is a long way off right now though)

# Why not use the Matrix protocol?

## Matrix accounts are allocated by the homeserver

In the Matrix federation accounts are created attached to a homeserver, while this makes it easy to write the user id (username:server) on paper it results in accounts being immovable.

In KeyField the homeserver part of a user's identity can be changed at will and users can move existing identities between homeservers. If the user's previous homeserver is still a good faith actor, then anyone trying to contact the user with an outdated identity block will be correctly redirected to the user's new homeserver.

In the case that the user's homeserver shuts down permanently or turns evil the user can continue using existing chat channels by propogating their updated user identity via a new homeserver to all their friends.

## Voice, Video, File transfer and storage aren't a priority

Matrix wasn't designed for realtime and huge data sizes, with KeyField we hope to implement our identity and federation model in a way that can be expanded to both low-latency and large data use cases.

# Why not use Discord?

It's not end-to-end encrypted, does not allow self-hosting, and is vulnerable to censorship and the company's whim.

# How about the Signal protocol?

The protocol itself is cool, and clients could potentially implement a subset of it in KeyField DMs, since KeyField's homeservers don't actually care about what's inside the payload. (It's just encrypted bytes...)

The Signal app still has to come a long way before it meets our needs for anonymous accounts, censorship resistance, and support for non-chat data services.
