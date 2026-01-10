```yaml
number: 13542
title: "[DOC] Explain `uv sync` in the projects guide for working on existing projects"
type: pull_request
state: open
author: turbotimon
labels: []
assignees: []
base: main
head: main
created_at: 2025-05-19T18:52:52Z
updated_at: 2025-10-21T07:41:58Z
url: https://github.com/astral-sh/uv/pull/13542
synced_at: 2026-01-10T06:36:15Z
```

# [DOC] Explain `uv sync` in the projects guide for working on existing projects

---

_Pull request opened by @turbotimon on 2025-05-19 18:52_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Improving documentation by adding a section about working on existing projects in the projects guide. I didn't find anything explicit, and it is in my opinion not obvious to people new to uv. Full motivation is in #13541

Please feel free to change or improve the wording if necessary.

## Test Plan

Documentation preview.


---

_Review requested from @zanieb by @konstin on 2025-05-21 17:26_

---

_Assigned to @zanieb by @zanieb on 2025-05-21 18:35_

---

_Comment by @zanieb on 2025-05-21 18:35_

Thanks! I'll review this when I'm back from PyCon.

---

_Comment by @turbotimon on 2025-06-12 07:06_

@zanieb don't want to rush you, but any updates here? Thanks in advance

---

_Comment by @zanieb on 2025-06-12 13:43_

Thanks for the poke! Lots to keep track of, appreciate the patience :)

---

_Review comment by @zanieb on `docs/guides/projects.md`:48 on 2025-06-12 13:44_

We don't say "simply": https://github.com/astral-sh/uv/blob/main/STYLE.md#documentation

---

_@zanieb reviewed on 2025-06-12 13:44_

---

_@zanieb reviewed on 2025-06-12 13:45_

---

_Review comment by @zanieb on `docs/guides/projects.md`:46 on 2025-06-12 13:45_

I wonder if this should be before "Creating ..."?

---

_@zanieb reviewed on 2025-06-12 13:45_

---

_Review comment by @zanieb on `docs/guides/projects.md`:49 on 2025-06-12 13:45_

Nit: We won't install "all" the dependencies, we'll install the subset relevant for your platform and Python version

---

_Review comment by @zanieb on `docs/guides/projects.md`:48 on 2025-06-12 13:46_

Separately, do we want to recommend `uv sync`? The idea is you can just `uv run pytest` and start working. Maybe we want to recommend `uv sync` as a way to get things ready for an IDE?

---

_@zanieb reviewed on 2025-06-12 13:46_

---

_Review comment by @turbotimon on `docs/guides/projects.md`:46 on 2025-06-16 19:16_

I think it's fine this way (e.g. poetry has the [same order](https://python-poetry.org/docs/basic-usage/) (but on the other hand poetry may not be a reference since we try to do things better 😉 )). However, I'm completly open to both options, whichever you think is better!

---

_Review comment by @turbotimon on `docs/guides/projects.md`:48 on 2025-06-16 19:20_

Yes, that was my idea to set up the venv for the IDE. A hint about automatic locking&syncing is just below.

---

_Review comment by @turbotimon on `docs/guides/projects.md`:49 on 2025-06-16 19:29_

Valid Nit. What you think about the "..and the [default](https://docs.astral.sh/uv/concepts/projects/dependencies/#default-groups) dependencies" with a link to the concept? I would not go into more details about platforms and Python versions at this point. I like the guides as simple and general as possible.

---

_Review comment by @turbotimon on `docs/guides/projects.md`:48 on 2025-06-16 19:31_

"We don't say simply": Thanks! I completly missed the STYLE.md (a link in the CONTRIBUTING.md would have helped in my case)

---

_@turbotimon reviewed on 2025-06-16 19:47_

---

_@zanieb reviewed on 2025-06-17 01:33_

---

_Review comment by @zanieb on `docs/guides/projects.md`:48 on 2025-06-17 01:33_

That makes sense. I think we might just want to reframe it a bit, I'll take a swing at the phrasing.

---

_@zanieb reviewed on 2025-06-17 01:34_

---

_Review comment by @zanieb on `docs/guides/projects.md`:46 on 2025-06-17 01:34_

I don't have strong feelings. Thanks for the reference though, I'll look at that and see if I like it that way :)

---

_@zanieb reviewed on 2025-06-17 01:36_

---

_Review comment by @zanieb on `docs/guides/projects.md`:49 on 2025-06-17 01:36_

I agree we shouldn't go into detail. I worry about introducing the idea of "default" dependencies. I'll try to rephrase this too. 

---

_Comment by @turbotimon on 2025-07-15 14:15_

@zanieb just saw that there was a linting issue, it's now fixed. Do you think it's ready to merge now?

---

_Comment by @turbotimon on 2025-08-11 09:53_

@zanieb any updates here? Thanks in advance

---

_Comment by @turbotimon on 2025-10-21 07:41_

@zanieb or anybody from the team. Everything is resolved, could you please merge?

---

_Review requested from @zanieb by @turbotimon on 2025-10-21 07:41_

---
