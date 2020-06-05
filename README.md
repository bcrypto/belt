# Belt: encryption and integrity control algorithms

![](figs/belt-logo-small.png)

[![Build Status](https://travis-ci.org/bcrypto/belt.svg?branch=master)](https://travis-ci.org/bcrypto/belt)

## What is Belt?

Belt is a 128-bit block cipher developed in 2001 and standardized 6 years later in Belarus. 
The official standard STB 34.101.31 borrowed the name Belt while the core 
block cipher has tended to be called `belt-block`.

Additionally to `belt-block`, the 1st edition of STB 34.101.31 determines the 
following cryptographic mechanisms:
- `belt-ecb` - encryption in the ECB (Electronic CodeBook) mode;
- `belt-cbc` - encryption in the CBC (Cipher Block Chaining) mode;
- `belt-cfb` - encryption in the CFB (Cipher FeedBack) mode;
- `belt-ctr` - encryption in the CTR (CounTeR) mode;
- `belt-mac` - data authentication through MAC (Message Authentication Codes).
