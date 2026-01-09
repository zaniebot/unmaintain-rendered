---
number: 20747
title: Fix several rules to handle deferred annotations
type: issue
state: open
author: ntBre
labels:
  - bug
assignees: []
created_at: 2025-10-07T15:17:45Z
updated_at: 2025-10-23T20:37:40Z
url: https://github.com/astral-sh/ruff/issues/20747
synced_at: 2026-01-07T13:12:16-06:00
---

# Fix several rules to handle deferred annotations

---

_Issue opened by @ntBre on 2025-10-07 15:17_

Several rules exhibit very different behavior in the presence of deferred annotations, whether from an explicit `from __future__ import annotations` or on Python 3.14. These include:

- FAST001
- FAST003
- A003
- PTH210
- RUF001

You can see the effect of switching the Python version from 3.13 to 3.14 in this [commit](https://github.com/astral-sh/ruff/pull/20725/commits/095acaff3356c9941262b72d4e911514d6dd28bb) from #20725.

In addition to the tests modified in the commit above, each of these rules also has a new `assert_diagnostics_diff` test:

- [FAST001 and FAST003](https://github.com/astral-sh/ruff/blob/a658f4181d4e3ee85815e9f84c88d77c51160186/crates/ruff_linter/src/rules/fastapi/mod.rs#L34)
- [A003](https://github.com/astral-sh/ruff/blob/a658f4181d4e3ee85815e9f84c88d77c51160186/crates/ruff_linter/src/rules/flake8_builtins/mod.rs#L68)
- [PTH210](https://github.com/astral-sh/ruff/blob/a658f4181d4e3ee85815e9f84c88d77c51160186/crates/ruff_linter/src/rules/flake8_use_pathlib/mod.rs#L87)
- [RUF001](https://github.com/astral-sh/ruff/blob/a658f4181d4e3ee85815e9f84c88d77c51160186/crates/ruff_linter/src/rules/ruff/mod.rs#L249)

To fix this issue, we should remove all of these diffs.

Micha and Dylan observed that `semantic.resolve_name` seems to be returning the wrong results now (https://github.com/astral-sh/ruff/pull/20725#issuecomment-3376143411), and I think they suggested possibly making these checks deferred so that we can resolve these lookups differently.

Unless the changes end up being quite small, I think it could make sense to tackle these with one PR per rule.

---

_Renamed from "Fix several rules to handle typing-only annotations" to "Fix several rules to handle deferred annotations" by @ntBre on 2025-10-07 15:18_

---

_Label `bug` added by @ntBre on 2025-10-07 15:30_

---

_Assigned to @dylwil3 by @ntBre on 2025-10-07 15:30_

---

_Referenced in [astral-sh/ruff#20750](../../astral-sh/ruff/pulls/20750.md) on 2025-10-07 16:40_

---

_Comment by @11happy on 2025-10-12 12:51_

Hello @ntBre , I am interested to work on this , Can I work on `A003` ? 
Thank you

---

_Comment by @MichaReiser on 2025-10-13 06:30_

We have to check with @dylwil3, as he's currenlty working on this

---

_Comment by @dylwil3 on 2025-10-13 17:55_

The more the merrier - feel free to work on `A003` @11happy !

---

_Referenced in [astral-sh/ruff#21004](../../astral-sh/ruff/issues/21004.md) on 2025-10-21 04:36_

---

_Comment by @11happy on 2025-10-21 18:16_

While Debugging for `A003` I noticed that for this case
```
def str(self):
        pass

    def method_usage(self) -> str:
        pass
```
in python 3.14 zero references were there you see can the debug prints below
```
binding: str
Binding ID: BindingId(173)
Binding kind: FunctionDefinition(ScopeId(11))
Binding start: 206
Binding type: Method
'str' shadows a builtin
Number of references: 0
```

& similary for 3.13
```
binding: str
Binding ID: BindingId(172)
Binding kind: FunctionDefinition(ScopeId(11))
Binding start: 206
Binding type: Method
'str' shadows a builtin
Number of references: 1
```
now to solve this issue I was thinking can we walk the AST to to find shadowed names in annotations, instead of relying on semantic references ? Please let me know if this is the expected approach ? Would appreciate any pointers here.

Thank you : )

---

_Comment by @ntBre on 2025-10-23 20:37_

I haven't really looked closely enough to give a definitive answer, and Dylan might have a better idea, but the nice thing about fixing the semantic model in principle is that it will "automatically" fix all of the affected rules. And walking the AST repeatedly can also be expensive, but sometimes there's no other choice.

---
