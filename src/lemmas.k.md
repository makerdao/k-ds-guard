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
syntax Int ::= "minUInt32"
             | "maxUInt32"

rule minUInt32      =>  0          [macro]
rule maxUInt32      =>  3 [macro]

rule #rangeUInt(32, X) => #range (minUInt32 <= X <= maxUInt32) [macro]

syntax TypedArg ::= #bytes4 ( Int )

rule #typeName( #bytes4( _ )) => "bytes4"

rule #lenOfHead( #bytes4( _ )) => 32

rule #isStaticType( #bytes4( _ )) => true

rule #enc(#bytes4( DATA )) => #buf(32, #getValue(#bytes4( DATA )))

rule #getValue(#bytes4( DATA )) => DATA
      requires minUInt32 <=Int DATA andBool DATA <=Int maxUInt32

rule #asWord( nthbyteof(keccak(V, W),  0, 32) )=> VW
```
