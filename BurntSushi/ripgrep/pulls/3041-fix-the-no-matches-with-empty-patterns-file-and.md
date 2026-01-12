```yaml
number: 3041
title: "Fix the \"No matches with empty patterns file and invert match\" bug"
type: pull_request
state: closed
author: xenoninja
labels:
  - rollup
assignees: []
base: master
head: fix-pattern-file-searching
created_at: 2025-05-08T07:02:58Z
updated_at: 2025-07-29T05:22:25Z
url: https://github.com/BurntSushi/ripgrep/pull/3041
synced_at: 2026-01-12T18:23:15Z
```

# Fix the "No matches with empty patterns file and invert match" bug

---

_@xenoninja_

This PR addresses https://github.com/BurntSushi/ripgrep/issues/1332 and https://github.com/BurntSushi/ripgrep/issues/3001.
The problem is when an empty pattern file is given, ripgrep sees the pattern as "an empty regex" not "zero regexes".

>
> | pattern file | grep's output | rg's output |
> |:-|:-:|:-:|
> | **empty** | everything |  nothing |
> | **newline** | nothing | nothing |
>

Now the fix works out as below

| pattern file | grep's output | rg's output |
|:-|:-:|:-:|
| **empty** | everything |  **everything** |
| **newline** | nothing | nothing |

---

_Label `rollup` added by @BurntSushi on 2025-07-27 19:12_

---

_Label `rollup` removed by @BurntSushi on 2025-07-27 19:12_

---

_Label `rollup` added by @BurntSushi on 2025-07-27 19:13_

---

_Comment by @BurntSushi on 2025-07-27 19:13_

Thank you for this PR. Unfortunately, this was not quite correct. The optimization needs to be a hair smarter and the PCRE2 regex builder had to be fixed to correctly construct a pattern that can't match when there are zero patterns. And this should also come with a regression test.

I have this fixed in a branch that I'll release at some point soon.

---

_Comment by @xenoninja on 2025-07-29 05:22_

Oh, sorry about the mistaken PR. I've seen the fix commit. Thanks for your effort!

---

_Closed by @xenoninja on 2025-07-29 05:22_

---
