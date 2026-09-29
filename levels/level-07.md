# Bandit Level 7 → 8

**Concepts:** searching inside a large file with `grep`, inspecting a file before dumping it, stopping a runaway command with `Ctrl+C`

## Goal
The password is in `data.txt` in my home folder, on the same line as the word "millionth". The catch is that the file is huge.

## What I tried
I started with the usual routine: `ls`, found `data.txt` right away, and ran `cat` on it. The terminal spent about two minutes scrolling text. I stopped it with `Ctrl+C`.

Then I opened the manuals for the commands suggested for the level, looking for some way to "print" just part of the file, but nothing I tried there helped. What worked was flipping the approach: instead of reading the file and looking for the word, let a tool look for the word and print only that line. That tool is `grep`.

<details>
<summary>🔓 Show solution</summary>

```bash
grep millionth data.txt
# prints the single matching line: the word, then the password
```

</details>

## Password check
SHA-256 of the password for `bandit8`:

```
122b7f5fa9dd9af5ee7c96b124ce945b4428d6a91172ec98b90b1783dc1bfd80
```

## What I learned
- **`grep PATTERN FILE` prints only the lines that contain the pattern.** On a file with thousands of lines, that turns minutes of scrolling into one line of output.
- **`find` vs `grep`:** `find` searches for *files* by their properties (owner, size, name). `grep` searches for *text inside* files. Level 6 was a `find` problem; this one is a `grep` problem.
- **Look before you dump.** Before running `cat` on an unknown file, check how big it is:
  ```bash
  wc -l data.txt     # how many lines
  head data.txt      # first 10 lines only
  less data.txt      # scroll page by page, q to quit (and / to search)
  ```
- `Ctrl+C` interrupts the running command. It's the emergency brake of the terminal.
- **Where this shows up in real security work:** `grep` is the first tool for digging through logs on a Linux box, for example `grep "Failed password" /var/log/auth.log` to spot SSH brute-force attempts. In a SIEM, the same idea is a KQL filter like `| where Message has "Failed password"`.
