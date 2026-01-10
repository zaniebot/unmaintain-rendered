```yaml
number: 5165
title: Make entrypoint writes atomic to avoid overwriting symlinks
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/atomic
created_at: 2024-07-17T19:28:57Z
updated_at: 2024-07-17T19:44:27Z
url: https://github.com/astral-sh/uv/pull/5165
synced_at: 2026-01-10T13:42:52Z
```

# Make entrypoint writes atomic to avoid overwriting symlinks

---

_Pull request opened by @charliermarsh on 2024-07-17 19:28_

## Summary

It turns out that if `path` is a symlink, `File::create(path)?.write_all(content.as_ref())?` will overwrite the _target_ file. That means an entrypoint named `python` would actually overwrite the user's source Python executable, which is symlinked into the virtual environment.

This PR replaces that code with our atomic write method.

Closes https://github.com/astral-sh/uv/issues/5152.

## Test Plan

I ran through the test plan `https://github.com/astral-sh/uv/issues/5152`, but used an executable named `bar` linked to `foo.txt` instead...


---

_Label `bug` added by @charliermarsh on 2024-07-17 19:29_

---

_Marked ready for review by @charliermarsh on 2024-07-17 19:29_

---

_@konstin approved on 2024-07-17 19:42_

---

_Merged by @charliermarsh on 2024-07-17 19:44_

---

_Closed by @charliermarsh on 2024-07-17 19:44_

---

_Branch deleted on 2024-07-17 19:44_

---
