```k
syntax Int ::= "#DSGuard.acl" "[" Int "]" "[" Int "]" "[" Int "]" [function]
rule #DSGuard.acl[A][B][C] => #hashedLocation("Solidity", 2, A B C)

syntax Int ::= "#DSGuard.owner" [function]
rule #DSGuard.owner => 1

syntax Int ::= "#DSGuard.authority" [function]
rule #DSGuard.authority => 0

syntax Int ::= "#DSAuth.owner" [function]
rule #DSAuth.owner => 1

syntax Int ::= "#DSAuth.authority" [function]
rule #DSAuth.authority => 0
```
