```yaml
number: 15760
title: "`uv format` cannot be disabled/reconfigured"
type: issue
state: open
author: onerandomusername
labels: []
assignees: []
created_at: 2025-09-09T16:34:47Z
updated_at: 2025-09-09T16:34:47Z
url: https://github.com/astral-sh/uv/issues/15760
synced_at: 2026-01-10T03:23:54Z
```

# `uv format` cannot be disabled/reconfigured

---

_Issue opened by @onerandomusername on 2025-09-09 16:34_

There's two parts to this issue, though the overarching request is same, hence while I'm posting them together.

## Problem 1: `uv format` cannot be disabled

`uv format` is a command that will run ruff over the project, and grants an entrypoint to a command that isn't how the developer of the project may have opted to have formatting run. (anti-ethos to [this comment on python discussions](https://discuss.python.org/t/idea-introduce-standardize-project-development-scripts/67194/35))

## Problem 2: `uv format` has vendor lock-in

When a project has a different formatter already in use in their project, it would be beneficial to be able to configure `uv format` to use that designated formatter. 

Hatch already has an [implementation of this](https://hatch.pypa.io/latest/how-to/static-analysis/behavior/#customize-static-analysis-behavior) which allows selecting dependencies for its `hatch fmt` command and creates a specific environment for running lint and format. uv already has the necessary tooling to create a temporary environment with these specified dependencies, and could most likely implement something like this for `uv format`.

---
