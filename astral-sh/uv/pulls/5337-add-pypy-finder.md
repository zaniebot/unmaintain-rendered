```yaml
number: 5337
title: Add PyPy finder
type: pull_request
state: merged
author: j178
labels:
  - enhancement
  - pypy
  - preview
assignees: []
merged: true
base: main
head: pypy-finder
created_at: 2024-07-23T15:35:29Z
updated_at: 2024-07-23T23:47:15Z
url: https://github.com/astral-sh/uv/pull/5337
synced_at: 2026-01-12T16:06:46Z
```

# Add PyPy finder

---

_@j178_

## Summary

This PR adds PyPy finder and adds PyPy to uv managed Python versions.

## Test Plan

```console
$ cargo run -- python install
```


---

_@zanieb reviewed on 2024-07-23 15:38_

---

_Review comment by @zanieb on `.python-versions`:8 on 2024-07-23 15:38_

Maybe just use `uv python install` for these in the PyPy integration jobs we have in CI instead? Are you going to make use of these in unit tests?

---

_Comment by @zanieb on 2024-07-23 15:38_

Awesome!

---

_Assigned to @zanieb by @zanieb on 2024-07-23 15:38_

---

_@j178 reviewed on 2024-07-23 16:24_

---

_Review comment by @j178 on `.python-versions`:8 on 2024-07-23 16:24_

Thanks for pointing out, I wasn't aware of that.

---

_@zanieb reviewed on 2024-07-23 16:31_

---

_Review comment by @zanieb on `docs/python-versions.md`:27 on 2024-07-23 16:31_

I would note something like "uv supports managed CPython and PyPy versions"

---

_@zanieb reviewed on 2024-07-23 16:47_

---

_Review comment by @zanieb on `docs/python-versions.md`:27 on 2024-07-23 16:47_

Maybe we should note this elsewhere... like in the installs section?

---

_Review comment by @zanieb on `docs/python-versions.md`:23 on 2024-07-23 16:48_

I wonder if we should be enumerating these all here? Maybe we should start a separate section for supported implementations — this is also missing GraalPy and I don't want to to be too verbose. 

---

_@zanieb reviewed on 2024-07-23 16:48_

---

_@j178 reviewed on 2024-07-23 17:04_

---

_Review comment by @j178 on `docs/python-versions.md`:27 on 2024-07-23 17:04_

I added `uv bundles a list of downloadable CPython and PyPy distributions for macOS, Linux, and Windows.` in `Installing a Python version` section, what do you think?

---

_Marked ready for review by @j178 on 2024-07-23 17:26_

---

_@zanieb approved on 2024-07-23 19:57_

Thank you! Nice work.

---

_Merged by @zanieb on 2024-07-23 19:58_

---

_Closed by @zanieb on 2024-07-23 19:58_

---

_Label `enhancement` added by @zanieb on 2024-07-23 19:58_

---

_Label `preview` added by @zanieb on 2024-07-23 19:58_

---

_Label `pypy` added by @zanieb on 2024-07-23 19:58_

---

_Branch deleted on 2024-07-23 23:47_

---
