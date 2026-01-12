```yaml
number: 3720
title: Propagate URL errors in verbatim parsing
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/rel
created_at: 2024-05-21T19:48:24Z
updated_at: 2024-05-21T19:59:00Z
url: https://github.com/astral-sh/uv/pull/3720
synced_at: 2026-01-12T16:05:49Z
```

# Propagate URL errors in verbatim parsing

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/3715.

## Test Plan

```
❯ echo "/../test" | cargo run pip compile -
error: Couldn't parse requirement in `-` at position 0
  Caused by: path could not be normalized: /../test
/../test
^^^^^^^^

❯ echo "-e /../test" | cargo run pip compile -
error: Invalid URL in `-`: `/../test`
  Caused by: path could not be normalized: /../test
  Caused by: cannot normalize a relative path beyond the base directory
```


---

_Label `error messages` added by @charliermarsh on 2024-05-21 19:48_

---

_@zanieb approved on 2024-05-21 19:53_

That was more work than I expected!

---

_Comment by @charliermarsh on 2024-05-21 19:54_

Probably why I didn't do it in the first place >.< But it's the right thing. For some reason all the error threading in `requirements-txt` is sorta painful.

---

_Merged by @charliermarsh on 2024-05-21 19:58_

---

_Closed by @charliermarsh on 2024-05-21 19:59_

---

_Branch deleted on 2024-05-21 19:59_

---
