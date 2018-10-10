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

### Reference: https://www.hacker101.com
