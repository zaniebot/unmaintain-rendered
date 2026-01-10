```yaml
number: 17455
title: F401 false positive for forward references in TypeAliases
type: issue
state: open
author: ktbarrett
labels:
  - bug
assignees: []
created_at: 2025-04-17T23:25:57Z
updated_at: 2025-04-26T15:13:49Z
url: https://github.com/astral-sh/ruff/issues/17455
synced_at: 2026-01-10T11:09:58Z
```

# F401 false positive for forward references in TypeAliases

---

_Issue opened by @ktbarrett on 2025-04-17 23:25_

### Summary

When using forward references on the right hand side of the `=` in a type alias, ruff considers the types used in that forward reference unused and issues a F401. Interestingly, the `"TypeAlias"` import does not cause an issue because it's used in an annotation.

https://play.ruff.rs/e30129f9-c09c-4c76-9272-9aa757ece49b

It seems that ruff needs to be aware of type aliases and treat the right side as uses. While this isn't possible for implicit aliases (where `TypeAlias` is *not* used in the annotation of the alias), it is possible for explicit aliases.

### Version

0.11.6

---

_Renamed from "F401 false positive for forward references" to "F401 false positive for forward references in TypeAliases" by @ktbarrett on 2025-04-17 23:26_

---

_Comment by @ktbarrett on 2025-04-17 23:28_

Perhaps the implicit alias can be supported, you'll just have to do a program-wide check to see if the variable on the left hand side is ever used in a type annotation.

---

_Comment by @Daverball on 2025-04-18 09:08_

This happens because of the string forward reference for `TypeAlias`, ruff understands type aliases just fine without the forward reference.

I see two potential ways to address this:

1. If the trimmed annotation only contains alphanumeric characters and dots, try to resolve that name and if it resolves to `{typing module}.TypeAlias` take the same branch as if it hadn't been a forward reference. This seems however like it might add quite a bit of overhead, since we're doing speculative work for every `AnnAssign` with a string annotation.

2. If we encounter `{typing module}.TypeAlias` when walking a deferred string annotation, check if the parent node is a `AnnAssign` and if so, re-walk the `value` expression of that node to collect all the string literals into `string_type_definitions` so they get parsed and walked in the next iteration of the checker loop dealing with string forward references.

---

_Comment by @Daverball on 2025-04-18 09:15_

That being said, I think only 1. really promises correct semantics for rules like TC007 and TC008 that depend on the `in_annotated_type_alias`/`in_type_alias` checks.

---

_Label `bug` added by @ntBre on 2025-04-23 14:10_

---

_Comment by @pekkaklarck on 2025-04-26 12:16_

This seems to be similar to #9298.


---

_Comment by @Daverball on 2025-04-26 15:13_

@pekkaklarck It's not really related to the other issue. The other issue is mostly about lack of type inference and multi-file analysis, so ruff doesn't know which subscripts are generics and which ones aren't. There isn't really a better solution to this than what we currently have, because if you start treating more things as generic subscripts instead of regular subscripts, you will instead start getting false positives for `undefined-name` (`F821`) and other rules that rely on the string being correctly interpreted as a string, rather than a forward reference.

This issue on the other hand is more of a chicken and egg problem. In order to know that this assignment is an annotated type alias we need to resolve the forward reference, but in order to correctly resolve forward references, we need to defer evaluating them until the entire module has been analyzed.

But once we have determined that this is indeed a type alias, it is already too late, because we already had to analyze the r.h.s. of the assignment without the knowledge that this is a type alias. If we defer the r.h.s. the references will be incorrect once we finally do walk it and if we don't defer the annotation, the references in the annotation will be incorrect.

So the best we can do is do, is to do the same thing we do for `from __future__ import annotations` and do a speculative lookup of the annotation to see whether or not it resolves to `{typing module}.TypeAlias`, which would require us to parse all forward references in `AnnAssign.annotation` eagerly, rather than lazily. You can still get the wrong result that way by moving the import of `TypeAlias` below its use, but that's a highly unorthodox use-case, so likely not a problem many people will encounter.

We may be able to get away with something simpler, like trying to interpret the annotation as a dotted name. If the text isn't a dotted name we know it can't resolve to `{typing module}.TypeAlias` and if the dotted name doesn't resolve to `{typing module}.TypeAlias` we know that it's very unlikely that it will ever resolve to it, even after we analyze the rest of the module. But doing that might be more expensive than eagerly parsing the annotation and doing the standard name matching lookup, since we need to eventually parse it anyways.

---
