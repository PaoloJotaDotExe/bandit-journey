# Bandit Level 3 → 4

**Concepts:** hidden files, tab completion

## Goal
Find the password in a hidden file inside the `inhere` directory.

## What I tried
I listed the home directory, found `inhere`, and looked inside. A plain `ls` showed nothing useful, because in Linux any file whose name starts with `.` is **hidden** from a normal listing. I went through the commands suggested on the level page (`ls`, `cd`, `cat`, `file`, `du`, `find`).

What actually worked was typing `cat` inside `inhere` and pressing **Tab**. The shell's auto-completion revealed the hidden file's name.

<details>
<summary>🔓 Show solution</summary>

```bash
cd inhere
ls -la
cat ./...Hiding-From-You
```

</details>

## Password check
SHA-256 of the password for `bandit4`:

```
394902f09bf09e7c3f864e5838a0b00d7a5dea9cf88dfbe12ad00c06d9562210
```

## What I learned
- Files starting with `.` are hidden. `ls -a` shows them, and `ls -la` adds details.
- Every directory contains `.` (the directory itself) and `..` (its parent). These were the two entries I didn't understand at first.
- **Tab completion** saves typing and doubles as a quick way to discover file names.
