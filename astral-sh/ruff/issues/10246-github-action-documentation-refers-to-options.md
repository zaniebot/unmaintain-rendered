---
number: 10246
title: "Github action documentation refers to 'options' instead of 'args'"
type: issue
state: closed
author: elliot-100
labels:
  - documentation
assignees: []
created_at: 2024-03-06T11:20:51Z
updated_at: 2024-03-06T18:10:47Z
url: https://github.com/astral-sh/ruff/issues/10246
synced_at: 2026-01-10T01:22:49Z
---

# Github action documentation refers to 'options' instead of 'args'

---

_Issue opened by @elliot-100 on 2024-03-06 11:20_

https://docs.astral.sh/ruff/integrations/#github-actions says:

> ruff-action accepts optional configuration parameters via with:, including:
> - version: The Ruff version to install (default: latest).
> - options: The command-line arguments to pass to Ruff (default: "check").
> - src: The source paths to pass to Ruff (default: ".").

I think 'options' here should be 'args' to match the example code:

```
- uses: chartboost/ruff-action@v1
  with:
    src: "./src"
    version: 0.0.259
    args: --select B

```

as using `options` gives:

`Warning: Unexpected input(s) 'options', valid inputs are ['args', 'src', 'version']`

---

_Label `documentation` added by @MichaReiser on 2024-03-06 12:45_

---

_Comment by @MichaReiser on 2024-03-06 12:46_

Thanks; you're right. This should be `args`, according to the [GitHub action's documentation](https://github.com/ChartBoost/ruff-action). Would you be interested PRing a fix for the documentation?

---

_Referenced in [astral-sh/ruff#10249](../../astral-sh/ruff/pulls/10249.md) on 2024-03-06 16:20_

---

_Closed by @MichaReiser on 2024-03-06 17:09_

---

_Comment by @elliot-100 on 2024-03-06 18:10_

Thank you!

---
