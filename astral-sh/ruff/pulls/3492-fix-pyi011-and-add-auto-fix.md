```yaml
number: 3492
title: Fix PYI011 and add auto-fix
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: pyi011-auto-fix
created_at: 2023-03-13T21:02:13Z
updated_at: 2023-03-14T20:05:03Z
url: https://github.com/astral-sh/ruff/pull/3492
synced_at: 2026-01-12T04:39:45Z
```

# Fix PYI011 and add auto-fix

---

_Pull request opened by @JonathanPlasse on 2023-03-13 21:02_

- Fix PYI011 accept math constants in defaults
- Add auto-fix

---

_@JonathanPlasse reviewed on 2023-03-13 21:04_

---

_Review comment by @JonathanPlasse on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI011.pyi`:108 on 2023-03-13 21:04_

This comment will be removed.
I am working on another PR to add a method to aggregate comments from `Expression` to keep them.

---

_Comment by @github-actions[bot] on 2023-03-13 21:13_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:203 on 2023-03-14 08:48_

Can we add a `SAFETY: <reason>` comment here and on line 234 explaining why it's safe to call `unwrap` here?

---

_@MichaReiser approved on 2023-03-14 08:48_

---

_@JonathanPlasse reviewed on 2023-03-14 09:18_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:203 on 2023-03-14 09:18_

This is done in a lot of places in the code.
I do not think it is needed.
- `node.end_location` is always the `Some` variant (c.f. RustPython/RustPython#4192)

---

_Merged by @charliermarsh on 2023-03-14 18:43_

---

_Closed by @charliermarsh on 2023-03-14 18:43_

---

_Branch deleted on 2023-03-14 20:05_

---
