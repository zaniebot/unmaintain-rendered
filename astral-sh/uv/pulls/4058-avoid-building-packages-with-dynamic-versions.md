```yaml
number: 4058
title: Avoid building packages with dynamic versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/static
created_at: 2024-06-05T17:56:03Z
updated_at: 2024-06-05T18:11:59Z
url: https://github.com/astral-sh/uv/pull/4058
synced_at: 2026-01-12T16:06:01Z
```

# Avoid building packages with dynamic versions

---

_@charliermarsh_

## Summary

This PR separates "gathering the requirements" from the rest of the metadata (e.g., version), which isn't required when installing a package's _dependencies_ (as opposed to installing the package itself). It thus ensures that we don't need to build a package when a static `pyproject.toml` is provided in `pip compile`.

Closes https://github.com/astral-sh/uv/issues/4040.


---

_Label `performance` added by @charliermarsh on 2024-06-05 17:56_

---

_Comment by @charliermarsh on 2024-06-05 17:56_

Ok, I'm happy with how little special-casing this required, even if it requires ~200 LoC.

---

_Marked ready for review by @charliermarsh on 2024-06-05 17:56_

---

_Merged by @charliermarsh on 2024-06-05 18:11_

---

_Closed by @charliermarsh on 2024-06-05 18:11_

---

_Branch deleted on 2024-06-05 18:11_

---
