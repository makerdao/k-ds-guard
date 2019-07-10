`DSGuard` spec

```act
behaviour owner of DSGuard
interface owner()

types
  Owner : address

storage
  owner |-> Owner

iff
  VCallValue == 0

returns Owner
```

```act
behaviour authority of DSGuard
interface authority()

types
  Authority : address

storage
  authority |-> Authority

iff
  VCallValue == 0

returns Authority
```

```act
behaviour setOwner of DSGuard
interface setOwner(address usr)

types
  Owner : address
  Authority : address

storage
  owner |-> CALLER_ID => usr
  authority |-> Authority

iff
  VCallValue == 0
  (CALLER_ID == Owner) or (CALLER_ID == ACCT_ID)

if
  (CALLER_ID == Owner) or (CALLER_ID == ACCT_ID) or (Authority == 0)
```

```act
behaviour setOwner of DSGuard
interface setOwner(address usr)

types
  Owner : address
  Authority : address

storage
  owner |-> Owner => usr
  authority |-> Authority

iff
  VCallValue == 0

if
  (Owner == CALLER_ID) or (ACCT_ID == CALLER_ID)
```

```act
behaviour setAuthority of DSGuard
interface setAuthority(address usr)

types
  Owner : address
  Authority : address

storage
  authority |-> 0 => usr

iff
  VCallValue == 0

if
  CALLER_ID == Owner
  ACCT_ID =/= CALLER_ID
```

```act
behaviour canCall of DSGuard
interface canCall(address src, address dst, bytes4 sig)



returns 1
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
