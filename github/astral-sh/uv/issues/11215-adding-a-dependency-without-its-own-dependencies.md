---
number: 11215
title: "Adding a dependency without its own dependencies  like `--no-deps` in pip"
type: issue
state: open
author: harisarang
labels:
  - question
assignees: []
created_at: 2025-02-04T13:05:00Z
updated_at: 2025-02-04T21:36:10Z
url: https://github.com/astral-sh/uv/issues/11215
synced_at: 2026-01-07T13:12:18-06:00
---

# Adding a dependency without its own dependencies  like `--no-deps` in pip

---

_Issue opened by @harisarang on 2025-02-04 13:05_

### Question

I would like add a package named `scann` where `tensorflow` is a dependency. But I would like to add `scann` with `tensorflow`
```bash
uv pip install scann --no-deps
```
This command doesn't install `tensorflow`
But is there way I could run the similar with `uv add` with tensorflow ?

### Platform

Ubuntu 22.04 (amd)

### Version

v0.5.27

---

_Label `question` added by @harisarang on 2025-02-04 13:05_

---

_Renamed from "Adding a dependency without its own dependencies `--no-deps` like no deps in pip" to "Adding a dependency without its own dependencies  like `--no-deps` in pip" by @harisarang on 2025-02-04 13:05_

---

_Comment by @zanieb on 2025-02-04 21:36_

I'm not sure the semantics of this make sense with `uv add`. If the package says it depends on `tensorflow`, we respect that.

The best you can do now is override the metadata for `scann` to say it has no dependencies

https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata

---
