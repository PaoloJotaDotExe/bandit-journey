# Bandit Level 9 → 10

**Concepts:** binary files vs text, `strings` to pull readable text out of a binary, filtering with `grep`

## Goal
The password is in `data.txt`, in one of the few human-readable strings in the file, and it comes right after several `=` characters. Most of the file is not text.

## What I tried
My plan was to use `grep` with another command after it in a pipe. Before getting there I just ran `cat data.txt`, and among all the garbage characters I could actually see the password right after a row of `====`. My mental model was "read the file, then press Ctrl+H to search for the `=` signs", like in a text editor.

That worked this time, but only because the file was small. The level is designed around a cleaner approach: first extract only the readable parts of the binary, then filter them.

<details>
<summary>🔓 Show solution</summary>

```bash
strings data.txt | grep "=="
# strings keeps only runs of printable characters, grep keeps the lines with ==
```

</details>

## Password check
SHA-256 of the password for `bandit10`:

```
6a5d9a20dc3f462124795c154987edbc6cb76de8d5413ebbd617b395a38f6674
```

## What I learned
- **`cat` on a binary file is a bad habit.** It prints control bytes that can mess up the terminal (if that happens, `reset` fixes it). Check what the file is first with `file data.txt`.
- **`strings` extracts the human-readable parts** of a binary (by default, runs of 4+ printable characters). It's one of the first tools used to look inside an unknown executable or a malware sample.
- **Ctrl+H is not search in the terminal.** In most terminals it acts as Backspace. To search inside output, pipe it to `grep`, or open it in `less` and press `/` followed by the text.
- `grep "=="` is safer than `grep "="`: the more specific the pattern, the less noise.
- **Where this shows up in real security work:** `strings suspicious.exe | grep -i http` is a classic first look at a suspicious file, to find URLs, IPs or commands hidden in it without running it.
