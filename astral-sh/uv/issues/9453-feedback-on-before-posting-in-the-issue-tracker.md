---
number: 9453
title: "Feedback on \"Before posting in the issue tracker\""
type: issue
state: open
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2024-11-26T21:19:03Z
updated_at: 2025-05-23T00:51:27Z
url: https://github.com/astral-sh/uv/issues/9453
synced_at: 2026-01-10T01:24:41Z
---

# Feedback on "Before posting in the issue tracker"

---

_Issue opened by @zanieb on 2024-11-26 21:19_

Hey! We're giving https://github.com/astral-sh/uv/issues/9452 a try to help guide people towards common issues and set expectations for interactions in the issue tracker.

That issue is locked to avoid distracting commentary, but feedback is very welcome!

---

_Label `documentation` added by @zanieb on 2024-11-26 21:19_

---

_Referenced in [astral-sh/uv#9452](../../astral-sh/uv/issues/9452.md) on 2024-11-26 21:19_

---

_Comment by @cthoyt on 2024-11-26 22:01_

Minor typo in `<details> bock` that should be `<details> block`

You might want to mention not to take screenshots and that copy/pasting out of the terminal is more helpful. I manage a ML library where people send screenshots and it's no fun at all 😓 



---

_Comment by @zanieb on 2024-11-26 22:12_

Thanks @cthoyt! I fixed that, and I'll expand the note on screenshots.

---

_Comment by @zanieb on 2024-11-26 23:15_

Note to self, we should also cover "Confirming that an issue is a problem with uv" by suggesting

- `python -m build` for build failures
- `python -m pip install --use-pep517 --no-cache --force-reinstall` for install failures

---

_Comment by @FishAlchemist on 2024-11-27 02:19_

Although it might not be directly related to this issue, have you considered mentioning #9452 in the issue template? For example, like this:

- [x] I have read #9452
- [x]  What uv version is being used?
- [x] Can issue be reproduced in the latest uv version?
- [x] What is the operating system?
- [x] What command did you run?
- [x] What is the output of the command with the ``--verbose`` flag?
- [x] Is there a minimal reproduction of the problem?

---

_Comment by @FishAlchemist on 2024-11-27 02:21_

UV has auto-completion, so is it necessary to mention the text editor being used?

---

_Comment by @drmikehenry on 2024-11-27 21:40_

(Minor) There's a double "and" in the phrase `help diagnose your issue and and` in #9452.

---

_Comment by @zanieb on 2024-11-27 23:13_

> (Minor) There's a double "and" in the phrase `help diagnose your issue and and` in [#9452](https://github.com/astral-sh/uv/issues/9452).

Thank you! Fixed.

---

_Comment by @weihenglim on 2024-12-16 06:45_

A helpful tip that we can include is to add the `sort:relevance` qualifier when searching for existing issues. This drastically improves the search results in my experience.

---

_Comment by @FishAlchemist on 2024-12-25 09:46_

Are we possible to prevent using ``echo`` to print ``environment variables``?
Because ``echo`` can print both ``environment variables`` and ``shell variables`` in the same way, as shown in the figure below.
![Image](https://github.com/user-attachments/assets/14dc5752-aa7a-48db-bef5-be4fca94b4f9)
I suggest using ``printenv`` instead of ``echo`` to display environment variables.
https://www.man7.org/linux/man-pages/man1/printenv.1.html

---

_Comment by @rsyring on 2025-05-23 00:51_

Turn on GH Discussions to differentiate discussions from true issues.  refs: https://github.com/astral-sh/uv/issues/11685

---
