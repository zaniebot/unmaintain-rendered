```yaml
number: 2032
title: "build: enforce consistent casing"
type: pull_request
state: closed
author: sbrugman
labels: []
assignees: []
base: main
head: enforce-consistent-name
created_at: 2023-01-20T15:57:23Z
updated_at: 2023-01-20T23:44:42Z
url: https://github.com/astral-sh/ruff/pull/2032
synced_at: 2026-01-12T15:55:07Z
```

# build: enforce consistent casing

---

_@sbrugman_

Minor OCD fix, enforced in `build.rs`

We might also consider sorting `RuleOrigin` in `registry.rs` ([related](https://github.com/rust-lang/rustfmt/issues/3422)). This especially makes sense for the flake8 plugins.

---

_Comment by @andersk on 2023-01-20 20:19_

I think the case differences here were intentional. [Pylint](https://pylint.readthedocs.io/en/latest/) refers to itself capitalized at the beginning of headings and sentences, while [Pyflakes](https://pypi.org/project/pyflakes/) and Ruff refer to themselves capitalized everywhere.

---

_Comment by @charliermarsh on 2023-01-20 21:37_

Ah yeah, I appreciate the attention-to-detail but this actually was intentional. (Hopefully I got them all right.)

---

_Closed by @charliermarsh on 2023-01-20 21:37_

---

_Branch deleted on 2023-01-20 23:44_

---
