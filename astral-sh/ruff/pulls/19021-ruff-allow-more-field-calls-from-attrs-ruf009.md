```yaml
number: 19021
title: "[`ruff`] Allow more `field` calls from `attrs` (`RUF009`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: brent/fix-ruf009
created_at: 2025-06-28T23:16:26Z
updated_at: 2025-07-03T14:29:57Z
url: https://github.com/astral-sh/ruff/pull/19021
synced_at: 2026-01-12T15:56:29Z
```

# [`ruff`] Allow more `field` calls from `attrs` (`RUF009`)

---

_@ntBre_

Summary
--

Closes #19014 by identifying more `field` functions from `attrs`. We already detected these when imported from `attrs` but not the `attr` module from the same package. These functions are identical to the `attrs` versions:

```pycon
>>> import attrs, attr
>>> attrs.field is attr.field
True
>>> attrs.Factory is attr.Factory
True
>>>
```

Test Plan
--

Regression tests based on the issue

---

_Label `bug` added by @ntBre on 2025-06-28 23:16_

---

_Label `rule` added by @ntBre on 2025-06-28 23:16_

---

_Comment by @github-actions[bot] on 2025-06-28 23:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @ntBre on 2025-06-29 00:56_

---

_Comment by @alex-700 on 2025-06-29 20:13_

Hi @ntBre !

I think the fix should also include `attr.attr` and `attr.attrib`.
https://github.com/python-attrs/attrs/blob/main/src/attr/__init__.py#L33

Also `attr.attributes` is missing [here](https://github.com/astral-sh/ruff/blob/e7aadfc28b5a1447c45d39017a37f1acd1001f14/crates/ruff_linter/src/rules/ruff/helpers.rs#L123) (see [aliases](https://github.com/python-attrs/attrs/blob/main/src/attr/__init__.py#L32)).

---

_Comment by @ntBre on 2025-07-03 13:28_

@alex-700 Thank you very much for catching those and for the links! I think I've covered all of them now!

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-03 13:29_

---

_@AlexWaygood approved on 2025-07-03 13:44_

nice!

---

_Merged by @ntBre on 2025-07-03 14:29_

---

_Closed by @ntBre on 2025-07-03 14:29_

---

_Branch deleted on 2025-07-03 14:29_

---
