# strobe
> [Fed Up Getting Shattered and Log Jammed? A New Generation of Crypto Is Coming](https://www.youtube.com/watch?v=bTGLO4obxco)

Recent paper by JP Aumasson discusses retroactively reducing the round requirements for block ciphers, [Too Much Crypto](https://eprint.iacr.org/2019/1492)

Team Keccak released a [paper on Sponge Constructions](https://keccak.team/sponge_duplex.html), which consist of a mode of operation, based on a fixed-length permutation (or transformation) and on a padding rule, which builds a function mapping variable-length input to variable-length output...It operates on a finite state by iteratively applying the inner permutation to it, interleaved with the entry of input or the retrieval of output.

Applying the sponge construction with a random permutation results in a so-called random sponge. It turns out that a random sponge is as strong as a random oracle, except for the effects induced by the finite memory. Random sponges are thus well suited to replace random oracles for expressing security claims.

Additionally, the sponge construction and its sister construction, called the duplex construction, can be used to implement a wide spectrum of symmetric cryptography functions. This includes hashing, reseedable pseudo random bit sequence generation, key derivation, encryption, message authentication code (MAC) computation and authenticated encryption. The fundamental cryptographic primitive underlying all this is a fixed-length permutation. These permutation-based modes form efficient alternatives for the current block-cipher dominated cryptographic practice. On top of its conceptual elegance, a permutation has the advantages that it does not have a key schedule and that its inverse does not need to be implemented or efficient.

## BLINKER

[Beyond Modes: Building a Secure Record Protocol froma Cryptographic Sponge Permutation](https://eprint.iacr.org/2013/772.pdf) introduces BLINKER, a lightweight cryptographic suite and record protocol built from a single permutation.

> * the resulting record protocol is secure against a two-channel synchronization attack while also having a significantly smaller implementation footprint*

examines the SpongeWrap authenticated encryption mode and expand its padding mechanism to offer explicit do-main separation and enhanced security for our specific requirements: shared secret half-duplex keying, encryption, and a MAC-and-continue mode.

## <a href= "https://strobe.sourceforge.io/">Strobe</a>

## <a href = "http://noiseprotocol.org/">Noise</a>

## <a href = "https://cryptologie.net/article/408/noisestrobedisco/">Noise + Strobe = Disco</a>