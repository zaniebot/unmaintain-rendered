```yaml
number: 2361
title: Ability to set output format with an environment variable
type: issue
state: open
author: robertodr
labels:
  - cli
  - wish
assignees: []
created_at: 2026-01-06T10:05:59Z
updated_at: 2026-01-06T15:40:27Z
url: https://github.com/astral-sh/ty/issues/2361
synced_at: 2026-01-10T01:56:41Z
```

# Ability to set output format with an environment variable

---

_Issue opened by @robertodr on 2026-01-06 10:05_

It would be useful to set the output format with an environment variable, _i.e._ `TY_OUTPUT_FORMAT`. Ruff has a similar environment variable.

---

_Label `cli` added by @AlexWaygood on 2026-01-06 10:07_

---

_Label `configuration` added by @AlexWaygood on 2026-01-06 10:07_

---

_Comment by @MichaReiser on 2026-01-06 10:17_

Can you say a bit more about your use case?

---

_Comment by @robertodr on 2026-01-06 12:42_

I'm creating a custom pre-commit hook and would like to be able to switch the output format based on whether it's run in a github workflow or locally.

---

_Comment by @MichaReiser on 2026-01-06 13:00_

I see, thanks. Seems reasonable to me. Would you be interested in contributing the change? 

All that should be required is to set `env` similar to how it's done for the configuration: 

https://github.com/astral-sh/ruff/blob/e869046dcf75bd1581f6230e13b7bf3dec9b7353/crates/ty/src/args.rs#L125-L126

and then add a new test here

https://github.com/astral-sh/ruff/blob/e869046dcf75bd1581f6230e13b7bf3dec9b7353/crates/ty/tests/cli/main.rs#L96-L115

---

_Label `wish` added by @MichaReiser on 2026-01-06 13:00_

---

_Label `configuration` removed by @MichaReiser on 2026-01-06 13:00_

---

_Comment by @robertodr on 2026-01-06 15:40_

I can try!

---
