```yaml
number: 17495
title: "Short form `-t` for `--target` wit `uv pip`"
type: issue
state: open
author: fifty-six
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2026-01-15T20:07:58Z
updated_at: 2026-01-15T20:46:19Z
url: https://github.com/astral-sh/uv/issues/17495
synced_at: 2026-01-15T21:13:03Z
```

# Short form `-t` for `--target` wit `uv pip`

---

_@fifty-six_

### Summary

When using `uv pip` as a drop-in for `pip`, one discrepancy I've noticed is that `uv pip` doesn't support the short-form `-t` of `pip`'s  `--target`. I had a few scripts at work use `pip install -t` and ended up writing a small script to workaround it, but it'd be nice to have upstream.

I'd be fine PRing this, but wanted to file an issue first to make sure that was alright.

### Example

`uv pip install -t $DIR pkg1 pkg2 ...`

---

_Label `enhancement` added by @fifty-six on 2026-01-15 20:07_

---

_Comment by @zanieb on 2026-01-15 20:46_

Generally we're trying to reserve short flag names until it's clear what the best use of them is, but it sounds sensible to match `pip` here. Feel free to open a pull request!

---

_Label `compatibility` added by @zanieb on 2026-01-15 20:46_

---
