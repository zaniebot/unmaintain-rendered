```yaml
number: 10499
title: "Docs: Link inline settings when not part of options section"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
  - documentation
assignees: []
merged: true
base: main
head: link-inline-settings
created_at: 2024-03-21T02:03:42Z
updated_at: 2024-03-31T16:00:40Z
url: https://github.com/astral-sh/ruff/pull/10499
synced_at: 2026-01-12T15:55:32Z
```

# Docs: Link inline settings when not part of options section

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
 
Some contributors have referenced settings in their documentation without adding the settings to an options section, this has lead to some rendering issues (#10427). This PR addresses this looking for potential inline links to settings, cross-checking them with the options sections, and then linking them anyway if they are not found.

Resolves #10427.

## Test Plan

Manually verified that the correct modifications were made and no docs were broken.


---

_Comment by @github-actions[bot] on 2024-03-21 02:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `bug` added by @charliermarsh on 2024-03-21 04:33_

---

_Label `documentation` added by @charliermarsh on 2024-03-21 04:33_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_docs.rs`:151 on 2024-03-21 08:02_

Nit: I first assumed that `options` points to `Options::metadata`, which isn't the case. What I understand is that `options` stores all linked or referenced options, maybe rename to `linked_options` or `referenced_options`?

---

_@MichaReiser reviewed on 2024-03-21 08:02_

---

_@MichaReiser approved on 2024-03-21 08:06_

Nice, thank you

---

_@charliermarsh reviewed on 2024-03-21 16:24_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_docs.rs`:151 on 2024-03-21 16:24_

I renamed to `referenced_options`.

---

_Merged by @charliermarsh on 2024-03-21 16:33_

---

_Closed by @charliermarsh on 2024-03-21 16:33_

---

_Branch deleted on 2024-03-31 16:00_

---
