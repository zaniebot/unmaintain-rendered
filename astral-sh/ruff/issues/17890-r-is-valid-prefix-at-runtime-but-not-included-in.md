```yaml
number: 17890
title: R is valid prefix at runtime but not included in JSON schema
type: issue
state: open
author: xixixao
labels:
  - bug
  - configuration
  - help wanted
assignees: []
created_at: 2025-05-06T14:43:31Z
updated_at: 2025-05-06T19:15:34Z
url: https://github.com/astral-sh/ruff/issues/17890
synced_at: 2026-01-10T11:09:58Z
```

# R is valid prefix at runtime but not included in JSON schema

---

_Issue opened by @xixixao on 2025-05-06 14:43_

### Summary

**Problem**: `ruff check --select R` works, so `R` seems to be a valid rule selector.
But neither https://github.com/astral-sh/ruff/blob/main/ruff.schema.json nor https://github.com/SchemaStore/schemastore/edit/master/src/schemas/json/ruff.json include "R" in the list of valid select rules. This leads to `pyproject.toml` that includes "R" in `lint.select` items to error in IDEs.

**Fix**: I didn't open a PR because I don't know whether R is or isn't supposed to be valid.

**Workaround**: Use the more explicit `"RET"`, which is the only rule set that R enables.

### Version

ruff 0.11.8 (75effb8ed 2025-05-01)

---

_Label `configuration` added by @AlexWaygood on 2025-05-06 15:15_

---

_Comment by @MichaReiser on 2025-05-06 15:24_

I'm not sure that allowing `R` to enable all `RET`, `RSE` and `RUF` is intentional. The main idea is that you can enable multiple rules from the same category, but not necessarily across categories.

---

_Comment by @xixixao on 2025-05-06 15:43_

In fact I just debugged it and "R" only enables "RET". Edited my first comment. Seems more likely that "R" at runtime is a bug (or perhaps a legacy behavior?).

---

_Comment by @MichaReiser on 2025-05-06 17:53_

Yeah, it may be a relict from before we had `RUF` rules. That seems even worse :)

---

_Label `bug` added by @MichaReiser on 2025-05-06 17:54_

---

_Label `help wanted` added by @MichaReiser on 2025-05-06 17:54_

---

_Comment by @MichaReiser on 2025-05-06 17:54_

@ntBre I think this shouldn't be too hard to fix. In case you're looking for a possible bug fix.

---

_Comment by @ntBre on 2025-05-06 17:56_

Thanks! I'll keep my eye on it if no contributors get to it first.

---

_Comment by @VanOvermeire on 2025-05-06 19:15_

I was just exploring your tool and codebase when I saw this issue. Maybe I've misunderstood, but this seems like a one-line fix?

https://github.com/astral-sh/ruff/pull/17896

If so, apologies for stealing your bug @ntBre :D 

---
