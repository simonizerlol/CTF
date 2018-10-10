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

## Crypto Wrap Up:
* ECB mode: If you have control of data being encrypted in some way, then repetition is the easiest way to determine if ECBB mode is in use. In ECBB, each block is encrypted independently.
  - Detection: If you put 47 'A' characters in a row into a payload that's encrypted, this will guarantee repetition if a block size of 16 bytes (128 bits) or less is used. No matter how many character are before or after those 47 As, you will always have enough to fill at least two blocks.
  - Determining block size: Pick a character somewhere in the middle of your repeated As and turn it into a 'B'. You will see some number of bytes change, or a gap in the repetition. That number of bytes is your block size.
  - Determining data offset: The number of characters you can change at the beginning of your payload is the offset from the beginning of the whole encrypted string. The number of characters you can change at the end of your payload is the offset from the end.
* Padding oracles:
  1. Only come into play with CBC mode, due to the XOR chaining nature of the mode.
  2. You must be abble to manipulate the data. IF there's an HMAC then this is a no-go.
  - Detection: run through all 256 possibilities for the last byte of the second-to-last block. There should be no more than two values that give you different errors; everything else should be padding errors.
  - Exploitation: padbuster (https://github.com/GDSSecurity/PadBuster)
* General advice:
  - Do not design your own crpto protocols unless you are with thousand experts
  - If you have data in flight, use TLS (what used to be SSL)
  - If you have data at rest, use PGP
  - Do not Mac-then-Encrypt
  - Do not use hashes instead of MACs (you enable hash extension attacks)
  - Do not reuse key-IV or key-nonce pairs (you open yourself up to a multitude of issues)
  - Do not use ECB mode
* Cryptopals challenges: https://cryptopals.com/

## Threat Modeling:
* Definition: a process by which you can determine what threats are important to an application and find points where defenses may be lacking.
* Goals:
  - Achieve better coverage in testing (test all the entrypoints)
  - Find more interesting and valuable bugs (have a testing game plan before you start)
  - Waste less time on dead ends (Eliminate entire classes of vulnerabilities before you even start testing)
* “Heavy-weight” threat modeling (for large companies and internal security teams):
  1. Decomposition: The modeler will document each component of the application and its infrastructure, then develop data flow diagrams that show how these components interact. Additionally, privilege boundaries are identified, to ensure that proper controls are in place for any data crossing these boundaries.
  2. Threat Determination: Develop threats for each portion of the application, e.g. "attacker may be able to access administration features." You link each of these threats to the components that would be affected in the case of such an attack. And you rank them by means of an objective measure of severity.
  3. Countermeasures.: Determine and document any countermeasures currently in place to prevent an attack, along with identifying new locations where countermeasures may be installed to prevent threats you have assessed.
* Light-weight threat modeling (for bug bounty hunters and other security consultants):
  1. Enumerate Entrypoints: Enable Burp Proxy and then use every function of the application which you can find, for every access level you have.
  2. Document Target Assets: Think through and write down every asset in which an attacker may be interested, along with the business impact of its compromise.
    - User PII and passwords
    - Admin panel access
    - Transaction histories
    - Source code
    - Database credentials
  3. Develop Game Plan: Rank the entrypoints in order of perceived value.
  4. Restrict Yourself: Handle the most applications in under an hour. Do not overthink it and reconsider your approach if you are going over this.
* Resources:
  - Example HackerOne threat model(https://www.hacker101.com/resources/hackerone_threat_model)
  - OWASP Threat Modeling Guide(https://www.owasp.org/index.php/Application_Threat_Modeling)


    ```


## Reference: https://www.hacker101.com
