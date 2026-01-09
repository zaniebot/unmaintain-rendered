---
number: 1340
title: "UTF8 files with BOM produce E999 \"unexpected token\""
type: issue
state: closed
author: danielmenzel
labels:
  - bug
assignees: []
created_at: 2022-12-22T20:52:46Z
updated_at: 2022-12-26T12:03:14Z
url: https://github.com/astral-sh/ruff/issues/1340
synced_at: 2026-01-07T13:12:14-06:00
---

# UTF8 files with BOM produce E999 "unexpected token"

---

_Issue opened by @danielmenzel on 2022-12-22 20:52_

I tried using ruff 0.0.191 (Python 3.9 on Windows 10) to check a file that is encoded in UTF8 with BOM (byte order mark). I used the default options for ruff.

Ruff reported a `E999 SyntaxError: Got unexpected token` in line 1 column 2 (and then apparently stopped processing further rules except for line length warnings).

When I changed the encoding of the file to UTF8 **without** BOM, ruff worked as expected and no `E999` was reported.

As a workaround I can just change the encoding of the files to make sure no BOM is present, but I think it would be nice if ruff would be able to handle any of the usual source encodings.

---

_Label `bug` added by @charliermarsh on 2022-12-23 03:55_

---

_Comment by @charliermarsh on 2022-12-23 03:55_

Yeah definitely a bug!

---

_Comment by @squiddy on 2022-12-23 19:27_

I've raised https://github.com/RustPython/RustPython/issues/4353 for this.

---

_Referenced in [astral-sh/ruff#1379](../../astral-sh/ruff/pulls/1379.md) on 2022-12-26 07:13_

---

_Closed by @charliermarsh on 2022-12-26 12:03_

---
