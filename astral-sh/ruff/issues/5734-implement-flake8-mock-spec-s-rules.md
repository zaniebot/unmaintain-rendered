```yaml
number: 5734
title: "Implement flake8-mock-spec's rules"
type: issue
state: open
author: sauloperez
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-07-13T10:25:52Z
updated_at: 2025-10-06T20:21:53Z
url: https://github.com/astral-sh/ruff/issues/5734
synced_at: 2026-01-12T15:54:45Z
```

# Implement flake8-mock-spec's rules

---

_@sauloperez_

I want to enforce the use of the `autospec` argument on Python's unittest mocks in my team's codebase and so after a quick googling I found https://pypi.org/project/flake8-mock-spec/. Specifically, the rule `TMS021` is what we need.

It would be really good to give support for that plugin, as we are actively migrating to Ruff.

I would be glad to help myself in the implementation but I have absolutely no knowledge of Rust. I'm happy to help in any other way.

---

_Label `plugin` added by @charliermarsh on 2023-07-13 17:23_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-13 17:23_

---

_Comment by @evanrittenhouse on 2023-08-06 01:13_

I can implement this, feel free to assign to me, if you decide to do it.

---

_Comment by @neob91-close on 2024-02-14 09:47_

I'd love to see this get implemented!

---

_Comment by @kirschem-fernride on 2025-02-19 14:30_

Any progress on this?

---

_Comment by @Tenzer on 2025-10-06 20:21_

I have tried to start some work on this and I've made an incomplete draft PR in #20728. I'm hoping I can get a bit of feedback on what I'm doing looks correct, as this is my first time contributing to Ruff.

---
