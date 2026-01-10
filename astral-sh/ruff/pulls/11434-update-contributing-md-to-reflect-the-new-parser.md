```yaml
number: 11434
title: Update CONTRIBUTING.md to reflect the new parser
type: pull_request
state: merged
author: lemeb
labels:
  - documentation
assignees: []
merged: true
base: main
head: contributing-md-fix
created_at: 2024-05-15T12:26:00Z
updated_at: 2024-05-15T14:36:28Z
url: https://github.com/astral-sh/ruff/pull/11434
synced_at: 2026-01-10T22:05:26Z
```

# Update CONTRIBUTING.md to reflect the new parser

---

_Pull request opened by @lemeb on 2024-05-15 12:26_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

CONTRIBUTING.md says that `cargo dev print-ast` uses the old RuffPython parser, even though, as far as I can tell, it uses the shiny new parser. This PR fixes this.

## Test Plan

CI jobs should do the trick -- I didn't modify any code.

---

_@dhruvmanila reviewed on 2024-05-15 14:33_

---

_Review comment by @dhruvmanila on `CONTRIBUTING.md`:641 on 2024-05-15 14:33_

```suggestion
- `cargo dev print-ast <file>`: Print the AST of a python file using Ruff's
    [Python parser](https://github.com/astral-sh/ruff/tree/main/crates/ruff_python_parser).
```

---

_@dhruvmanila approved on 2024-05-15 14:34_

Thank you!

---

_Label `documentation` added by @dhruvmanila on 2024-05-15 14:34_

---

_Renamed from "updating CONTRIBUTING.md to reflect the new parser" to "Update CONTRIBUTING.md to reflect the new parser" by @dhruvmanila on 2024-05-15 14:34_

---

_Merged by @dhruvmanila on 2024-05-15 14:36_

---

_Closed by @dhruvmanila on 2024-05-15 14:36_

---
