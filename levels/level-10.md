# Bandit Level 10 → 11

**Concepts:** recognising Base64, decoding with `base64 -d`, encoding is not encryption

## Goal
The password is in `data.txt`, but the file content is Base64-encoded.

## What I tried
I started with `ls -la` to see the home folder and the file permissions, then read the file with `cat`. It was a single line of letters and numbers ending in `==`, which is a typical Base64 look.

Then I read `man base64`. The manual explains the flags directly, and one of them decodes. Simple and fast.

<details>
<summary>🔓 Show solution</summary>

```bash
base64 -d data.txt
# -d = decode; prints a short sentence that contains the password
```

</details>

## Password check
SHA-256 of the password for `bandit11` (the decoded password, not the Base64 text):

```
60735251149ec5764e1c5164580ed83fd1ebf6143a1c040551388282dbf4a6fc
```

## What I learned
- **How to recognise Base64:** only `A-Z a-z 0-9 + /`, length usually a multiple of 4, often padded with one or two `=` at the end.
- **Encoding is not encryption.** Base64 has no key; anyone can reverse it. It exists to carry binary data through text-only channels (email, JSON, HTTP headers), not to hide anything.
- **`man` first works.** The answer was in the manual in under a minute, no searching online.
- `base64` (encode) and `base64 -d` (decode) can also read from a pipe: `echo 'aGVsbG8=' | base64 -d`.
- **Where this shows up in real security work:** attackers often hide PowerShell commands in Base64 (`powershell -EncodedCommand ...`). Decoding them is a routine SOC task. CyberChef's "From Base64" does the same thing in a browser.
