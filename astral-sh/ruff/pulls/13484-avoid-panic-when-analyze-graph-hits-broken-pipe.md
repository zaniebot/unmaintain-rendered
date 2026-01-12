```yaml
number: 13484
title: Avoid panic when analyze graph hits broken pipe
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pipe
created_at: 2024-09-23T13:29:43Z
updated_at: 2024-09-23T13:43:28Z
url: https://github.com/astral-sh/ruff/pull/13484
synced_at: 2026-01-12T15:55:44Z
```

# Avoid panic when analyze graph hits broken pipe

---

_@charliermarsh_

## Summary

I think we should also make the change that @BurntSushi recommended in the linked issue, but this gets rid of the panic.

See: https://github.com/astral-sh/ruff/issues/13483

See: https://github.com/astral-sh/ruff/issues/13442

## Test Plan

```
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
ruff failed
  Cause: Broken pipe (os error 32)
```


---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-23 13:29_

---

_Label `bug` added by @charliermarsh on 2024-09-23 13:29_

---

_@BurntSushi approved on 2024-09-23 13:33_

I think this is an improvement over panicking, but the "ruff failed" message and the failure exit code is probably non-ideal and probably not what the user expects.

---

_Comment by @charliermarsh on 2024-09-23 13:38_

That’s covered in https://github.com/astral-sh/ruff/pull/13485 since it affects more than this subcommand.

---

_Merged by @charliermarsh on 2024-09-23 13:43_

---

_Closed by @charliermarsh on 2024-09-23 13:43_

---

_Branch deleted on 2024-09-23 13:43_

---

_Comment by @github-actions[bot] on 2024-09-23 13:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
