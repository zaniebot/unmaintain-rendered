```yaml
number: 498
title: "CLI docs: auto-annotate defaults"
type: issue
state: open
author: brainwane
labels:
  - documentation
assignees: []
created_at: 2025-05-23T15:39:07Z
updated_at: 2025-05-23T15:41:44Z
url: https://github.com/astral-sh/ty/issues/498
synced_at: 2026-01-12T15:54:23Z
```

# CLI docs: auto-annotate defaults

---

_@brainwane_

`docs/reference/cli.md` lists a few options that are lacking documentation of default values (e.g.  `--color`).

Per @carljm in https://github.com/astral-sh/ruff/issues/18246#issuecomment-2899218319 , 

> This seems like useful information for users to have in the command reference; should we just add it in the comment at https://github.com/astral-sh/ruff/blob/d098118e37be9437345d527c29a5aeea91b676a6/crates/ty/src/args.rs#L289, or is there a way to update the script so that we automatically output this in the reference based on the existing `[#default]` annotation?

Seems to me it would be more elegant to do the latter, if feasible.

(Sub-issue within #149; followup to #478)

---

_Label `documentation` added by @MichaReiser on 2025-05-23 15:40_

---

_Comment by @MichaReiser on 2025-05-23 15:41_

> Seems to me it would be more elegant to do the latter, if feasible.

We should only do this if we also change the output of `ty --help`. The idea is that the CLI docs mirros (or at least, captures the same information) as `ty --help`. 

---
