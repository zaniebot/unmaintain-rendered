```yaml
number: 3809
title: "Deprecate `ArgMatches::get_raw`"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2022-06-09T14:04:49Z
updated_at: 2022-06-09T14:04:49Z
url: https://github.com/clap-rs/clap/issues/3809
synced_at: 2026-01-12T16:14:15Z
```

# Deprecate `ArgMatches::get_raw`

---

_@epage_

Blocked on #3807 and #3808

---

With #3732, we added a typed API but provided `get_raw` as an escape hatch in case people need it.  However, this means everyone pays the price for us tracking raw values.

With this, #3807, and #3808, we could stop tracking raw values and reduce some overhead

---

_Label `C-enhancement` added by @epage on 2022-06-09 14:04_

---

_Label `S-waiting-on-decision` added by @epage on 2022-06-09 14:04_

---

_Label `A-parsing` added by @epage on 2022-06-09 14:04_

---
