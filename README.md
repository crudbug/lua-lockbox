# The Lua Lockbox
A collection of cryptographic primitives and protocols written in pure Lua.  This was written to provide cross-platform, tested reference implementations of many different cryptographic primitives.  These are written to be easy to read and easy to use, not for performance!

# Implemented Primitives

Digests:
* MD2
* MD4
* MD5
* RIPEMD128
* RIPEMD160
* SHA1
* SHA2-224
* SHA2-256

Message Authentication Codes (MACs):
* HMAC

Key Derivation Functions (KDFs):
* PBKDF2

Block Ciphers:
* DES
* AES128

Block Cipher Modes:
* ECB
* CBC
* PCBC
* CFB
* OFB
* CTR

Block Cipher Padding:
* Zero Padding
* ANSI X.923 Padding
* ISO/IEC 7816 Padding
* PKCS7 Padding (PKCS5-Compatible)

# Usage
To use these cryptographic primitives in a project, you'll likely have to modify Lockbox.lua to change the module search path.  All the primitives import this module to find the packages they require.  See RunTests.lua as an example.

The cryptographic primitives are designed to work on streams of bytes.  There are three data structures used to help with this:  Array(a Lua array of bytes), Stream(an iterator that returns a series of bytes), and Queue(a FIFO pipe of bytes).  See Array.lua, Stream.lua, and Queue.lua for more details. 

Most cryptographic primitives are designed in a builder-style pattern.  They usually have three functions: init, update, and finish.  All of these functions will return the primitive, so you can chain functions calls together.

* init() - resets the state of the primitive, so you can reuse it.
* update( byteStream ) - takes in a Stream of bytes, and updates its internal state.  This function can be called repeatedly, which effectively concatenates separate inputs.  If the primitive requires an IV, it is usually read as the first input provided to update.
* finish() - does any finalization necessary to finish generating output.

For examples of how to use the different primitives, read the test case files under tests.

# Security Concerns
Several weak or broken primitives are implemented in this library, for research or legacy reasons.  These should not be used under normal circumstances!  To restrict their usage, they have been marked as insecure, with the Lockbox.insecure() method.  This will cause a failed assertion when you attempt to import the module, unless you set Lockbox.ALLOW_INSECURE to true before the import.  For an example, see RunTests.lua.

# Planned Updates
* 3DES
* AES192 / AES256
* SHA3(Keccak)
* MD6
* BLAKE2s
* bcrypt / scrypt

