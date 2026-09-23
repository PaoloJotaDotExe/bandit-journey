# Bandit Level 4 → 5

**Concepts:** file types, human-readable vs. binary data, wildcards

## Goal
Among several files in `inhere`, find the only one that contains human-readable text.

## What I tried
I opened the files one by one with `cat`. Most printed garbled characters, which is what binary data looks like when printed as text. One printed a clean 32-character string, the same format as every previous password.

It worked, but it doesn't scale. With 1,000 files, reading each one by hand isn't an option.

<details>
<summary>🔓 Show solution</summary>

What I did:

```bash
cd inhere
cat ./-file00   # repeat for each file until one is readable
```

The better approach I'll use from now on: let `file` classify every file at once, then read only the text one.

```bash
file ./*
# look for the entry reported as "ASCII text"
cat ./-fileXX
```

</details>

## Password check
SHA-256 of the password for `bandit5`:

```
a6536e7476611ed542c943803bad3d79c641ba92b935dadbf8eb433bf0b5ad80
```

## What I learned
- `file` identifies a file's type (text, binary, image...) without opening it.
- The `*` wildcard applies a command to many files at once. `./*` keeps names starting with `-` safe.
- Doing it by hand is fine the first time. After that, look for the command that automates it.
