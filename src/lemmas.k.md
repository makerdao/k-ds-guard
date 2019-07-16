```k
syntax Int ::= "Mask12_32" [function]

rule Mask12_32 => 115792089237316195423570985007226406215939081747436879206741300988257197096960 [macro]

rule Mask12_32 &Int A => 0
  requires #rangeAddress(A)

rule X |Int 0 => X

rule chop(A |Int B) => A |Int B
  requires #rangeUInt(256, A)
  andBool #rangeUInt(256, B)


// custom ones:
rule #asWord(#padToWidth(32, #asByteStack(#unsigned(X)))) => #unsigned(X)
  requires #rangeSInt(256, X)
```
