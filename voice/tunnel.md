
# Voice Channels

This document covers server-relayed voice channels, there will likely be p2p voice channels too, built on the same method as video.

The basic building block here is a `VoiceTunnel`:

```toml
# a hash of the tunnel's symmetric encryption key
tunnelID = "BlargSomeRandomBase64Digits="
# reference to the ChatChannel that determines who is allowed to connect / read / write
auth_chatchannel = "BleckMoreRandomBase64Digits="
connections = {
  userA = {
    type = "direct",
    # more connection info depending on transport method
  },
  userB = {
    type = "proxy",
    proxy = {address = "serverB", public = "ServerPublicKeyBytes="},
    # more connection support info
  }
}
```

Symmetric keys are ephemeral along with the tunnel, they are constructed on-demand:

## Starting a Voice Tunnel

A voice tunnel is constructed by a homeserver user, it is a type of chat channel, however the protocol will likely change from HTTP. Final transport TBD.

When a voice tunnel is declared to the server it needs to know who can write, who can read, and who can administrate, just like a ChatChannel.

On tunnel construction by User A, User A encrypts the voice shared symmetric key with the shared ChatChannel asymmetric key, and then sends a InitializeVoice message with that voice encryption key to the chat channel. A hash of the symmetric voice data key is sent to the server to identify the tunnel: the tunnel ID.

> Note for clients: recommended default behaviour is to send notifications on InitializeVoice in DMs, and do not send notifications for groups

Now User B sees (or gets a notification, depending on channel type) that User A started a voice tunnel, and User B wishes to join.

User B can either ask his homeserver to relay voice data for that tunnel, or can connect to the existing tunnel on homeserver A.

## Users all connect to one homeserver

In order for User B to connect to a tunnel on server A (which they are not a member of) User B must present both the tunnel ID and specify the associated ChatChannel to prove User B has access.

The tunnel will now forward all data received to all connected users.

## Users connect to separate homeservers

Let's say User B does not want ISP C to know that they are communicating with people on server A (or it's blocked).

User B can ask homeserver B to connect as a proxy to homeserver A's tunnel.

Homeserver B now asks homeserver A to forward voice data from the tunnel to itself, which is proven by a signature from User B asking for a proxy connection which includes the tunnel ID and name of homeserver B for the proxy destination.

# VoiceTunnel server implementation

Obviously running encrypted voice packet data over HTTP methods is dumb, so the VoiceTunnel relay runs separately. I'll call it vtserver A for the tunnel server on homeserver A.

Once homeserver A accepts the creation of a voice tunnel, it will convey the details of the tunnel to vtserver A. Most likely this can be done by having them both share the database connection, since vtserver will need the server's private keypair as well for transport of data between client <-> server.

All vtserver now has to do is wait for a connection to be opened by User A, in which vtserver A gets the tunnel ID and sees that User A is authorized, so the connection opens and voice data is relayed.

For vtserver B, User B wishes for it to relay voice to vtserver A, so vtserver passes a proof-of-relay request to vtserver A, and given that it's a correct tunnel ID and good signature from User B and homeserver B, will now copy voice data to the new connection on vtserver B, which then forwards again to User B.
