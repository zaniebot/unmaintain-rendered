```yaml
number: 8053
title: "[bug] [formatter] parenthesized single list comp var chokes formatter"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2023-10-18T19:35:17Z
updated_at: 2023-10-18T23:14:00Z
url: https://github.com/astral-sh/ruff/issues/8053
synced_at: 2026-01-12T15:54:47Z
```

# [bug] [formatter] parenthesized single list comp var chokes formatter

---

_@smackesey_

The below line is legal python but chokes the formatter:

```
    return all(filepath.startswith(DAGIT_CLOUD_PATH) for (filepath) in diff_files)
```

Error:

```
error: Failed to format .buildkite/utils.py: syntax error: Expected `in` keyword between the `target` and `iter`.
```


---

_Comment by @charliermarsh on 2023-10-18 20:23_

Taking a look now.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-18 20:23_

---

_Label `bug` added by @charliermarsh on 2023-10-18 20:23_

---

_Closed by @charliermarsh on 2023-10-18 23:14_

---
