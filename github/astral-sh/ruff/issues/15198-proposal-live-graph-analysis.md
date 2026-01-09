---
number: 15198
title: "[PROPOSAL] live (graph) analysis"
type: issue
state: open
author: purajit
labels:
  - wish
  - needs-decision
assignees: []
created_at: 2024-12-30T07:22:12Z
updated_at: 2024-12-30T08:24:42Z
url: https://github.com/astral-sh/ruff/issues/15198
synced_at: 2026-01-07T13:12:16-06:00
---

# [PROPOSAL] live (graph) analysis

---

_Issue opened by @purajit on 2024-12-30 07:22_

The basic idea is to start `ruff` in a mode where it monitors file changes and runs something whenever 
it detects changes. Primarily, a user should be able to provide a command to be run, for instance, `pytest`.

This would allow users to run `ruff analyze live --cmd -- pytest`, which would then run `pytest` on
_all_ impacted tests whenever a file is changed, while considering transitive dependencies; it could also
be used to understand how broad the scope of your changes might be (by just showing transitively 
impacted files on each change), to monitor cyclic dependencies to see in real-time  to check whether your 
changes impact them, probably a ton more ideas others might have.

I have an MVP/WIP implementation in https://github.com/astral-sh/ruff/pull/15178.

Example uses:
* `ruff analyze live -- pytest` to run all impacted tests
* `ruff analyze live --paths tests/test_a.py,tests/test_b.py -- pytest` to only run specific tests if 
they are impacted
* `ruff analyze live`: (unimplemented in the PR) could print certain code health/analysis on each change, 
like a list of impacted files, whether cyclic dependencies will be introduced, etc. For instance, if a repo
is configured with data/config files in its `include-dependencies`, this could show how much might change if
a seemingly simple JSON is edited. Depending on how useful this could be made, it could end up being 
something that's constantly running on the side and offering information and possibly aid.

---

_Referenced in [astral-sh/ruff#15178](../../astral-sh/ruff/pulls/15178.md) on 2024-12-30 07:22_

---

_Label `wish` added by @MichaReiser on 2024-12-30 08:24_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-30 08:24_

---
