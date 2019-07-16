
# `DSGuard` and `DSAuth` spec

## `DSAuth` spec

```act
behaviour isAuthorized of DSAuth
interface isAuthorized(address src, bytes4 sig) internal

types
  Owner : address
  Authority : address

storage
  owner |-> Owner
  authority |-> 0

iff
  VCallValue == 0
  (Owner == src) or (ACCT_ID == src)

if
  (Owner == src) or (ACCT_ID == src) or (Authority == 0)

returns true
```

*Status:* Not Accepted

```act
behaviour setOwner of DSAuth
interface setOwner(address usr)

types
  Owner : address
  Authority : address

storage
  owner |-> Owner => usr
  authority |-> Authority

iff
  VCallValue == 0
  (CALLER_ID == Owner) or (CALLER_ID == ACCT_ID)

if
  (CALLER_ID == Owner) or (CALLER_ID == ACCT_ID) or (Authority == 0)
```

```act
behaviour setOwner-self of DSAuth
interface setOwner(address usr)

types
  Owner : address
  Authority : address

storage
  owner |-> Owner => usr
  authority |-> 0

iff
  VCallValue == 0

if
  CALLER_ID == ACCT_ID
```

```act
behaviour setOwner-authority of DSAuth
interface setOwner(address usr)

types
  Owner : address
  Authority : address

storage
  owner |-> Owner => usr
  authority |-> 0

iff
  VCallValue == 0
  Authority =/= 0

if
  Authority == 0
```

*Status:* WIP

```act
behaviour owner of DSAuth
interface owner()

types
  Owner : address

storage
  owner |-> Owner

iff
  VCallValue == 0

returns Owner
```
*Status:* Pass/Fail accepted

```act
behaviour authority of DSAuth
interface authority()

types
  Authority : address

storage
  authority |-> Authority

iff
  VCallValue == 0

returns Authority
```
*Status:* Pass/Fail accepted

```act
behaviour setAuthority of DSAuth
interface setAuthority(address usr)

types
  Owner : address
  Authority : address

storage
  owner |-> Owner
  authority |-> 0 => usr

iff
  VCallValue == 0
  (CALLER_ID == Owner) or (CALLER_ID == ACCT_ID)

if
  (CALLER_ID == Owner) or (CALLER_ID == ACCT_ID) or (Authority == 0)
```

*Status:* Pass/Fail accepted

## `DSGuard` spec - in progress

```act
behaviour canCall of DSGuard
interface canCall(address src, address dst, bytes4 sig)

types
  Can : uint256

storage
  acl[#asWord(src)][#asWord(dst)][sig] |-> Can

if
  src == dst

returns Can
```

```act
behaviour permit-auth of DSGuard
interface permit(bytes32 src, bytes32 dst, bytes32 sig)

types
  Approval  : bool
  Owner     : address
  Authority : address
storage
  acl[src][dst][sig] |-> Approval => true
  owner |-> Owner

iff
  VCallValue == 0

if
  CALLER_ID == Owner
```


```act
behaviour permit-address-owner of DSGuard
interface permit(address src, address dst, bytes32 sig)

types
  Approval  : bool
  Owner     : address
  Authority : address

storage
  acl[src][dst][sig] |-> Approval => true
  owner |-> Owner
  authority |-> Authority

iff
  VCallValue == 0

if
  CALLER_ID == Owner
  ACCT_ID =/= CALLER_ID
```
