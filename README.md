# CTF (Capture The Flag)
Hunt for bugs and experiment with exploitation in practice: https://ctf.hacker101.com/ctf

## File uploads
* Mitigation:
  - do not host on the same domain.

## Null Terminators Bugs
* definition:
* exploit:

## Password Storage:
* Use BCrypt when storing passwords on the server
  - SCrypt is a less battle-tested but still great solution. It's designed to not only be computationally expensive, but to use lots of RAM, making it hard to parallellze.
* Do not use a bare hash (e.g. MD5, SHA1)
  - use PBKDF1 and 2
  - salting (use different salt for different hash, which would achieve the goal of user hashes being unique)
    - the goal is to break rainbow table and prevent hashes from repeating.
* If not BCrypt: use SHA256 in PBKDF2 using per-user salt values with at least 10000 rounds

## Crypto:
* One-time pads
* Types of ciphers:
  - Symmetric:
    - Stream:
    - Block: AES, DES, 3DES, Twofish
  - Asymmetric: RSA
* Block cipher modes:
  - ECB (Electronic Codebook)
  - CBC (Cipher Block Chaining)
* Hashes
* MACs (Message Authentication Codes)
  - HMAC (Hash-based MAC): HMAC(key, message) = hash(key + hash(key + message))

## Crypto Attacks:
* Stream cipher key reuse: for RC4 stream ciphers
  - Mitigation: XORed with a nonce prior to encryption/decryption
* ECB block reordering
* ECB decryption
  - Mitigation: encrypt your data, then append a MAC of the encrypted data. (Note. Never MAC-then encrypt, which introduces a multitude of problems, such as padding oracles)
* Padding: PKCS#7, look at the last byte of the last block and see how many padding bytes there are, then check that those all match.
* Padding oracles: When you have CBC-mode data that is padded with PKCS#7. If the server behaves differently when decrypting improperly padded data than properly padded data, this is an oracle -- you can send it data and know whether or not it's correctly padded.
* Hash length extension:
  - Mitigation: When you are hashing a secret with data from the user, you should always use HMAC.

## Reference: https://www.hacker101.com
