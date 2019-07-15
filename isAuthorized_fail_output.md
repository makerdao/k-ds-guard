Accepted but with very few steps. Solidity tracking says it only reaches `src == address(this)` in the function.

During debug EVM and Stack show:
```
  0448  018e  JUMPDEST                    00  ACCT_ID
  0449  018f  PUSH1 00                    01  0
  044b  0190  ADDRESS
  044c  0191  PUSH20 fffffffffffff...
  0461  0192  AND
  0462  0193  DUP4
```

There are bytecodes above `JUMPDEST` but `p` won't reach them. There are bytecodes after `DUP4` but `n` won't reach them.
