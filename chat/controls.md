
# Chat Channel control types (`io.keyfield.channel.*`)

These types include the various permissions nodes needed to grant granular permissions to users of a channel.

These range from serverside permission controls like kicks and bans to private channel metadata changes like the description or name.

## Roles (`io.keyfield.channel.role.*`)

Roles are the basis for grouping sets of permissions, the information the server has about a role is kept to the absolute minimum:

```toml
# uuid instead of a string to keep the role name private and mutable
uuid = 0b0c13eb-cdd8-472c-9834-eb4a8c649ba3
# order in which the permissions apply: higher numbers override lower ones
order = 4
# types this user can submit to the channel
pass_scopes = ['io.keyfield.chat.*', 'io.keyfield.channel.']
# types this user is denied from submitting
deny_scopes = ['io.keyfield.chat.voice.*']
# information about the role only decryptable with the channel private key:
private_payload = 'U29tZUJpbmFyeUNvbnRlbnQK'
```

Within the `private_payload` we recommend storing basic information like the name of the role, perhaps it's color and description, with more fields open to be added by different clients.
