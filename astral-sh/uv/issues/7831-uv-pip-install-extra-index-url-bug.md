---
number: 7831
title: uv pip install ... --extra-index-url bug
type: issue
state: closed
author: Ruhrozz
labels:
  - question
assignees: []
created_at: 2024-10-01T09:44:32Z
updated_at: 2024-10-08T07:02:16Z
url: https://github.com/astral-sh/uv/issues/7831
synced_at: 2026-01-10T01:24:19Z
---

# uv pip install ... --extra-index-url bug

---

_Issue opened by @Ruhrozz on 2024-10-01 09:44_

I have specific torch version in my extra-index-url and some other packages. When I trying to install my python library with `uv pip install -e .` it searches `torch` in extra-index-url and throws an error that you have a single torch version which does not meet the requirements.

I expected that it will try to find it on PyPI, because pip itself cope with that situation somehow

I don't know how to make a reproducible example for it =(

---

_Renamed from "extra-index-url bug" to "uv pip install ... --extra-index-url bug" by @Ruhrozz on 2024-10-01 09:45_

---

_Comment by @zanieb on 2024-10-01 12:44_

Hi! Sounds like your encountering this [intentional difference from pip](https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes).

---

_Label `question` added by @zanieb on 2024-10-01 12:45_

---

_Comment by @Ruhrozz on 2024-10-01 15:00_

Thanks for help!

---

_Closed by @Ruhrozz on 2024-10-01 15:00_

---

_Comment by @FelixSchwarz on 2024-10-08 07:02_

Just wanted to mention that I encountered the same issue and @zanieb's answer was helpful for me as well :-)

---
