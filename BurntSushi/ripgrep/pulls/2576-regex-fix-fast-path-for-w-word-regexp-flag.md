```yaml
number: 2576
title: "regex: fix fast path for -w/--word-regexp flag"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-2574
created_at: 2023-07-31T12:06:17Z
updated_at: 2023-07-31T12:51:14Z
url: https://github.com/BurntSushi/ripgrep/pull/2576
synced_at: 2026-01-12T18:23:14Z
```

# regex: fix fast path for -w/--word-regexp flag

---

_@BurntSushi_

It turns out our fast path for -w/--word-regexp wasn't quite correct in some cases. Namely, we use `(?m:^|\W)(<original-regex>)(?m:\W|$)` as the implementation of -w/--word-regexp since `\b(<original-regex>)\b` has some unintuitive results in certain cases, specifically when <original-regex> matches non-word characters at match boundaries.

The problem is that using this formulation means that you need to extract the capture group around <original-regex> to find the "real" match, since the surrounding (^|\W) and (\W|$) aren't part of the match. This is fine, but the capture group engine is usually slow, so we have a fast path where we try to deduce the correct match boundary after an initial match (before running capture groups). The problem is that doing this is rather tricky because it's hard to know, in general, whether the `^` or the `\W` matched.

This still doesn't seem quite right overall, but we at least fix one more case.

Fixes #2574

---

_Merged by @BurntSushi on 2023-07-31 12:51_

---

_Closed by @BurntSushi on 2023-07-31 12:51_

---

_Branch deleted on 2023-07-31 12:51_

---
