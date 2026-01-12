```yaml
number: 11429
title: Allow source distributions to produce wheels with +local suffixes
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2025-02-11T21:45:48Z
updated_at: 2025-02-11T22:26:42Z
url: https://github.com/astral-sh/uv/pull/11429
synced_at: 2026-01-12T16:09:50Z
```

# Allow source distributions to produce wheels with +local suffixes

---

_@charliermarsh_

## Summary

We currently enforce that if you do `uv pip install ./dist/iniconfig-1.0.0.tar.gz`, the build _must_ produce a wheel like `iniconfig-1.0.0-py3-none-any.whl` (i.e., the name and version must match). It turns out some packages produce a wheel that has a local suffix on it, like `vllm`. This PR makes the check a little more permissive in that we now accept `1.0.0` or that version with a local suffix (e.g., `1.0.0+cpu`). I don't love this practice, but we already relaxed this check when _installing_ a wheel, so this seems reasonable:

https://github.com/astral-sh/uv/blob/5e15881dccc9201c10696cfcaf3a3e1ad8081f31/crates/uv-install-wheel/src/install.rs#L50-L52

Note that this is _still_ stricter than pip. pip seems to only require that the package name is the same (i.e., `iniconfig` matches `iniconfig`; but they'll happily install a wheel like `iniconfig-2.0.0-py3-none-any.whl` given `./dist/iniconfig-1.0.0.tar.gz`).

Closes https://github.com/astral-sh/uv/issues/11038.


---

_Label `compatibility` added by @charliermarsh on 2025-02-11 21:45_

---

_Marked ready for review by @charliermarsh on 2025-02-11 21:50_

---

_Merged by @charliermarsh on 2025-02-11 22:26_

---

_Closed by @charliermarsh on 2025-02-11 22:26_

---

_Branch deleted on 2025-02-11 22:26_

---
