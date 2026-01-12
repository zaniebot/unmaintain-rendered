```yaml
number: 21304
title: "PLW1514 doesn't catch argparse.FileType without encoding"
type: issue
state: open
author: novas0x2a
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-11-06T19:39:52Z
updated_at: 2025-11-11T23:48:08Z
url: https://github.com/astral-sh/ruff/issues/21304
synced_at: 2026-01-12T15:54:57Z
```

# PLW1514 doesn't catch argparse.FileType without encoding

---

_@novas0x2a_

### Summary

`argparse.FileType` is essentially just a wrapper around `open`, so it would be helpful if `PLW1514` noticed when the encoding argument was missing. Important note: pylint does not catch this either, so this change would make ruff stricter than pylint (I'm not sure what the project policy on this is, but I figured I'd note it...)

---

_Comment by @ntBre on 2025-11-07 19:10_

This seems reasonable to me, but I'm curious what others think (cc @amyreese). I hadn't heard of `argparse.FileType` before today and it seems to be [deprecated](https://docs.python.org/3/library/argparse.html#argparse.FileType) as of 3.14, but that doesn't necessarily mean we shouldn't include it in the rule.

---

_Label `rule` added by @ntBre on 2025-11-07 19:10_

---

_Label `needs-decision` added by @ntBre on 2025-11-07 19:10_

---

_Comment by @amyreese on 2025-11-11 23:48_

I also had not seen `argparse.FileType` before, and I'm also surprised that it's deprecated without a clear replacement. 😅 I agree that this could be a useful extension of the existing lint rule, but I'm curious how widely that option is even used in the wider ecosystem as a measure of deciding whether it's worth linting something that's being deprecated.

---
