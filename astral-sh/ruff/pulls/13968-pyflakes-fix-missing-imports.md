```yaml
number: 13968
title: "[pyflakes] Fix missing imports"
type: pull_request
state: closed
author: gpilikin
labels: []
assignees: []
draft: true
base: main
head: fix/unimported_submodules
created_at: 2024-10-28T18:41:44Z
updated_at: 2024-10-29T05:53:28Z
url: https://github.com/astral-sh/ruff/pull/13968
synced_at: 2026-01-10T20:59:37Z
```

# [pyflakes] Fix missing imports

---

_Pull request opened by @gpilikin on 2024-10-28 18:41_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Mark as unresolved the use of Non-imported submodules.

```
import foo.bar
import foo.baz

fu = foo.ban.asd() # not imported and not marked.
...
```

## Test Plan

<!-- How was it tested? -->
cargo test

---

_Closed by @gpilikin on 2024-10-28 18:42_

---

_Renamed from "Fix/unimported submodules" to "[pyflakes] Fix missing submodules" by @gpilikin on 2024-10-29 05:53_

---

_Renamed from "[pyflakes] Fix missing submodules" to "[pyflakes] Fix missing imports" by @gpilikin on 2024-10-29 05:53_

---
