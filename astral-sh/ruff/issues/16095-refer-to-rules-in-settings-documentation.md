```yaml
number: 16095
title: Refer to rules in settings documentation
type: issue
state: open
author: DaniBodor
labels:
  - documentation
assignees: []
created_at: 2025-02-11T11:00:31Z
updated_at: 2025-02-12T05:56:21Z
url: https://github.com/astral-sh/ruff/issues/16095
synced_at: 2026-01-10T11:09:57Z
```

# Refer to rules in settings documentation

---

_Issue opened by @DaniBodor on 2025-02-11 11:00_

Not sure whether this should be part of #1773 and/or #1774, but figured it's maybe just about different enough to create a separate issue.

Currently, when looking at the settings in the [documentation](https://docs.astral.sh/ruff/settings/) or when going through a ruff settings file, there is no way to know which setting the rule interacts with.
Conversely, if I want to change the settings regarding a specific rule, I will almost always need to refer to that rule's documentation page to find what the rule is called. There is no logical way to deduce it without checking the documentation, unless one happens to know. (Also, there is no safeguard that the documentation for a rule is up to date with this, so can be forgotten when updating a rule as e.g. just happened [here](https://github.com/astral-sh/ruff/pull/15951#issuecomment-2650413727)).

It would be nice if:
1. the settings documentation states which rule a given setting refers to
2. there were some logical way to link settings names to rule names (this will probably not be touched before #1773/#1774)
3. some workflow ensuring that updates to rules also require updates in rule documentation

---

_Renamed from "Settings name to match rule codes" to "Refer to rules in settings documentation" by @DaniBodor on 2025-02-11 11:00_

---

_Label `documentation` added by @MichaReiser on 2025-02-11 11:04_

---

_Comment by @dhruvmanila on 2025-02-12 05:56_

For reference, Clippy has a "Configuration" section ([example](https://rust-lang.github.io/rust-clippy/master/index.html#unreadable_literal)) although I haven't looked at whether it's being generated automatically or not. It would be useful to have it done automatically, not sure how though.

---
