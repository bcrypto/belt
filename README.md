# Belt: encryption and integrity control algorithms

![](figs/belt-logo-small.png)

[![Build Status](https://travis-ci.org/bcrypto/belt.svg?branch=master)](https://travis-ci.org/bcrypto/belt)

## What is Belt?

Belt is a 128-bit block cipher developed in 2001 and standardized 6 years later in Belarus. 
The official standard STB 34.101.31 informally inherits the name Belt while the core 
block cipher tends to be called `belt-block`.

## BeltV1

Additionally to `belt-block`, the first edition of STB 34.101.31 defines the 
following cryptographic mechanisms:
- `belt-ecb` — encryption in the ECB (Electronic CodeBook) mode;
- `belt-cbc` — encryption in the CBC (Cipher Block Chaining) mode;
- `belt-cfb` — encryption in the CFB (Cipher FeedBack) mode;
- `belt-ctr` — encryption in the CTR (CounTeR) mode;
- `belt-mac` — data authentication through MAC (Message Authentication Codes).

The standardized encryption modes are 
[conventional](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation), 
they usually accompany every block cipher. The only caveat is that in `belt-ctr` a 
nonce (initialization vector) is encrypted before producing a sequence of 
counters from it. Thus, `belt-ctr` formally implements not CTR but the mode
[known as](https://web.cs.ucdavis.edu/~rogaway/papers/nonce.pdf) CTR2. 
This mode was originally proposed in the Soviet standard 
[GOST 28147-89](https://en.wikipedia.org/wiki/GOST_(block_cipher)).

In `belt-mac`, the [OMAC](https://eprint.iacr.org/2002/180.pdf) mode is instantiated. 
The way of instantiation, which avoids multiplication in GF(2^128),
slightly differs from the standard one.

## BeltV2

The second version of STB 34.101.31, released in 2011, additionally defines:
- `belt-dwp` — authenticated encryption with associated data 
   ([AEAD](https://en.wikipedia.org/wiki/Authenticated_encryption));
- `belt-kwp` — wrapping (encryption and authentication) of keys;
- `belt-hash` — hashing;
- `belt-keyrep` — deriving one key from another.

Here we use the updated names of mechanisms that will be set in the 
forthcoming version of STB 34.101.31.

In `belt-dwp`, the DWP mode of AEAD is implemented. Actually, `belt-dwp` 
continues `belt-ctr` additionally generating authentication tags over encrypted 
and associated (optional public) data. DWP is similar to the well-known 
[GCM](https://en.wikipedia.org/wiki/Galois/Counter_Mode) 
mode but provides greater security guarantees under nonce misuse (repeating).

The `belt-hash` algorithm implements hashing using the compression function 
`belt-compress`. To process two 128-bit blocks of data, `belt-compress` invokes 
`belt-block` 3 times. This means that the hash rate is approximately 2/3 of the 
encryption rate.

The `belt-keyrep` mechanism is based on `belt-compress`. The mechanism can be 
used for key updating (renewing) and diversification (generating a family of 
subordinate keys from a master key). 

In BeltV2, the [CTS](https://en.wikipedia.org/wiki/Ciphertext_stealing) 
(CipherText Srealing) technique is integrated into the `belt-ecb` and 
`belt-cbc` modes. This allows encryption to be extended to messages with 
a non-integral number of blocks.

## BeltV3

The third version of STB 34.101.31 was released in 2020. It will be 
operational from September 2021.

BeltV3 additionally defines:
- `belt-wblock` — wide-block encryption;
- `belt-che` — AEAD in the CHE (Counter-Hash-Encrypt) mode;
- `belt-bde` — block-wise disk encryption;
- `belt-sde` — sector-wise disk encryption;
- `belt-fmt` — format preserving encryption;
- quotas for encryption keys.

The `belt-wblock` mechanism is a core of `belt-kwp`. This mechanism has been
singled out since it has an independent significance allowing to encrypt a wide
data block (for example, 4 Kbytes long) so that each byte of the block affects
all other bytes. In `belt-wblock`, the theory of 
[XS-circuits](https://eprint.iacr.org/2018/592.pdf) is applied. 

The [CHE](https://eprint.iacr.org/2020/331.pdf) mode is a slightly lightweight 
variant of DWP that saves one invocation of `belt-block`. This is achieved by 
loss of compatibility with `belt-ctr`.

A special feature of the DWP and CHE modes is the permission to
issue intermediate authentication tags. This facilitates the processing of 
large data streams. 

The `belt-bde` mechanism implements the 
[XTS](https://en.wikipedia.org/wiki/Disk_encryption_theory#XTS) disk encryption mode 
in which one encryption key is dropped. The drawback of `belt-bde` is that each block 
in each disk sector is processed separately, without affecting other blocks. 
In `belt-sde`, this drawback is overcome by switching to `belt-wblock`.

Using `belt-fmt`, one can encrypt a string in a numeric alphabet preserving 
both the alphabet and the length of the string. A 6-round 
[alternating numeric Feistel network](https://eprint.iacr.org/2010/301.pdf) 
is implemented.

Quotas for encryption keys regulate amounts of data that can be safely
processed using a single key without changing it. Quotas are determined following the 
[Provable Security](https://en.wikipedia.org/wiki/Provable_security) paradigm.

## What is this repo?

In this repo, we are discussing Belt version 3 and higher.

The latest releases of Belt can be found at 
[Releases](https://github.com/bcrypto/belt/releases).

Comments and proposals are processed at 
[Issues](https://github.com/bcrypto/belt/issues). 

