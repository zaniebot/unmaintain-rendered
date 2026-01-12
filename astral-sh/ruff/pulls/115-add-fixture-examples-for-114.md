```yaml
number: 115
title: "Add fixture examples for #114"
type: pull_request
state: merged
author: cjfuller
labels: []
assignees: []
merged: true
base: main
head: add_fixture_for_issue_114
created_at: 2022-09-06T23:10:21Z
updated_at: 2022-09-07T00:39:37Z
url: https://github.com/astral-sh/ruff/pull/115
synced_at: 2026-01-12T05:48:45Z
```

# Add fixture examples for #114

---

_Pull request opened by @cjfuller on 2022-09-06 23:10_

Type annotations on class-level or top-level variable definitions lead to an erroneous E821 (see #114). This commit contains fixture entries that catch this issue, currently annotated with `# noqa`.

Tested with: `cargo test`, which shows failures prior to adding the `# noqa` but none after doing so.

---

_Merged by @charliermarsh on 2022-09-07 00:18_

---

_Closed by @charliermarsh on 2022-09-07 00:18_

---

_Branch deleted on 2022-09-07 00:39_

---
