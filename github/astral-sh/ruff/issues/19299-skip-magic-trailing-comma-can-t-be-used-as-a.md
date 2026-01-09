---
number: 19299
title: "`skip-magic-trailing-comma` can't be used as a command line flag"
type: issue
state: closed
author: samuelcolvin
labels: []
assignees: []
created_at: 2025-07-12T13:38:19Z
updated_at: 2025-07-12T13:39:53Z
url: https://github.com/astral-sh/ruff/issues/19299
synced_at: 2026-01-07T13:12:16-06:00
---

# `skip-magic-trailing-comma` can't be used as a command line flag

---

_Issue opened by @samuelcolvin on 2025-07-12 13:38_

### Summary

In the terminal I see:

```
➤ uv run ruff format --config 'skip-magic-trailing-comma = true' packages/python/genai_prices/data.py
error: invalid value 'skip-magic-trailing-comma = true' for '--config <CONFIG_OPTION>'

  tip: A `--config` flag must either be a path to a `.toml` configuration file
       or a TOML `<KEY> = <VALUE>` pair overriding a specific configuration
       option

Could not parse the supplied argument as a `ruff.toml` configuration option:

unknown field `skip-magic-trailing-comma`

For more information, try '--help'.
```

Yet `skip-magic-trailing-comma` works in `pyproject.toml` (albeit with difficulty due to #9006)

### Version

ruff 0.12.3

---

_Comment by @samuelcolvin on 2025-07-12 13:39_

oh, I need `format.skip-magic-trailing-comma = true`, that was non-obvious.

---

_Closed by @samuelcolvin on 2025-07-12 13:39_

---
