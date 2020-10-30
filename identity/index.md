
In order for User A to connect with user B, we need the following pieces of information:

 - B's latest user public main key
 - B's homeserver address and it's public key

# User Identities

KeyField users are composed of a few pieces of required data:

 - User main keypair: public signing key is used for unique identification
   - Private signingkey is shared between all of user's devices
   - Public verifykey is regularly sent to homeserver for distribution
   - Changes on every modification to the user's device list or homeserver
 - Username: string of printable word characters (just for the humans)
 - Chain of all previous main keypairs (verification of the newer pair by the previous)
 - List of user's devices
   - Each device has a static encryption key created at init
   - Public friendly device name (public name cannot be changed without re-generating device key, which should be easy)
 - User's current homeserver address and homeserver's public key
   - This is what allows the network to deliver content to the user

# User Profile

On the user's current homeserver all of the identity information should be available with a valid signature from the User, and if that User is authorized as a homeserver member then it will be signed by that homeserver as well.

# Homeserver Identities

In order to establish inter-homeserver trust correctly each homeserver will have it's own partial profile.

The homeserver's profile contains much of the same information as a User profile apart from the list of devices. The homeserver maintains a Username (like `system`) and it's own set of keys so that it can send messages and be verified / trusted by clients just like a User. System messages can be delivered from this homeserver "user" just like any other user. Clients are recommended not to allow blocking of the user's own homeserver.

The homeserver keypair is used not only to secure communications between users and homeservers but also to allow homeservers to verify the network relationships of other homeservers. Lookup of a public identity block can return the same structure for both users and homeservers, however homeservers will have no devices.
