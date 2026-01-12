```yaml
number: 1253
title: "Mention `--auto-hybrid-regex` in the README’s advantages section"
type: pull_request
state: merged
author: roryokane
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2019-04-16T20:56:43Z
updated_at: 2019-04-16T21:21:41Z
url: https://github.com/BurntSushi/ripgrep/pull/1253
synced_at: 2026-01-12T18:23:13Z
```

# Mention `--auto-hybrid-regex` in the README’s advantages section

---

_@roryokane_

This feature solves a major reason I was skeptical of using ripgrep, so I think it’s good to mention it in the section about why one should use it.

I use backreferences a lot, so I had previously thought that ripgrep would provide no speed advantage over ag, since I would always have `-P` enabled. But when I saw `--auto-hybrid-regex` in the [11.0.0 changelog](https://github.com/BurntSushi/ripgrep/blob/11.0.0/CHANGELOG.md#1100-2019-04-15), I learned that ripgrep can use it to speed up simple queries while still allowing me to write backreferences.

---

_@BurntSushi approved on 2019-04-16 21:00_

Great idea! I like it.

Note that even when using PCRE2, ripgrep could very well be faster than ag. ripgrep's optimizations span a much broader category than just "faster regex engine." :-) Plus, with ripgrep, you get proper Unicode support (even with PCRE2) by default.

---

_Merged by @BurntSushi on 2019-04-16 21:21_

---

_Closed by @BurntSushi on 2019-04-16 21:21_

---
