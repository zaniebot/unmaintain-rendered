```yaml
number: 218
title: "`--config` option"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
  - configuration
assignees: []
created_at: 2025-01-15T14:24:54Z
updated_at: 2025-05-17T17:07:28Z
url: https://github.com/astral-sh/ty/issues/218
synced_at: 2026-01-10T02:34:09Z
```

# `--config` option

---

_Issue opened by @MichaReiser on 2025-01-15 14:24_

A `key=value` pair to set an arbitrary configuration option in addition to the project’s configuration



---

_Comment by @thejchap on 2025-04-25 04:58_

@MichaReiser I'll take a look at this one - let me know if you think there are/should be any notable differences from Ruff's [implementation](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/args.rs#L868-L868) or otherwise any design considerations I should keep in mind

---

_Comment by @MichaReiser on 2025-04-25 06:08_

Ruff's implementation is probably a good start. The only thing that I wonder is if it's possible to parse it into a structural representation similar to what we now do with `RulesArg`. That feels slightly nicer.

https://github.com/astral-sh/ruff/blob/8249a724127c090c47bc74aefe1d5aa935b8c681/crates/red_knot/src/args.rs#L168-L237

---

_Renamed from "[red-knot] `--config` option" to "`--config` option" by @MichaReiser on 2025-05-07 15:27_

---

_Label `cli` added by @AlexWaygood on 2025-05-10 21:40_

---

_Label `configuration` added by @AlexWaygood on 2025-05-10 21:40_

---

_Closed by @MichaReiser on 2025-05-17 17:07_

---
