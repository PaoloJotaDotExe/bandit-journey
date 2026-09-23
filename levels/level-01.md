# Bandit Level 1 → 2

**Concepts:** special characters in file names, relative paths

## Goal
Read the password stored in a file whose name is a single dash: `-`.

## What I tried
This one took about 20 minutes. `ls` showed the file, but `cat -` didn't print anything useful. Many Linux commands read `-` as "read from standard input" instead of as a file name, so `cat` sat waiting for keyboard input. I also tried `cd`, which doesn't apply to files.

The fix was to give the shell a **path** instead of a bare name. With `./` in front, the argument can no longer be mistaken for an option or for standard input.

<details>
<summary>🔓 Show solution</summary>

```bash
cat ./-
```

The absolute path works too: `cat /home/bandit1/-`

</details>

## Password check
SHA-256 of the password for `bandit2`:

```
4fc9a12a57302f354e92922f7a02d4e1e45454ddc7cea6ca67861e7219b50e02
```

## What I learned
- `-` has special meaning for many commands (standard input).
- Prefixing a name with `./` (current directory) makes it an unambiguous path.
- Reading the level's hints and the command's `man` page before brute-forcing attempts saves time.
