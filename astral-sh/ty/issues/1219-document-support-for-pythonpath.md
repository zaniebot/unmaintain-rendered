```yaml
number: 1219
title: "Document support for `PYTHONPATH`"
type: issue
state: open
author: MichaReiser
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-09-20T11:53:16Z
updated_at: 2025-11-14T08:53:51Z
url: https://github.com/astral-sh/ty/issues/1219
synced_at: 2026-01-10T02:06:25Z
```

# Document support for `PYTHONPATH`

---

_Issue opened by @MichaReiser on 2025-09-20 11:53_

Update the https://docs.astral.sh/ty/modules/#python-environment to account for https://github.com/astral-sh/ruff/pull/20441 

I also just realized that we need to change https://github.com/astral-sh/ruff/pull/20441 to add and use the `PYTHONPATH` to/from [`EnvVar`](https://github.com/astral-sh/ruff/blob/1d2128f918a2315a5fc81042b5417f9d8cf34282/crates/ty_static/src/env_vars.rs#L7) so that `PYTHONPATH` shows up in our environment variable documentation.

CC: @mmlb in case you're interested in any of the work

---

_Label `documentation` added by @MichaReiser on 2025-09-20 11:53_

---

_Label `help wanted` added by @MichaReiser on 2025-09-20 11:53_

---

_Comment by @mmlb on 2025-09-20 18:09_

Yep I can do both.

---

_Comment by @mmlb on 2025-09-20 18:39_

I've also noticed that my PR was non-windows friendly by hard-coding `:` as the path separator. Will fix that too.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:53_

---
