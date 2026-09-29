# Bandit Level 6 → 7

**Concepts:** `find` from the filesystem root, filtering by owner, group and size, Linux permissions, redirecting stderr with `2>/dev/null`

## Goal
This time the password isn't in my home folder. It's in a file somewhere on the whole server. I only know three things about it: which user owns it, which group it belongs to, and that it is exactly 33 bytes long.

## What I tried
The first thing I noticed was that, unlike the earlier levels, I could keep going up with `cd ..` all the way to the root of the server. Listing `/home` printed a huge list of folders, one for every user on the machine. Walking through all of that by hand was clearly not the way.

So I went back to `find`, the tool that saved me in the previous level, and learned its structure properly:

```
find   WHERE        FILTERS                          ACTION (optional)
find   /            -type f -user X -group Y -size N  -exec cat {} \;
```

- **WHERE:** `.` is the current folder, `/home` is one folder, `/` is the whole server.
- **FILTERS:** each filter is one property the file must have. When you list several, the file has to match **all** of them.
- **ACTION:** optional. Without one, `find` just prints the path of each match.

My first idea for the command included a name filter:

```bash
find / -type f -name "*.log" -user bandit7 -group bandit6 -size 33c
```

That filter was a mistake. The level never says anything about the file's name or extension, so `-name "*.log"` would silently exclude the right file if it wasn't a `.log`. **Only filter on what you actually know.**

Without the name filter, the search works. But searching from `/` as an unprivileged user floods the screen with `Permission denied` lines for every folder I'm not allowed to open, and the one real result gets buried in the noise.

<details>
<summary>🔓 Show solution</summary>

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
# prints a single path
cat <that-path>
```

`2>/dev/null` throws the error messages away, so only the match is left on screen.

</details>

## Password check
SHA-256 of the password for `bandit7`:

```
fb0449bcb9a2329c073d51af4eeb728c560df247e54e156e7681f63f7b686066
```

## What I learned
- **`find` has a fixed shape:** where to look, then the filters, then an optional action. Multiple filters combine as AND.
- `-user` and `-group` filter by ownership. `-size 33c` means exactly 33 **bytes** (without `c`, `find` counts 512-byte blocks).
- **Permissions explain why I could see other users' folders.** Many directories let "others" *list* their contents without letting them *read* the files inside. `ls -la` shows this in the permission column (`drwxr-xr-x`: the last three characters are what everyone else can do).
- **The terminal has two output channels:** stdout (1) for results and stderr (2) for errors. `2>/dev/null` sends channel 2 to `/dev/null`, the Linux "black hole". It **only hides the error messages**. It doesn't change what I'm allowed to access and doesn't make the command safer. I first thought it limited the search to files I had permission for, and that's wrong.
- Nobody is born knowing `2>/dev/null`. The way to find it is to run the command, see the problem (a wall of `Permission denied`), and search for exactly that problem.
- **Where this shows up in real security work:** the same "filter by properties, drop the noise" pattern is used in incident response (`find / -mmin -60 -type f 2>/dev/null` lists files changed in the last hour) and in privilege escalation checks (`find / -perm -4000 2>/dev/null` lists SUID programs). It's also the same logic as a KQL or SQL query: `WHERE owner = 'bandit7' AND size = 33`.
