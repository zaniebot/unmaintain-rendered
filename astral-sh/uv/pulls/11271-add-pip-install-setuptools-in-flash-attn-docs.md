```yaml
number: 11271
title: add pip install setuptools in flash-attn docs
type: pull_request
state: merged
author: samsja
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-02-06T01:52:29Z
updated_at: 2025-02-06T01:59:56Z
url: https://github.com/astral-sh/uv/pull/11271
synced_at: 2026-01-12T16:09:45Z
```

# add pip install setuptools in flash-attn docs

---

_@samsja_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I followed the docs to install flash-attn with uv and got the following issue 
```uv sync
Resolved 27 packages in 12ms
  × Failed to build `flash-attn==2.7.4.post1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
      ModuleNotFoundError: No module named 'setuptools'

      hint: This usually indicates a problem with the package or the build environment.
```

installing setuptools before running uv sync as done with torch helps fix it.
## Test Plan

I tested locally before it failed to install, after it worked

---

_@charliermarsh approved on 2025-02-06 01:59_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2025-02-06 01:59_

---

_Merged by @charliermarsh on 2025-02-06 01:59_

---

_Closed by @charliermarsh on 2025-02-06 01:59_

---
