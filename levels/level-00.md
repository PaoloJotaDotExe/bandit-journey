# Bandit Level 0 → 1

**Concepts:** SSH, remote login, listing and reading files

## Goal
Log in to the game server over SSH with the credentials given on the level page, then find the password for the next level in a file in the home directory.

## What I tried
Getting in was the hardest part. I had never used SSH, and I didn't know the server listens on a non-default port. Once I understood the command structure, I broke it down:

| Part | Meaning |
|---|---|
| `ssh` | the program that opens a secure remote shell |
| `bandit0@bandit.labs.overthewire.org` | `user@host` |
| `-p 2220` | connect on **port** 2220 instead of the default 22 |

After logging in, I listed the home directory and found a `readme` file. My first instinct was `cd readme`, which failed because `readme` is a file, not a directory. Opening a file takes a different command.

<details>
<summary>🔓 Show solution</summary>

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
ls
cat readme
```

</details>

## Password check
SHA-256 of the password for `bandit1`:

```
fd58c8428709610587c2eda9576163588a536d1376b1f1ff39821baa7fd25de1
```

## What I learned
- `ssh user@host -p PORT` connects to a remote machine on a specific port.
- `ls` (*list*) shows directory contents. `ls -la` also shows hidden files and permissions.
- `cd` (*change directory*) only works on directories. To read a file, use `cat` (*concatenate*). The PowerShell equivalent is `Get-Content`.
