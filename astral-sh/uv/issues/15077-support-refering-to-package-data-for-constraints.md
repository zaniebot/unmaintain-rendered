```yaml
number: 15077
title: support refering to package data for constraints
type: issue
state: open
author: RonnyPfannschmidt
labels:
  - enhancement
assignees: []
created_at: 2025-08-05T08:09:45Z
updated_at: 2025-08-05T08:09:45Z
url: https://github.com/astral-sh/uv/issues/15077
synced_at: 2026-01-12T16:02:03Z
```

# support refering to package data for constraints

---

_@RonnyPfannschmidt_

### Summary

in my use case i have a python package that ships managed python constraints file as package data - currently we have to wrap pip/uv into a script that resolves the constraint filename to a absolute path then invoking the package manager that uses it

for more robust operation i'd like to be able to use something like `package://my.package/the/file` to ensure the package data is looked up in the current python

### Example

_No response_

---

_Label `enhancement` added by @RonnyPfannschmidt on 2025-08-05 08:09_

---
