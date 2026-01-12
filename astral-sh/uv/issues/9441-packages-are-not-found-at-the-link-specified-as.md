```yaml
number: 9441
title: "Packages are NOT found at the link specified as \"index\". But if specify the link as \"find-links\" then the installation is correct."
type: issue
state: closed
author: VladislavSoren
labels:
  - question
assignees: []
created_at: 2024-11-26T14:02:39Z
updated_at: 2024-11-27T15:24:22Z
url: https://github.com/astral-sh/uv/issues/9441
synced_at: 2026-01-12T15:59:50Z
```

# Packages are NOT found at the link specified as "index". But if specify the link as "find-links" then the installation is correct.

---

_@VladislavSoren_

Info:
OS: MacOS
Python: 3.9.18
uv: 0.5.4

if specify the link as "find-links" then the installation is correct
![photo_2024-11-26_16-56-52](https://github.com/user-attachments/assets/db9b84d2-8109-4a5b-a915-da605d945998)

If the same link specified as "index" then package are NOT found
![photo_2024-11-26_16-39-23](https://github.com/user-attachments/assets/ec330505-3a1b-47b4-adfa-6ddd2bbe705f)

**Why can't I define a link as an index so that the packages install correctly?**


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-11-26 14:03_

The `--find-links` and `index` formats aren't the same -- the layouts are not equivalent.

---

_Comment by @VladislavSoren on 2024-11-26 14:51_

How can I correctly specify my source so that it can be used as an index?

---

_Comment by @konstin on 2024-11-27 13:11_

The page needs to follow [PEP 503](https://peps.python.org/pep-0503/) to be usable as an index, otherwise it's only usable as find-links.

---

_Label `question` added by @charliermarsh on 2024-11-27 13:39_

---

_Comment by @charliermarsh on 2024-11-27 14:05_

Yeah it needs to have `index.html` files and such -- it essentially needs to look like an index that you could host with `python -m http.server`.

---

_Comment by @VladislavSoren on 2024-11-27 15:23_

Thank you guys for answers very much !

---

_Closed by @zanieb on 2024-11-27 15:24_

---
