# Bandit Level 8 → 9

**Concepts:** `sort` and `uniq` in a pipeline, finding the one line that is not repeated, thinking about text tools like SQL

## Goal
The password is in `data.txt`, and it is the only line in the file that appears exactly once. Every other line is repeated several times.

## What I tried
The level page lists a handful of suggested commands (`grep`, `sort`, `uniq`, `strings`, `base64`, `tr` and more). Because the goal was about a line that shows up *only once*, I went straight for `sort` and `uniq`. It felt a lot like a `COUNT` with `GROUP BY` in SQL.

My first idea was `uniq -c` to count how many times each line appears and then spot the line with a count of 1. That works. Reading `man uniq` afterwards showed a flag that does the filtering for me.

<details>
<summary>🔓 Show solution</summary>

```bash
sort data.txt | uniq -u
# -u prints only the lines that are never repeated
```

My first version, which also works but needs reading through the counts:

```bash
sort data.txt | uniq -c | sort -n | head
# counts each line, then lists the smallest counts first
```

</details>

## Password check
SHA-256 of the password for `bandit9`:

```
fdba3d116616b31328f68da32a15a02ca46b347743af3df46b13c58cba06f7e1
```

## What I learned
- **`uniq` only compares neighbouring lines.** Duplicates that are far apart in the file are not detected, which is why the file must go through `sort` first. `sort | uniq` is a pair you'll use a lot.
- **Useful `uniq` flags:** `-c` counts occurrences, `-u` prints only lines that appear once, `-d` prints only lines that repeat.
- **It really is like SQL:**

  | SQL | Shell |
  |---|---|
  | `SELECT line, COUNT(*) ... GROUP BY line` | `sort file \| uniq -c` |
  | `... HAVING COUNT(*) = 1` | `sort file \| uniq -u` |
  | `ORDER BY count` | `... \| sort -n` |

- **Where this shows up in real security work:** counting repeated values is how you spot outliers in logs, for example which IP has the most failed logins: `grep "Failed password" auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn | head`. In KQL the same idea is `| summarize count() by IpAddress | order by count_ desc`.
