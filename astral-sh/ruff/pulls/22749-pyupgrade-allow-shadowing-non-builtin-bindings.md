```yaml
number: 22749
title: "[`pyupgrade`] Allow shadowing non-builtin bindings (`UP029`)"
type: pull_request
state: open
author: ntBre
labels:
  - bug
  - rule
assignees: []
base: main
head: brent/up029
created_at: 2026-01-20T00:34:48Z
updated_at: 2026-01-20T00:46:07Z
url: https://github.com/astral-sh/ruff/pull/22749
synced_at: 2026-01-20T01:38:34Z
```

# [`pyupgrade`] Allow shadowing non-builtin bindings (`UP029`)

---

_@ntBre_

Summary
--

I thought the fix unsafety example in the [rule docs](https://docs.astral.sh/ruff/rules/unnecessary-builtin-import/#fix-safety) looked a bit suspicious
while I was going through more potential default rules today ([playground](https://play.ruff.rs/f1a8b73d-6277-4414-b918-44bbba2863c2)):

```py
def str(x):
    return x

from builtins import str

str(1)  # `"1"` with the import, `1` without
```

Changing the behavior in this way seemed to go beyond fix unsafety and into bug
territory. Sure enough, there was an existing bug report in #16182.

This PR fixes #16182 (and the fix safety example) by checking that the builtin
import that the rule flags is actually shadowing a builtin binding. I also left
an exception for `from builtins import *`, but we could consider ignoring that
case too.

I initially tried reusing `SemanticModel::resolve_name` and `only_binding`, but
they are specific to `ExprName`s because that seems to be all that is inserted
into the `SemanticModel::resolved_names` map.

Test Plan
--

New tests based on #16182 and the fix safety docs


---

_Label `bug` added by @ntBre on 2026-01-20 00:34_

---

_Label `rule` added by @ntBre on 2026-01-20 00:34_

---

_Review requested from @MichaReiser by @ntBre on 2026-01-20 00:45_

---

_Marked ready for review by @ntBre on 2026-01-20 00:45_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 00:46_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---
