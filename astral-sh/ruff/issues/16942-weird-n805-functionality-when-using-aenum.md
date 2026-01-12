```yaml
number: 16942
title: "Weird N805 functionality when using `aenum`"
type: issue
state: closed
author: gtkacz
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-03-24T02:47:21Z
updated_at: 2025-03-25T22:12:03Z
url: https://github.com/astral-sh/ruff/issues/16942
synced_at: 2026-01-12T15:54:55Z
```

# Weird N805 functionality when using `aenum`

---

_@gtkacz_

### Summary

N805 doesn't recognize `aenum`'s metaclass when it does the same for regular enum. Examples below:

enum: https://play.ruff.rs/716742ee-e98f-42ef-abce-783e1cb18ac7
aenum: https://play.ruff.rs/49da9f35-57a8-4b5d-91f1-fec4b7579d06

### Version

ruff 0.9.10 (0dfa810e9 2025-03-07)

---

_Comment by @ntBre on 2025-03-24 14:18_

We have some special casing for the stdlib `enum`, so this would be an extension to the rule.

As a workaround, you can [ignore](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names) these property names, but that might apply to other unrelated methods too.

---

_Label `rule` added by @ntBre on 2025-03-24 14:18_

---

_Comment by @gtkacz on 2025-03-24 21:11_

@ntBre thank you for the response!

> We have some special casing for the stdlib enum, so this would be an extension to the rule.

Does that mean you guys plan on adding the same support for `aenum`? I think that'd be pretty great considering `aenum` is a massive package with [~25M monthly downloads](https://pypistats.org/packages/aenum).

---

_Label `needs-decision` added by @ntBre on 2025-03-24 21:21_

---

_Comment by @MichaReiser on 2025-03-25 02:26_

> Does that mean you guys plan on adding the same support for aenum? I think that'd be pretty great considering aenum is a massive package with [~25M monthly downloads](https://pypistats.org/packages/aenum).

Not necessarily. No. We have some rules with special casing for third-party dependencies but we try to keep third-party special-casing to a minimum because it is hard to keep up with every third-party dependencies and it's impossible to draw a clear line which libraries should be supported. 

My hope here would be that aenum support falls out from our work on adding a better type inference to ruff, so that it doesn't require special casing

---

_Comment by @InSyncWithFoo on 2025-03-25 09:59_

@MichaReiser Enums are already a special case within the type system. `aenum` is similarly magical; adding support for it in Red Knot would probably be even harder to justify than doing so in Ruff (at least when there is no plugin system).

---

_Comment by @gtkacz on 2025-03-25 22:12_

@MichaReiser @InSyncWithFoo @ntBre thank you all for the responses! For now I've disabled `N805` on this particular file that declares an `aenum` metaclass.

---

_Closed by @gtkacz on 2025-03-25 22:12_

---
