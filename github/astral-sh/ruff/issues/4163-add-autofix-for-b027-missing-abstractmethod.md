---
number: 4163
title: "Add autofix for B027 (missing @abstractmethod decorator)."
type: issue
state: closed
author: Skylion007
labels:
  - good first issue
  - fixes
assignees: []
created_at: 2023-04-30T17:20:36Z
updated_at: 2023-05-09T12:53:54Z
url: https://github.com/astral-sh/ruff/issues/4163
synced_at: 2026-01-07T13:12:14-06:00
---

# Add autofix for B027 (missing @abstractmethod decorator).

---

_Issue opened by @Skylion007 on 2023-04-30 17:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
B027 currently does not have an autofix, but the autofix seems fairly straightforward (just add an `@abstractmethod` decorator). I would recommend someone take a look at adding the autofix here. It is also probably a good first issue.


---

_Referenced in [pytorch/pytorch#100342](../../pytorch/pytorch/pulls/100342.md) on 2023-04-30 17:25_

---

_Label `good first issue` added by @charliermarsh on 2023-04-30 19:04_

---

_Label `autofix` added by @charliermarsh on 2023-04-30 19:04_

---

_Comment by @aacunningham on 2023-05-01 04:08_

I'm game to give this a shot, never messed with autofix logic before so should be fun

---

_Assigned to @aacunningham by @MichaReiser on 2023-05-01 10:38_

---

_Comment by @MichaReiser on 2023-05-01 10:38_

> I'm game to give this a shot, never messed with autofix logic before so should be fun

Thank you, and enjoy! 

---

_Referenced in [astral-sh/ruff#4178](../../astral-sh/ruff/pulls/4178.md) on 2023-05-02 06:25_

---

_Closed by @charliermarsh on 2023-05-04 16:36_

---

_Comment by @justinchuby on 2023-05-07 22:23_

I wonder how safe this fix is, since it may lead to broken code when existing derived classes don’t implement the supposed-to-be abstract methods

---

_Comment by @aacunningham on 2023-05-07 22:42_

That's a good point, actually dealt with that once after realizing I completely misused `abc`. Realized I had failed to fully implement the class in a few spots.

It's probably not the safest fix. Trading out one lint error for another (plus a runtime error). I know there's that effort to add safety levels to fixes, and this would probably fall into that unsafe category (or maybe some middle one). Are there any guidelines on how aggressive autofixes can be without something like that in place?

---

_Comment by @charliermarsh on 2023-05-07 22:51_

There aren't hard-and-fast rules, but autofix safety is being worked on and the severity levels are being defined, so this may be a useful example to lean on. We could also revert this fix until safety levels are introduced.

---

_Comment by @aacunningham on 2023-05-08 22:18_

I can revert this if it hasn't been done yet. Something like:
1. New MR to revert the change
2. Reopen this ticket and comment that we're waiting for autofix safety to get closer to done
3. Once it's ready, update the branch and start a new MR?

It looks like the change is already in a published version, but I'm assuming it's fine to still revert it?

---

_Comment by @charliermarsh on 2023-05-09 12:53_

@aacunningham - Yeah that sounds reasonable to me -- it's ok to revert, it's not a breaking change to remove autofix capabilities.

---

_Referenced in [astral-sh/ruff#4310](../../astral-sh/ruff/pulls/4310.md) on 2023-05-09 16:10_

---
