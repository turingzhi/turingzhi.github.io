---
title: "Missing Semester Lecture4: Data Wrangling"
date: 2024-04-21 17:57:46
tags: shell
categories:
- [missing semester]
---
### Lecture 4: Data Wrangling

link:https://missing.csail.mit.edu/2020/data-wrangling/

sed is short for stream editor

```bash
| sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
```

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
 | awk '{print $2}' | paste -sd,
```

```bash
| awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
```

```bash
BEGIN { rows = 0 }
$1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
END { print rows }
```

```bash
 | paste -sd+ | bc -l
```


