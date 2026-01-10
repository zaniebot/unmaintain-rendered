---
number: 15384
title: Intention of uv tool vs uv run
type: issue
state: open
author: wallacms
labels:
  - question
assignees: []
created_at: 2025-08-19T21:44:25Z
updated_at: 2025-08-20T13:47:45Z
url: https://github.com/astral-sh/uv/issues/15384
synced_at: 2026-01-10T01:25:55Z
---

# Intention of uv tool vs uv run

---

_Issue opened by @wallacms on 2025-08-19 21:44_

### Question

I'm trying to better understand the intended use case for `uv tool` vs `uv run` when it comes to external tools.  For simplicity, I'll use `ruff` as an example.  Let's say I want to configure `ruff` in my project and I put certain settings in my pyproject.toml.  Obviously, these settings are going to be specific to some lower-bound (and possibly upper-bound) version of `ruff`.  If some user downloads my project and tries to run ruff with some ancient version, it might not work or might give spurious errors.

I'd like to be able to specify a version bound in my pyproject.toml to say "this project needs X version of ruff".  One easy way for me to do this is to declare a dependency group like "dev" and declare the dependency there.  However, this feels a bit at odds with the philosophy of `uv tool` because it now opens up my project to dependency conflicts from `ruff` even though in practice the two are completely separate.

As far as I know, there is no way to declare version constraints in my project when using `uv tool run` (aka `uvx`).  I know that for the specific case of `ruff` I can declare a required version, but this will only result in an error.  What I'd like is for `uv tool` to check the required version in my pyproject.toml, then check the tools I've installed to see if I have a compatible version and then install compatible version if not.  This kind of requires the functionality described in #6365 but it's a bit separate as well.

To summarize my question, is the best practice `uvx ruff` or `uv run ruff`?  If the answer is the former, how are versions supposed to be managed?  If the answer is the latter, what is the intended use case of `uv tool`?

### Platform

all

### Version

uv 0.6.11

---

_Label `question` added by @wallacms on 2025-08-19 21:44_

---

_Comment by @zanieb on 2025-08-19 21:58_

The documentation at https://docs.astral.sh/uv/concepts/tools/#relationship-to-uv-run covers this a little. I think we could expand it, but we're still trying to figure out what the answer is :)

I think the crux of your question is discussed in https://github.com/astral-sh/uv/issues/7186, I'd recommend reading through that and the linked issues.

I think the best practice depends on the tool — is version consistency important for your project, or not? If it's important that everyone is on the same version, then you probably want to use `uv run`.

---

_Comment by @wallacms on 2025-08-20 13:09_

Thanks, this is helpful.  I think ultimately what I'm looking for is `uv tool add` as described in #3560.  Are there any conceptual blockers to implementing this?  It mentions PEP 735, but this is finalized now.

The discussion in #7186 seems to be largely centered around "magic" behavior and trying to guess what the user wants.  If `uv tool add` was implemented, I think this sidesteps a lot of this.  If my project explicitly declares it needs tool X at version Y, I think it's quite reasonable for `uv tool run` to always use that version.  If it doesn't, then I think the current behavior is fine.

---

_Comment by @zanieb on 2025-08-20 13:47_

I think main the conceptual blocker to #3560 is that it was the largest point of confusion in that thread. The design space also overlaps with #5903. It's not entirely clear it's the "right" solution.

---
