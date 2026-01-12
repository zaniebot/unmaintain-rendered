```yaml
number: 6606
title: Add sphinx documentation style to pydocstyle config
type: issue
state: open
author: Eutropios
labels:
  - docstring
assignees: []
created_at: 2023-08-16T02:23:20Z
updated_at: 2024-10-27T11:05:25Z
url: https://github.com/astral-sh/ruff/issues/6606
synced_at: 2026-01-12T15:54:46Z
```

# Add sphinx documentation style to pydocstyle config

---

_@Eutropios_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Addition of sphinx documentation style to pydocstyle config as so:
```toml
[tool.ruff.pydocstyle]
    convention = "sphinx"
```
Currently pep257, google, and numpy documentation styles are supported. It'd be nice to have this major documentation style added as well.

---

_Label `docstring` added by @charliermarsh on 2023-08-16 03:42_

---

_Comment by @charliermarsh on 2023-08-16 03:43_

We can probably add a convention that's compatible, but there would also be necessary work to actually support parsing Sphinx docstrings (e.g., https://github.com/PyCQA/pydocstyle/pull/595). pydocstyle itself does not support this.

---

_Label `needs-decision` added by @charliermarsh on 2023-08-16 03:43_

---

_Comment by @Eutropios on 2023-09-22 19:41_

Alright, so as far as I know, the necessary pull requests to pydocstyle have been made as of 5 hours ago. Relevant commit here: [PyCQA/pydocstyle/commit](https://github.com/PyCQA/pydocstyle/commit/6d5455e4050b9f8b0905f9d5b0bc7bb73c6ef442).

---

_Comment by @LecrisUT on 2023-11-22 18:43_

Related, it would be nice to get a check when a different convention is used.

---

_Label `needs-decision` removed by @charliermarsh on 2023-11-22 18:46_

---

_Comment by @charliermarsh on 2023-11-22 18:46_

(Removing `needs-decision` since we should support this now.)

---

_Comment by @Eutropios on 2023-11-24 00:18_

Sounds great! Let me know if there's anything I can do to help. 

---

_Comment by @winwinashwin on 2024-02-05 06:25_

+1 on this feature. Any updates on sphinx support?

---

_Comment by @DanPaseltiner on 2024-02-16 21:17_

Ditto @winwinashwin. We would adopt Ruff immediately on [this project](https://github.com/CDCgov/phdi) it there was sphinx support. Unfortunately, we have a code base full of Sphinx style docstrings and need to implement linting on them.     

---

_Comment by @jaladh-singhal on 2024-03-27 20:21_

Hi, just checking if/when this will be available?

BTW thanks for making this super awesome tool!

---

_Comment by @Lassii- on 2024-08-12 08:59_

Hello,

Also wondering if there's any plan updates to this? The project I work for is also looking for the support.

---

_Comment by @seppzer0 on 2024-10-17 21:15_

@charliermarsh , @zanieb , hi! Sorry for pinging directly on this, I see this as an only option to revive this ticket.

I noticed that this was going to be supported, but there haven't been news for a while. Could you please inform whether this is going to be supported in ruff?

Looking forward to bundle this with uv usage to have a fully configured ecosystem for our projects.


---

_Comment by @seppzer0 on 2024-10-17 21:24_

I think I found related PRs, as far as I understand they are still under review
- https://github.com/astral-sh/ruff/pull/13286
- https://github.com/astral-sh/ruff/pull/13280

---

_Comment by @seppzer0 on 2024-10-26 13:11_

@charliermarsh , @zanieb, any info please

---

_Comment by @MichaReiser on 2024-10-26 16:08_

@seppzer0 I understand that this feature is important to you but please stop pinging people. We get a lot of notifications every day and pinging only results in additional distractions, meaning we have less time to do actual feature work.

We plan to look at linked PRs but it might take a while because we have to balance it with other features that we want to ship. We'll make sure to update this issue or the linked PRs if there's an update.

---

_Comment by @seppzer0 on 2024-10-27 11:05_

@MichaReiser , thank you for your response! I do understand that pinging people is not great, unfortunately though it is often the only way to remind of the topic at hand.

Thank you once again for your work! Looking towards to this great feature 👍🏻

---
