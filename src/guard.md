`DSAuth` spec

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
  Authority =/= 0
```

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

```act
behaviour setAuthority of DSAuth
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
