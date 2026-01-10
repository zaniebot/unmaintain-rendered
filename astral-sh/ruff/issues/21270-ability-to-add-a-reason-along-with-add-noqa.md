---
number: 21270
title: Ability to add a reason along with --add-noqa
type: issue
state: closed
author: rtpg
labels: []
assignees: []
created_at: 2025-11-04T04:16:21Z
updated_at: 2025-11-04T13:20:51Z
url: https://github.com/astral-sh/ruff/issues/21270
synced_at: 2026-01-10T01:23:02Z
---

# Ability to add a reason along with --add-noqa

---

_Issue opened by @rtpg on 2025-11-04 04:16_

### Summary

Biome's [supressions](https://biomejs.dev/analyzer/suppressions/) has an explanation field.

When you suppress in `biome lint` (their equivalent to `--add-noqa`), you're asked to provide a reason that gets added.

    biome lint --suppress --reason="initial ignoring of this error"

This is nice because it immediately expresses that it's a target for cleanup, for people who come across it.

It would be nice for `ruff` to have a similar `reason` argument, that could generate a comment...

`ruff check --add-noqa --reason="initial ignore"`

-> `logger.warn(...)  # noqa: G004 # initial ignore`

I'm not fussy about where the comment ends up, though I'm sure that the question of where the comment ends up is tricky. 

---

_Closed by @MichaReiser on 2025-11-04 13:20_

---

_Reopened by @MichaReiser on 2025-11-04 13:20_

---

_Closed by @MichaReiser on 2025-11-04 13:20_

---
