```yaml
number: 17103
title: "Invalid `exclude-newer` type in JSON Schema"
type: issue
state: closed
author: eggplants
labels:
  - bug
assignees: []
created_at: 2025-12-12T15:11:42Z
updated_at: 2025-12-13T19:42:02Z
url: https://github.com/astral-sh/uv/issues/17103
synced_at: 2026-01-12T16:02:44Z
```

# Invalid `exclude-newer` type in JSON Schema

---

_@eggplants_

### Summary

https://github.com/astral-sh/uv/blob/5a55bbe8838b45ad795714695a34f30e83dfc266/uv.schema.json#L216-L226

> a "friendly" duration (e.g., `24 hours`, `1 week`, `30 days`), or an ISO 8601 duration (e.g., `PT24H`, `P7D`, `P30D`)


They are not contained in `ExcludeNewerTimestamp | null`.


### Platform

Linux 6.17.8-orbstack-00308-g8f9c941121b1 aarch64 GNU/Linux

### Version

uv 0.9.17

### Python version

Python 3.14.2

---

_Label `bug` added by @eggplants on 2025-12-12 15:11_

---

_Comment by @eggplants on 2025-12-12 15:32_

For ISO 8601 durations, new data type [`duration`](https://json-schema.org/understanding-json-schema/reference/type#dates-and-times:~:text=11%2D13.-,%22duration%22,-%3A) is available after draft 2019-09.

However, the friendly durations accepted by [`jiff::fmt::friendly`](https://docs.rs/jiff/latest/jiff/fmt/friendly/index.html) are too complex to define as regular expressions.

---

_Comment by @zanieb on 2025-12-12 17:39_

@BurntSushi do you have a recommendation here? I guess we might just want to use `string`? Is there a benefit to having `pattern | duration | string`?

---

_Comment by @BurntSushi on 2025-12-12 17:46_

I'm not terribly familiar with what having precise types in a JSON Schema gives you. The "friendly" format isn't a widely recognized standard and representing it with a _succinct_ regex is probably not possible, so you'll likely need `string` _somewhere_.

I do believe the friendly format can be represented as a single regex, but it would be quite large. (About as big as [the friendly format grammar](https://docs.rs/jiff/latest/jiff/fmt/friendly/index.html#grammar).)

---

_Closed by @zanieb on 2025-12-13 19:42_

---
