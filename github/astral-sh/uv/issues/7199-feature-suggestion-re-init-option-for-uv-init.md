---
number: 7199
title: "Feature suggestion: Re-init option for `uv init`"
type: issue
state: open
author: smheidrich
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-09-08T20:53:25Z
updated_at: 2024-12-31T18:37:32Z
url: https://github.com/astral-sh/uv/issues/7199
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature suggestion: Re-init option for `uv init`

---

_Issue opened by @smheidrich on 2024-09-08 20:53_

## Proposal

It would be nice if uv provided a way to "re-initialize" a project, e.g. as a different kind of project (application vs library, package vs no-package) than the one originally selected.

## Motivation

uv's default of initializing projects as applications without a build backend defined in `pyproject.toml` (or source distributions subsequently built using `uv build`) may be confusing for people coming from other Python packaging tools (see also https://github.com/astral-sh/uv/issues/7181). Once they've figured out what's going on, many will want to change their project to a different kind, e.g. to a library.

## Workaround

People can re-initialize their project from scratch at any time by just deleting `pyproject.toml` and re-running `uv init` (https://github.com/astral-sh/uv/pull/7198 will make this more obvious). So I guess this feature is very low-priority, but might be nice to have.

---

_Label `enhancement` added by @charliermarsh on 2024-09-16 02:54_

---

_Label `needs-design` added by @charliermarsh on 2024-09-16 02:54_

---

_Comment by @edmorley on 2024-12-31 16:38_

Another use-case for allowing re-init (aside from eg switching a uv project from "app" to "lib" etc): A project that has a `pyproject.toml` already (eg to hold various `[tool.*]` config) that then wants to migrate to uv from say pip/requirements files. Such a project will have a `pyproject.toml` that is likely missing the `[project]` table.

Although supporting this will require the ability to merge in the template `pyproject.toml` to the existing content, rather than replacing it outright.

---

_Comment by @edmorley on 2024-12-31 16:41_

(The context for this for us at Heroku is to make it easier for us to write future guides/blog posts along the lines of "Heroku now supports uv; here's how you can try it out with your new/existing/... project" whilst having to cover as few of the permutations as possible via explicit steps. ie: If we can tell end users to run `uv init` and it interactively prompts appropriately for an existing vs new project etc, then it takes a lot of the boilerplate away from our guide.)

---

_Comment by @smheidrich on 2024-12-31 18:37_

@edmorley I guess that would be a mixture between this feature and

- https://github.com/astral-sh/uv/issues/6277,

which, I think, would do most/all of what you want, except it wouldn't modify `pyproject.toml` in-place and would require a separate CLI flag instead of being an interactively selectable option of `uv init`.

In my mind, the re-initialization feature requested in this issue here was going to be triggered by a CLI flag as well, something like `uv init --reinit`. But I agree that prompting the user interactively in a simple `uv init` invocation might be cool, too.

---
