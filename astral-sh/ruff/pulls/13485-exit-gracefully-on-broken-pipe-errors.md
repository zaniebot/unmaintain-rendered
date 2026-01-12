```yaml
number: 13485
title: Exit gracefully on broken pipe errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/grace
created_at: 2024-09-23T13:31:38Z
updated_at: 2024-09-23T13:58:00Z
url: https://github.com/astral-sh/ruff/pull/13485
synced_at: 2026-01-12T15:55:44Z
```

# Exit gracefully on broken pipe errors

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes https://github.com/astral-sh/ruff/issues/13483.

Closes https://github.com/astral-sh/ruff/issues/13442.

## Test Plan

```
❯ cargo run analyze graph ../django | head -n 10
   Compiling ruff v0.6.7 (/Users/crmarsh/workspace/ruff/crates/ruff)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.63s
     Running `target/debug/ruff analyze graph ../django`
warning: `ruff analyze graph` is experimental and may change without warning
{
  "/Users/crmarsh/workspace/django/django/__init__.py": [
    "/Users/crmarsh/workspace/django/django/apps/__init__.py",
    "/Users/crmarsh/workspace/django/django/conf/__init__.py",
    "/Users/crmarsh/workspace/django/django/urls/__init__.py",
    "/Users/crmarsh/workspace/django/django/utils/log.py",
    "/Users/crmarsh/workspace/django/django/utils/version.py"
  ],
  "/Users/crmarsh/workspace/django/django/__main__.py": [
    "/Users/crmarsh/workspace/django/django/core/management/__init__.py"
```


---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-23 13:31_

---

_Label `bug` added by @charliermarsh on 2024-09-23 13:31_

---

_Review comment by @BurntSushi on `crates/ruff/src/main.rs`:93 on 2024-09-23 13:40_

I think you want to drop this line. :P 

---

_@BurntSushi approved on 2024-09-23 13:42_

Ah nice, much better!

---

_@charliermarsh reviewed on 2024-09-23 13:43_

---

_Review comment by @charliermarsh on `crates/ruff/src/main.rs`:93 on 2024-09-23 13:43_

Oops :)

---

_Merged by @charliermarsh on 2024-09-23 13:48_

---

_Closed by @charliermarsh on 2024-09-23 13:48_

---

_Branch deleted on 2024-09-23 13:48_

---

_Comment by @github-actions[bot] on 2024-09-23 13:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
