---
number: 1567
title: "A way to get sub-command output when running `uv pip install`"
type: issue
state: closed
author: alex
labels:
  - enhancement
  - tracing
assignees: []
created_at: 2024-02-17T03:24:59Z
updated_at: 2024-09-02T19:46:11Z
url: https://github.com/astral-sh/uv/issues/1567
synced_at: 2026-01-10T01:23:07Z
---

# A way to get sub-command output when running `uv pip install`

---

_Issue opened by @alex on 2024-02-17 03:24_

Currently if you run `uv pip install -v` you get, more or less, a trace of all the operations `uv` does internally.

When doing a `uv pip install 'package @ .'`, and building that package requires compilation (notably, rust compilation!) I'd like to have a way to stream the output from that while running `uv pip install` (the output serves as a form of progress bar for long compilation).

Ideally I'd be able to get this output _without_ getting the full trace output from uv

---

_Comment by @charliermarsh on 2024-02-17 03:32_

Yeah I agree, I also want this.

---

_Label `enhancement` added by @charliermarsh on 2024-02-17 03:32_

---

_Label `tracing` added by @zanieb on 2024-02-17 04:05_

---

_Referenced in [astral-sh/uv#1569](../../astral-sh/uv/issues/1569.md) on 2024-02-17 04:07_

---

_Referenced in [astral-sh/uv#2146](../../astral-sh/uv/issues/2146.md) on 2024-03-04 13:21_

---

_Label `good first issue` added by @konstin on 2024-03-04 13:26_

---

_Assigned to @konstin by @konstin on 2024-03-04 13:51_

---

_Unassigned @konstin by @konstin on 2024-03-04 14:42_

---

_Label `good first issue` removed by @konstin on 2024-03-04 14:42_

---

_Referenced in [astral-sh/uv#2156](../../astral-sh/uv/pulls/2156.md) on 2024-03-04 14:46_

---

_Referenced in [astral-sh/uv#2158](../../astral-sh/uv/issues/2158.md) on 2024-03-04 15:47_

---

_Referenced in [astral-sh/uv#3802](../../astral-sh/uv/issues/3802.md) on 2024-05-24 02:04_

---

_Referenced in [astral-sh/uv#3902](../../astral-sh/uv/issues/3902.md) on 2024-05-29 13:26_

---

_Referenced in [abetlen/llama-cpp-python#1526](../../abetlen/llama-cpp-python/pulls/1526.md) on 2024-06-14 04:01_

---

_Referenced in [astral-sh/uv#6903](../../astral-sh/uv/pulls/6903.md) on 2024-08-31 23:53_

---

_Closed by @charliermarsh on 2024-09-02 19:46_

---
