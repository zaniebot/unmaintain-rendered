---
number: 412
title: Support the html PEP 503 simple repository api
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - wish
assignees: []
created_at: 2023-11-13T13:55:31Z
updated_at: 2023-12-24T16:04:02Z
url: https://github.com/astral-sh/uv/issues/412
synced_at: 2026-01-07T13:12:16-06:00
---

# Support the html PEP 503 simple repository api

---

_Issue opened by @konstin on 2023-11-13 13:55_

This involves:
 - [ ] Finding or writing a parser for the simple repository [PEP 503](https://peps.python.org/pep-0503/) html format
 - [ ] Introduce a new type that abstract over the pypi `File` and the few data that the html gives use 
   - [ ] Support hashes
   - [ ] Support `data-requires-python` 
   - [ ] Support `data-yanked` [PEP 592](https://peps.python.org/pep-0592/)
 - [ ] Support [PEP 691](https://peps.python.org/pep-0691/#version-format-selection) content negotiation

---

_Comment by @charliermarsh on 2023-11-13 14:06_

Why and when is this necessary?

---

_Comment by @konstin on 2023-11-13 14:28_

The standard format for indices is still html, see e.g. https://download.pytorch.org/whl/cu118, the torch index, so

https://github.com/astral-sh/puffin/blob/81c9cd0d4ad44920e3785fcb4a499e4f559c77f2/crates/puffin-client/src/client.rs#L162

will fail (https://download.pytorch.org/whl/cu118?format=application/vnd.pypi.simple.v1+json still outputs html - see also https://peps.python.org/pep-0691/#version-format-selection) for those and we need the html parsing which gives us also less data instead.

So the short is answer is: Whenever the user selects an index that doesn't support json output

---

_Label `enhancement` added by @charliermarsh on 2023-11-13 14:29_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-11-13 14:29_

---

_Label `wish` added by @charliermarsh on 2023-11-13 14:29_

---

_Referenced in [astral-sh/uv#428](../../astral-sh/uv/pulls/428.md) on 2023-11-15 18:53_

---

_Referenced in [astral-sh/uv#705](../../astral-sh/uv/issues/705.md) on 2023-12-19 18:08_

---

_Referenced in [astral-sh/uv#706](../../astral-sh/uv/issues/706.md) on 2023-12-19 18:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-21 02:49_

---

_Comment by @charliermarsh on 2023-12-21 02:49_

I'm on this one. Will take me a bit.

---

_Referenced in [astral-sh/uv#719](../../astral-sh/uv/pulls/719.md) on 2023-12-24 15:24_

---

_Closed by @charliermarsh on 2023-12-24 16:04_

---
