```yaml
number: 1072
title: Add documentation for PYTHONPATH config
type: pull_request
state: closed
author: natelust
labels:
  - documentation
assignees: []
base: main
head: main
created_at: 2025-08-21T01:49:55Z
updated_at: 2025-12-29T09:34:09Z
url: https://github.com/astral-sh/ty/pull/1072
synced_at: 2026-01-12T15:54:27Z
```

# Add documentation for PYTHONPATH config

---

_@natelust_

Add documentation on how to configure ty to read the paths defined in the PYTHONPATH environment variable.

## Summary

I added the ability to find modules from paths defined on PYTHONPATH. I opened a [PR](https://github.com/astral-sh/ruff/pull/20016) in the ruff repo. This is the accompanying documentation changes.

## Test Plan
This is only documentation changes, and was verified by building and reading the documentation locally


---

_Label `documentation` added by @MichaReiser on 2025-08-21 12:50_

---

_Comment by @MichaReiser on 2025-12-29 09:34_

Thanks for the documentation PR. I'll close this PR, given that we didn't end up introducing this setting in ty. 

---

_Closed by @MichaReiser on 2025-12-29 09:34_

---
