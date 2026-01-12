```yaml
number: 810
title: Improve formatting of version range logic
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-01-05T23:06:42Z
updated_at: 2024-01-10T20:16:24Z
url: https://github.com/astral-sh/uv/issues/810
synced_at: 2026-01-12T15:58:24Z
```

# Improve formatting of version range logic

---

_@zanieb_

It's very hard to distinguish when a `,` is part of the sentence **or** a logical and of version ranges, e.g. ([ref](https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L152-L155))

```
× No solution found when resolving dependencies:
╰─▶ Because there is no version of a available matching <1.0.0 | >1.0.0, <2.0.0 | >2.0.0, <3.0.0 | >3.0.0
        and a==1.0.0 depends on b==1.0.0, a<2.0.0 depends on b==1.0.0.
    And because a==3.0.0 depends on b==3.0.0, a<2.0.0 | >2.0.0 depends on b<=1.0.0 | >=3.0.0.
    And because root depends on b>=2.0.0, <3.0.0 and root depends on a<2.0.0 | >2.0.0, version solving failed.
```

`matching <1.0.0 | >1.0.0, <2.0.0 | >2.0.0, <3.0.0 | >3.0.0` is a logical and.
`depends on b==3.0.0, a<2.0.0 | >2.0.0` is part of the sentence.


---

_Renamed from "Improve formatting of version specifiers" to "Improve formatting of version range logic" by @zanieb on 2024-01-05 23:07_

---

_Label `error messages` added by @zanieb on 2024-01-05 23:07_

---

_Closed by @zanieb on 2024-01-10 20:16_

---
