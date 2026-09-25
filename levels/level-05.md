# Bandit Level 5 → 6

**Concepts:** `find` with filters (type, size, permissions), hidden files, decoy files

## Goal
Somewhere inside the ~20 subfolders of `inhere` there is exactly one file that is human-readable, exactly 1033 bytes long, and not executable. Find it.

## What I tried
I started opening files one by one across the folders and got lost fast. Twenty folders with several files each is not a job for `cat`. That's when I learned about `find`, which filters files by their properties instead of making me look at each one.

My first attempt had a typo:

```bash
find . type f -size 1033c ! -executable
# find: 'type': No such file or directory
# find: 'f': No such file or directory
```

Without the dash, `find` didn't read `type f` as a filter. It read `type` and `f` as two more folders to search in, and they don't exist. It still printed a result, because `.` was a valid starting point. With the dash fixed, `find` returned exactly one path.

Then I fell for the decoy. Instead of opening the path `find` gave me, I went into that folder and ran `ls`. `ls` doesn't show hidden files, so the file I was looking for wasn't in the list. I picked a file with a similar name instead. `file` said it was ASCII text, so it looked plausible, and `cat` printed ~2,500 characters of random text. I took that as the password.

I only realized it was wrong when I tried to log in as `bandit6` and it failed. Every Bandit password is 32 characters, so that wall of text should have been a red flag right away. I went back, listed the folder with hidden files included, found the real file, and got a proper 32-character password.

<details>
<summary>🔓 Show solution</summary>

```bash
cd inhere
find . -type f -size 1033c ! -executable
# prints a single path: the answer is that exact file
cat ./maybehere07/.file2
```

The leading `.` in `.file2` makes it a hidden file. That's why plain `ls` never showed it:

```bash
ls -la ./maybehere07   # -a shows hidden files
```

</details>

## Password check
SHA-256 of the password for `bandit6`:

```
1e1ad106d6c56f5f9bf01164fc00468671cadae3ff9d1dfba01ca400aad9a902
```

## What I learned
- `find` searches by properties. `-type f` means regular files only, `-size 1033c` means exactly 1033 bytes (`c` = bytes; without it, `find` counts 512-byte blocks), and `! -executable` means **not** executable (`!` negates the next test).
- Flags need their dash. `type` and `-type` mean completely different things to `find`.
- `ls` hides files that start with `.`. Use `ls -a` (or `ls -la`) to see everything.
- "Human-readable" alone didn't identify the file, because the decoy was ASCII text too. Only the combination of all three filters narrows it down to one.
- **Read the tool's output literally.** `find` gave me the exact answer, and I went exploring instead of trusting it.
- Sanity-check the result. A Bandit password is 32 characters, so anything else is wrong.
