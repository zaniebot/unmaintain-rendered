```yaml
number: 5920
title: Changelog top level date is wrong
type: issue
state: closed
author: ThierryBerger
labels: []
assignees: []
created_at: 2025-02-20T14:00:25Z
updated_at: 2025-08-07T20:15:54Z
url: https://github.com/clap-rs/clap/issues/5920
synced_at: 2026-01-12T16:14:17Z
```

# Changelog top level date is wrong

---

_@ThierryBerger_

Looking at the changelog is confusing due to 2022 being listed on the top level, it should probably be "unreleased". But then we see "unreleased" later.

https://github.com/clap-rs/clap/blob/cb2352f84a7663f32a89e70f01ad24446d5fa1e2/CHANGELOG.md?plain=1#L1-L23

---

_Comment by @epage on 2025-02-20 14:06_

Unsure why 5.0.0 has a date.  Its not released but we have to be careful how we mark it because we have regexes that will match on certain text to update the changelog on release.

I made some tweaks in 9caee534

---

_Closed by @epage on 2025-08-07 20:15_

---
