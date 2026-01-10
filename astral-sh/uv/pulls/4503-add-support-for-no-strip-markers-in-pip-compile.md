```yaml
number: 4503
title: "Add support for `--no-strip-markers` in `pip compile` output"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/req-txt
created_at: 2024-06-25T01:17:40Z
updated_at: 2024-06-25T20:56:00Z
url: https://github.com/astral-sh/uv/pull/4503
synced_at: 2026-01-10T13:48:28Z
```

# Add support for `--no-strip-markers` in `pip compile` output

---

_Pull request opened by @charliermarsh on 2024-06-25 01:17_

## Summary

This is an intermediary change in enabling universal resolution for `requirements.txt` files. To start, we need to be able to preserve markers in the `requirements.txt` output _and_ propagate those markers, such that if you have a dependency that's only included with a given marker, the transitive dependencies respect that marker too.

Closes #1429.


---

_Label `enhancement` added by @charliermarsh on 2024-06-25 01:33_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/display.rs`:197 on 2024-06-25 09:30_

Can this be simplified with a `let makers = edges.iter() ... .collect(); if !markers.is_empty() { MarkerTree::Or(markers)}`?

---

_@konstin approved on 2024-06-25 10:32_

---

_@charliermarsh reviewed on 2024-06-25 20:44_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/display.rs`:197 on 2024-06-25 20:44_

The nice thing about the current solution is that it doesn't allocate a vec.

---

_Merged by @charliermarsh on 2024-06-25 20:55_

---

_Closed by @charliermarsh on 2024-06-25 20:55_

---

_Branch deleted on 2024-06-25 20:56_

---
