# SHA256 algorithm implementation

Implemented completely from scratch and by myself, thus the solution is longer than some I have seen on the internet. But it works perfectly as it should.

I created a simple class:

```c++
class SHA256
{
public:
    string hash(const string &msg);

private:
    void prepareBlocks(const string &str);
    void blockPadding(bitset<SHA256_BLOCK_SIZE> &block);
    void processBlocks();
    void processBlock(const bitset<SHA256_BLOCK_SIZE> &block);

    void prepareWORDChunks(WORD *chunks, const bitset<SHA256_BLOCK_SIZE> &block);
    void prepareMsgSchedule(WORD *msgSchedules, const WORD *chunks);

    void computeHash(WORD *chunks, WORD *msgSchedules);

    WORD ROTR(WORD x, int n) const;
    WORD CH(WORD e, WORD f, WORD g) const;
    WORD MAJ(WORD a, WORD b, WORD c) const;

    string createOutputHash() const;

    vector<bitset<SHA256_BLOCK_SIZE>> blocks;
    bitset<SHA256_BIT_SIZE>
        hashBits;

    void printMessageBlock() const;

    // initial hash values (first 32 bits of the fractional parts of the square roots of the first 8 primes 2..19):
    WORD hVals[8] = {
        0x6a09e667,
        0xbb67ae85,
        0x3c6ef372,
        0xa54ff53a,
        0x510e527f,
        0x9b05688c,
        0x1f83d9ab,
        0x5be0cd19};

    // constants (first 32 bits of the fractional parts of the cube roots of the first 64 primes 2..311):
    WORD k[64] = {
        0x428a2f98, 0x71374491, 0xb5c0fbcf, 0xe9b5dba5, 0x3956c25b, 0x59f111f1, 0x923f82a4, 0xab1c5ed5,
        0xd807aa98, 0x12835b01, 0x243185be, 0x550c7dc3, 0x72be5d74, 0x80deb1fe, 0x9bdc06a7, 0xc19bf174,
        0xe49b69c1, 0xefbe4786, 0x0fc19dc6, 0x240ca1cc, 0x2de92c6f, 0x4a7484aa, 0x5cb0a9dc, 0x76f988da,
        0x983e5152, 0xa831c66d, 0xb00327c8, 0xbf597fc7, 0xc6e00bf3, 0xd5a79147, 0x06ca6351, 0x14292967,
        0x27b70a85, 0x2e1b2138, 0x4d2c6dfc, 0x53380d13, 0x650a7354, 0x766a0abb, 0x81c2c92e, 0x92722c85,
        0xa2bfe8a1, 0xa81a664b, 0xc24b8b70, 0xc76c51a3, 0xd192e819, 0xd6990624, 0xf40e3585, 0x106aa070,
        0x19a4c116, 0x1e376c08, 0x2748774c, 0x34b0bcb5, 0x391c0cb3, 0x4ed8aa4a, 0x5b9cca4f, 0x682e6ff3,
        0x748f82ee, 0x78a5636f, 0x84c87814, 0x8cc70208, 0x90befffa, 0xa4506ceb, 0xbef9a3f7, 0xc67178f2};

    int strBitSize;
};
```

which does exactly what you would expect - hashes the provided string with SHA256 algorithm.

### Example outputs

```
input string: abc
SHA256: ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad

input string: I'm now testing a very long message that will create a 1024 bit message block
SHA256: ad90ea1eff8fa44f8a8fa54bd0860bca7813eeea7838901e0884a9424f054b4b

input string: Olaf Dalach - Ph4sm4Solutions 12.23.2024
SHA256: a3b391c60e2f72afbaa7a6ecba464f2b2ce756d52dfd4b43ad7c7664c21713bb
```
