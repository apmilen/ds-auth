ds-auth
===

The `auth` module contains contract for the access control pattern used in several places throughout dappsys.

Think of `DSAuthorized` (alias: `DSAuth`) as analagous to `owned` and  the `auth` modifier as analagous to `owner_only`, except that
a `DSAuth` contract can be toggled into "Authority Mode", in which case the protected function works as if you had this line:

`assert(_authority.canCall(msg.sender, this, msg.sig));`

When in "authority mode", we expect `_authority` to be a `DSAuthority`:

```
contract DSAuthority {
    function canCall(address caller, address callee, bytes4 sig) returns (bool);
}
```

The `DSBasicAuthority` implements an authority that rejects by default and has a `(caller,callee,sig) => bool` whitelist.
Another example authority might be a `WhitelistAuthority`, which only cares about the `caller` argument.
