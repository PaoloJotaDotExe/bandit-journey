# Bandit Journey

My writeups for [OverTheWire Bandit](https://overthewire.org/wargames/bandit/), a beginner wargame that teaches Linux and the command line over SSH.

Each writeup records what the level asked, what I tried (including what failed), the working solution, and what I learned. I started Bandit with almost no terminal experience, so the failed attempts are part of the point.

## Progress

| Level | Concept | Writeup | Status |
|---|---|---|---|
| 0 → 1 | Connecting over SSH, reading a file | [level-00](levels/level-00.md) | ✅ |
| 1 → 2 | File named `-` | [level-01](levels/level-01.md) | ✅ |
| 2 → 3 | File name with dashes and spaces | [level-02](levels/level-02.md) | ✅ |
| 3 → 4 | Hidden files | [level-03](levels/level-03.md) | ✅ |
| 4 → 5 | Finding the only human-readable file | [level-04](levels/level-04.md) | ✅ |
| 5 → 6 | `find` with size and permission filters, hidden files | [level-05](levels/level-05.md) | ✅ |
| 6 → 7 | `find` across the whole server by owner, group and size, `2>/dev/null` | [level-06](levels/level-06.md) | ✅ |
| 7 → 8 | — | — | 🔄 in progress |

## Spoiler policy

- **Solutions are hidden by default.** Every writeup keeps the approach visible and puts the final commands inside a collapsed spoiler block. Open it only if you want the answer.
- **No passwords are published.** OverTheWire's [rules](https://overthewire.org/rules/) ask writeup authors not to publish credentials, and I follow that.
- **You can still check your answer.** Each level lists the SHA-256 hash of the password I found. Hash yours and compare. If the hashes match, you found the right password. The hash can't be turned back into the password.

```bash
# Linux / macOS / Git Bash
echo -n 'password-you-found' | sha256sum
```

```powershell
# Windows PowerShell
$p = 'password-you-found'
[BitConverter]::ToString([Security.Cryptography.SHA256]::Create().ComputeHash([Text.Encoding]::UTF8.GetBytes($p))).Replace('-','').ToLower()
```

> Bandit passwords are rotated from time to time, so an old hash may stop matching even when your method is right.

## Credits

All challenges belong to the [OverTheWire](https://overthewire.org) community. Level goals are described in my own words; visit the site for the original challenges.
