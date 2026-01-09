---
number: 325
title: Support selecting and ignoring groups of codes
type: issue
state: closed
author: andersk
labels: []
assignees: []
created_at: 2022-10-04T19:48:36Z
updated_at: 2022-10-28T22:19:59Z
url: https://github.com/astral-sh/ruff/issues/325
synced_at: 2026-01-07T13:12:14-06:00
---

# Support selecting and ignoring groups of codes

---

_Issue opened by @andersk on 2022-10-04 19:48_

Flake8 supports options like

* `flake8 --ignore=E` (ignore all `E???` rules)
* `flake8 --select=E5` (enable all `E5??` rules)
* `flake8 --select=E,E50 --ignore=E5,E501` (select all `E???` rules, except `E5??` which are ignored, except `E50?` which are selected, except `E501` which is ignored)

It might be nice to support this or something similar.

---

_Label `enhancement` added by @charliermarsh on 2022-10-04 20:32_

---

_Comment by @charliermarsh on 2022-10-04 20:33_

Yeah that's nice. If implemented, we should try to retain the strict-checking nature of the CLI (such that we error if you provide `--ignore=G`, and no codes match `G???`).

---

_Referenced in [astral-sh/ruff#438](../../astral-sh/ruff/issues/438.md) on 2022-10-18 21:19_

---

_Referenced in [astral-sh/ruff#423](../../astral-sh/ruff/issues/423.md) on 2022-10-26 16:21_

---

_Comment by @charliermarsh on 2022-10-26 16:23_

I'm also wondering if there are other linters (even from other ecosystems) that do something more intuitive or ergonomic than the prefix-based selection.

---

_Referenced in [astral-sh/ruff#493](../../astral-sh/ruff/pulls/493.md) on 2022-10-28 01:56_

---

_Closed by @charliermarsh on 2022-10-28 22:19_

---

_Referenced in [codegrande/ruff#4](../../codegrande/ruff/pulls/4.md) on 2024-05-17 07:07_

---
