```yaml
number: 8576
title: UV Python Install will not find Homebrew Installed dylibs
type: issue
state: closed
author: sonnen-coviance
labels:
  - duplicate
assignees: []
created_at: 2024-10-25T20:21:31Z
updated_at: 2024-10-25T20:42:31Z
url: https://github.com/astral-sh/uv/issues/8576
synced_at: 2026-01-12T15:59:29Z
```

# UV Python Install will not find Homebrew Installed dylibs

---

_@sonnen-coviance_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Currently using `uv` on a MacBook Pro with an Apple processor and am running into some issues using `uv python install`.

The issue is that C libraries installed by `homebrew` are not found by Python libraries when using Python installed through `uv python install` but are found when `uv` uses a version of python installed via homebrew.

It seems like Homebrew does some hacking during the Python install process in order to get dylibs installed via Homebrew to be found.

See:
https://github.com/Homebrew/homebrew-core/blob/ed6d3f73a6cd3d779d0254f4ae5ba39b99d3217b/Formula/python@3.10.rb#L185-L192


---

_Comment by @zanieb on 2024-10-25 20:25_

Hi! Yeah Homebrew special-cases their libraries in their Python distributions. It's unclear if we should do the same in ours.

This is a duplicate of

- https://github.com/astral-sh/uv/issues/7896
- https://github.com/astral-sh/uv/issues/7764

---

_Label `duplicate` added by @zanieb on 2024-10-25 20:25_

---

_Comment by @sonnen-coviance on 2024-10-25 20:27_

Thank you for pointing me to that! I think it would be nice behavior if UV could handle this. It was very confusing to troubleshoot across a few projects since UV picked up some already installed versions of python via brew but required installing python for others. This caused some python deps to fail to find libs in some projects but not others.

---

_Comment by @zanieb on 2024-10-25 20:34_

Let's track in #7764.

Yeah I can understand how that would be confusing. When does it fail? At runtime?


---

_Closed by @zanieb on 2024-10-25 20:34_

---

_Comment by @sonnen-coviance on 2024-10-25 20:41_

@zanieb Yep, at runtime when one of the python dependencies tries to call a C lib. 

I'm specifically running into it with the clib of `libmagic` and the python package `filemagic`. I could make a minimal example with some logs if that would be helpful.

---

_Comment by @zanieb on 2024-10-25 20:42_

Unfortunately that's hard for us to add a special case error message (e.g., like we do for build errors). A minimal example can't hurt! Just post it in the other issue please.

---
