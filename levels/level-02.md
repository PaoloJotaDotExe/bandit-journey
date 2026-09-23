# Bandit Level 2 → 3

**Concepts:** quoting, file names with spaces and leading dashes

## Goal
Read a file whose name starts and ends with `--` and contains spaces.

## What I tried
I reused the `./` trick from the previous level, but it wasn't enough on its own. The name had **two** problems:

1. **Leading `--`**: commands treat it as the start of an option.
2. **Spaces**: the shell splits the name into several separate arguments.

My failed attempts, in order:

| Attempt | Why it failed |
|---|---|
| `file ./--spaces in this filename--` | spaces split the name into four arguments |
| `cat "--spaces in this filename--"` | quoted, but the leading `--` was still read as an option |
| `file"./--spaces in this filename--"` | no space between the command and its argument |

The solution needed both fixes at once: quotes to keep the name together, and `./` to neutralize the dashes.

<details>
<summary>🔓 Show solution</summary>

```bash
cat "./--spaces in this filename--"
```

Alternatives: `cat -- "--spaces in this filename--"` (`--` marks the end of options), or escaping each space with `\`.

</details>

## Password check
SHA-256 of the password for `bandit3`:

```
7ea3355ee2e7f54f4c6606c190dbd58121ce8007e5e388c1c6880eeb49b30be5
```

## What I learned
- Quotes (`"..."`) keep a name with spaces together as one argument.
- `./name` or `-- name` stops a leading dash from being parsed as an option.
- When a command fails, find out *why* before trying the next variation. Each failure above had a different cause.
