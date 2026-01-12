```yaml
number: 1790
title: Stop on first non-match after positive match
type: issue
state: closed
author: sveniu
labels:
  - enhancement
  - help wanted
  - rollup
assignees: []
created_at: 2021-01-29T08:57:10Z
updated_at: 2023-07-08T22:53:14Z
url: https://github.com/BurntSushi/ripgrep/issues/1790
synced_at: 2026-01-12T16:13:24Z
```

# Stop on first non-match after positive match

---

_@sveniu_

### Feature request summary

Stop search on first non-matching line, but only after having seen matching lines.

### Motivation

On sorted or semi-sorted files, it is often useful to extract consecutive lines that match a given pattern. In some cases involving large files, time and IO can be saved by stopping the search after having matched such a run of consecutive lines.

I'm fairly convinced this feature can save countless CPU cycles and IO in big-ish data situations, particularly where timestamp matching is involved.

### Example

Input:
```
2021-01-29T07:08:09+0100 foo
2021-01-29T08:08:09+0100 bar
2021-01-29T09:08:09+0100 baz
2021-01-29T10:08:09+0100 boz
```

Behaviour when searching for pattern `^2021-01-29T08` with this option enabled:

1. First line does not match; continue to next.
2. Second line matches; print it and continue to next.
3. Third line does not match; break and exit; line 4 is never scanned.

### Pseudo code

```
have_seen_matches = null
for each line:
  if line matches:
    print line
    have_seen_matches = true
  else:
    if have_seen_matches == true:
      break
```

### Suggested documentation
```
--stop-on-nonmatch
    Stop reading a file on the first non-matching line that follows a matching
    line. At least one line must have matched for this to take effect.
```

---

_Comment by @BurntSushi on 2021-01-30 23:15_

Why doesn't the `--max-count` flag achieve what you need here?

---

_Comment by @sveniu on 2021-01-31 18:16_

> Why doesn't the `--max-count` flag achieve what you need here?

Because the number of matching lines is not known when starting the search. I might be misunderstanding what you actually mean, or my description or use case was unclear.

---

_Label `enhancement` added by @BurntSushi on 2021-06-01 00:14_

---

_Label `help wanted` added by @BurntSushi on 2021-06-01 00:14_

---

_Label `rollup` added by @BurntSushi on 2023-07-08 22:17_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
