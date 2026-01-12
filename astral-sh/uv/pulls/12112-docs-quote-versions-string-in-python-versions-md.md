```yaml
number: 12112
title: "Docs : Quote versions string in python-versions.md"
type: pull_request
state: merged
author: GCHQDeveloper314
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-fix-typo
created_at: 2025-03-11T11:59:17Z
updated_at: 2025-03-11T14:03:01Z
url: https://github.com/astral-sh/uv/pull/12112
synced_at: 2026-01-12T16:10:08Z
```

# Docs : Quote versions string in python-versions.md

---

_@GCHQDeveloper314_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The command `uv python find >=3.11` doesn't work . The version should be quoted otherwise the terminal interprets the `>`  and pipes output to a file named `=3.11`. I've used single quotes as used on line 90 of this file.

## Test Plan

Locally


---

_Renamed from "Quote versions string in python-versions.md" to "Docs : Quote versions string in python-versions.md" by @GCHQDeveloper314 on 2025-03-11 11:59_

---

_Review requested from @zanieb by @konstin on 2025-03-11 12:13_

---

_Label `documentation` added by @zanieb on 2025-03-11 14:02_

---

_@zanieb approved on 2025-03-11 14:02_

Thanks!

---

_Merged by @zanieb on 2025-03-11 14:03_

---

_Closed by @zanieb on 2025-03-11 14:03_

---
