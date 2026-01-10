```yaml
number: 18217
title: add full option name in formatter warning
type: pull_request
state: merged
author: CodeMan62
labels:
  - cli
assignees: []
merged: true
base: main
head: codeman/18199
created_at: 2025-05-20T12:14:07Z
updated_at: 2025-05-20T14:26:47Z
url: https://github.com/astral-sh/ruff/pull/18217
synced_at: 2026-01-10T18:51:02Z
```

# add full option name in formatter warning

---

_Pull request opened by @CodeMan62 on 2025-05-20 12:14_

## Summary
So this PR fixes #18199 in the issue mentioned the error message is as followed
```
$ ruff format --check .build/mytool.py
warning: The following rule may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling this rule, either by removing it from the `select` or `extend-select` configuration, or adding it to the `ignore` configuration.
 ```
the new message contains full option name as guided in issue the new message looks like this 
```
$ ruff format --check .build/mytool.py
warning: The following rule may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling this rule, either by removing it from the `select` or `extend-select` configuration, or adding it to the `lint.ignore` configuration.
```
## Test Plan
I just did manual testing 

---

_Comment by @CodeMan62 on 2025-05-20 13:19_

This time tests must pass because last time I have give wrong path but this time this is right and good to go 
I was wrong I am refactoring 

---

_Converted to draft by @CodeMan62 on 2025-05-20 13:30_

---

_Review comment by @MichaReiser on `crates/ruff/tests/format.rs`:865 on 2025-05-20 13:55_

What you change here is the test. You need to change the implementation here https://github.com/astral-sh/ruff/blob/fa628018b28fa018d7a0e109223186172ef91275/crates/ruff/src/commands/format.rs#L824 (and other lines)

---

_@MichaReiser reviewed on 2025-05-20 13:55_

---

_@CodeMan62 reviewed on 2025-05-20 13:57_

---

_Review comment by @CodeMan62 on `crates/ruff/tests/format.rs`:865 on 2025-05-20 13:57_

thanks i was just finding it.
EDIT:- Done

---

_Marked ready for review by @CodeMan62 on 2025-05-20 14:02_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:825 on 2025-05-20 14:04_

Let's use the full qualified name for all settings (same below)

```suggestion
                "The following rule may cause conflicts when used with the formatter: {rule}. To avoid unexpected behavior, we recommend disabling this rule, either by removing it from the `lint.select` or `lint.extend-select` configuration, or adding it to the `lint.ignore` configuration."
```

---

_@MichaReiser approved on 2025-05-20 14:05_

Thanks, I've a small suggestion but this otherwise looks good.

---

_Label `cli` added by @dhruvmanila on 2025-05-20 14:05_

---

_@CodeMan62 reviewed on 2025-05-20 14:07_

---

_Review comment by @CodeMan62 on `crates/ruff/src/commands/format.rs`:825 on 2025-05-20 14:07_

Okk
EDIT:- DONE

---

_Comment by @github-actions[bot] on 2025-05-20 14:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-05-20 14:26_

Thanks

---

_Merged by @MichaReiser on 2025-05-20 14:26_

---

_Closed by @MichaReiser on 2025-05-20 14:26_

---
