```yaml
number: 14241
title: "[`pylint`] Implement `shallow-copy-environ` (`W1507`)"
type: pull_request
state: merged
author: harupy
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: shallow-copy-environ
created_at: 2024-11-10T08:05:31Z
updated_at: 2024-11-15T10:31:38Z
url: https://github.com/astral-sh/ruff/pull/14241
synced_at: 2026-01-10T20:50:57Z
```

# [`pylint`] Implement `shallow-copy-environ` (`W1507`)

---

_Pull request opened by @harupy on 2024-11-10 08:05_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Related to #970. Implement [`shallow-copy-environ / W1507`](https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/shallow-copy-environ.html).

## Test Plan

<!-- How was it tested? -->

Unit test

---

_Comment by @github-actions[bot] on 2024-11-10 08:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sbrugman reviewed on 2024-11-10 12:24_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/shallow_copy_environ.rs`:14 on 2024-11-10 12:24_

Nit:
```suggestion
/// effects on original object. See [Python issue 15373: `copy.copy()` does not properly copy `os.environment`](https://bugs.python.org/issue15373) for reference.
```


---

_@sbrugman reviewed on 2024-11-10 12:26_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/shallow_copy_environ.rs`:31 on 2024-11-10 12:26_

```suggestion
/// ```
///
/// ## References
/// - [Python documentation: `copy` — Shallow and deep copy operations](https://docs.python.org/3/library/copy.html)
/// - [Python documentation: `os.environ`](https://docs.python.org/3/library/os.html#os.environ)
```

---

_@charliermarsh approved on 2024-11-10 22:52_

Thanks!

---

_Label `rule` added by @charliermarsh on 2024-11-10 22:53_

---

_Label `preview` added by @charliermarsh on 2024-11-10 22:53_

---

_Merged by @charliermarsh on 2024-11-10 22:58_

---

_Closed by @charliermarsh on 2024-11-10 22:58_

---

_Branch deleted on 2024-11-11 00:39_

---

_Renamed from "Implement `shallow-copy-environ / W1507`" to "[`pylint`] Implement `shallow-copy-environ` (`W1507`)" by @dhruvmanila on 2024-11-15 10:31_

---
