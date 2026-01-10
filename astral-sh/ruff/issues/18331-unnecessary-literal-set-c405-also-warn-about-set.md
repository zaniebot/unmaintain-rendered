---
number: 18331
title: "unnecessary-literal-set (C405): also warn about `set({})`"
type: issue
state: open
author: bluetech
labels:
  - rule
assignees: []
created_at: 2025-05-27T11:18:55Z
updated_at: 2025-06-07T08:51:35Z
url: https://github.com/astral-sh/ruff/issues/18331
synced_at: 2026-01-10T01:22:59Z
---

# unnecessary-literal-set (C405): also warn about `set({})`

---

_Issue opened by @bluetech on 2025-05-27 11:18_

### Summary

I have seen this a few times. It would be nice if [C405](https://docs.astral.sh/ruff/rules/unnecessary-literal-set/) would warn about this case as well. `{...}` is a literal set so seems reasonable to warn about `{}` in addition to `[]`.

---

_Comment by @MichaReiser on 2025-05-27 13:04_

This would have to be a new rule to keep it consistent with how we handle dicts: [`unnecessary-literal-within-dict-call`](https://docs.astral.sh/ruff/rules/unnecessary-literal-within-dict-call/)

Although it's not entirely clear into which upstream lint rule this would fit

---

_Label `rule` added by @MichaReiser on 2025-05-27 13:04_

---

_Label `needs-design` added by @MichaReiser on 2025-05-27 13:06_

---

_Comment by @bluetech on 2025-05-27 13:19_

I realize I should have raised this in upstream flake8-comprehensions first for consistency and so everyone can benefit. Did it now: https://github.com/adamchainz/flake8-comprehensions/issues/619

---

_Referenced in [adamchainz/flake8-comprehensions#619](../../adamchainz/flake8-comprehensions/issues/619.md) on 2025-05-30 09:52_

---

_Comment by @adamchainz on 2025-05-30 14:12_

I would prefer to not extend flake8-comprehensions, now that I only use Ruff. But if I were to add this feature there, I would make it a new rule. Perhaps you can use the next C4xx code for consistency?

---

_Comment by @MichaReiser on 2025-05-30 20:09_

That sounds good. The main reason we avoid adding rules that don't exist upstream is to avoid future overlaps but what I hear is that this is unlikely to happen here

---

_Label `needs-design` removed by @MichaReiser on 2025-05-30 20:10_

---

_Referenced in [astral-sh/ruff#18492](../../astral-sh/ruff/issues/18492.md) on 2025-06-07 00:19_

---

_Comment by @grihabor on 2025-06-07 08:51_

Are you taking C421? I'm going to take C422 for filter https://github.com/astral-sh/ruff/issues/18492

---

_Referenced in [astral-sh/ruff#19267](../../astral-sh/ruff/issues/19267.md) on 2025-07-11 16:42_

---
