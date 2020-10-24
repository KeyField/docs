
In order for User A to connect with user B, we need the following pieces of information:

 - B's latest user public main key
 - B's homeserver address and it's public key

# User Identities

KeyField users are composed of a few pieces of data:

 - Display username / name
 - User main keypair
   - Private key is shared between all of user's devices
   - Public key is regularly sent to homeserver for distribution
   - Changes on every modification to the user's device list or homeserver, devices keep all previous user main keys for decryption
 - User's devices
   - Each device has a static keypair created at init
   - Private and public friendly device name (public cannot be changed without re-generating device keypair)
 - User's current homeserver address and homeserver's public key
   - This is what allows the network to deliver content to the user

# User Profile

On the users current homeserver the following information must be kept up to date:

 - User display information: username, name, etc (signed by user main key)
 - User main public key
 - List of user device public keys + public names (signed by user main key)
 - Verification that this server is currently authoritative for the user (signed by latest user main key)

# Homeserver Identities

In order to establish inter-homeserver trust correctly each homeserver will have it's own rotating keypair.

This keypair is used not only to secure communications between users and homeservers but also to allow homeservers to verify the network relationships of other homeservers.
