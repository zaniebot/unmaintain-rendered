```yaml
number: 10142
title: Should Ruff linter parse and react to pylint disable waiver comments?
type: issue
state: closed
author: kaddkaka
labels: []
assignees: []
created_at: 2024-02-26T21:19:28Z
updated_at: 2024-02-27T08:13:44Z
url: https://github.com/astral-sh/ruff/issues/10142
synced_at: 2026-01-12T15:54:49Z
```

# Should Ruff linter parse and react to pylint disable waiver comments?

---

_@kaddkaka_

Probably many people that start to use ruff linter have been using pylint previously. These users might already have some pylint rules disabled in their codebase:

```py
class ComplexBanana:  # pylint:disable=too-many-public-methods
```

Could and should ruff read those disable comments and treat them as `noqa` comments? I think it would ease the crossover.

---

_Comment by @kaddkaka on 2024-02-27 06:56_

This ties in to https://github.com/astral-sh/ruff/issues/1773 ("Human-friendly rule names")

---

_Comment by @MichaReiser on 2024-02-27 08:13_

Thanks. This is a duplicate of https://github.com/astral-sh/ruff/issues/1203 (took me a while to find) 


---

_Closed by @MichaReiser on 2024-02-27 08:13_

---
