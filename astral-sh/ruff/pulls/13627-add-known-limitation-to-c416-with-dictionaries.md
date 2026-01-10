```yaml
number: 13627
title: "Add known limitation to `C416` with dictionaries"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/c416-unsafe
created_at: 2024-10-04T15:29:40Z
updated_at: 2024-10-07T16:30:08Z
url: https://github.com/astral-sh/ruff/pull/13627
synced_at: 2026-01-10T20:59:36Z
```

# Add known limitation to `C416` with dictionaries

---

_Pull request opened by @zanieb on 2024-10-04 15:29_

Part of https://github.com/astral-sh/ruff/issues/13625

See also #13629 

---

_Label `documentation` added by @zanieb on 2024-10-04 15:29_

---

_Comment by @github-actions[bot] on 2024-10-04 15:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-10-04 18:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:34 on 2024-10-04 18:55_

Hmm, not sure about this wording. I don't think this issue is specific to dictionaries with `tuple` keys. Rather, it's specific to when you're iterating over dictionaries. In general, for a given object `d`, `dict(d)` is going to give you a different object than `{a: b for a, b in d}` if `d` is a dictionary. E.g. you see this also with `frozenset` keys:

```pycon
>>> d = {frozenset({1, 2}): 'foo'}
>>> dict(d)
{frozenset({1, 2}): 'foo'}
>>> {a: b for a, b in d}
{1: 2}
```

---

_@zanieb reviewed on 2024-10-04 19:07_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:34 on 2024-10-04 19:07_

Hm yeah — I thought the example was suggesting that `for x, y in dict` differed in behavior depending on the type of key. It seems like we just shouldn't raise this violation for dictionaries in general i.e. where `d1` is a dictionary and `.items()` is not called?

```python
d1 = {1: 3, 2: 5}
d2 = {x: y for x, y in d1}
```

---

_Converted to draft by @zanieb on 2024-10-04 19:07_

---

_@AlexWaygood reviewed on 2024-10-04 19:14_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:34 on 2024-10-04 19:14_

Yeah, I think that's correct. Possibly we could try to detect if the type of the binding is a dictionary using something like https://github.com/astral-sh/ruff/blob/020f4d4a54eec15b53a5fd94409600f1ad0a63aa/crates/ruff_python_semantic/src/analyze/typing.rs#L752? But I think it's not fully accurate without generalised type inference, and possibly more expensive. Not sure, WDYT?

---

_@zanieb reviewed on 2024-10-04 19:15_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:34 on 2024-10-04 19:15_

I think we can do something reasonable with that. I'll look into it, but not with a high priority.

---

_@zanieb reviewed on 2024-10-04 19:25_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:34 on 2024-10-04 19:25_

I guess this only applies to keys that can be unpacked because otherwise that code is a runtime error. Regardless, it seems like we shouldn't depend on that here and should fix the false positive as I described.

---

_@AlexWaygood reviewed on 2024-10-04 19:28_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:34 on 2024-10-04 19:28_

If we're in a situation where we can statically detect that the keys can't be unpacked but the user appears to be trying to unpack them anyway in a comprehension, then we haven't really got any clue what the user wanted, so I don't think we should emit a violation for that situation either tbh. I'd just not emit the violation ever if we can see that they're iterating over a binding that we know is a dictionary, and they haven't called `.items()` on the dictionary before iterating over it.

---

_Renamed from "Add known limitation to `C416` with tuple keys" to "Add known limitation to `C416` with dictionaries" by @zanieb on 2024-10-04 20:26_

---

_Marked ready for review by @zanieb on 2024-10-04 20:58_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-07 08:24_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:45 on 2024-10-07 13:05_

How about something like this?

```suggestion
/// This rule may produce false positives for `dict` comprehensions due to the fact that
/// the `dict` constructor method behaves differently depending on whether it is passed
/// a sequence (such as a `list`) or a mapping (such as a `dict`). If a `dict` comprehension
/// iterates over the keys of an existing dictionary to create a new one, rewriting the
/// comprehension to use `dict()` directly according to this rule's suggestion will not
/// produce the same result.
///
/// For example:
/// ```pycon
/// >>> d1 = {(1, 2): 3, (4, 5): 6}
/// >>> {x: y for x, y in d1}  # iterates over the keys and unpacks each tuple key
/// {1: 2, 4: 5}
/// >>> dict(d1)               # creates an exact copy of the original mapping
/// {(1, 2): 3, (4, 5): 6}
/// ```
///
/// The suggested change is still valid for `dict` comprehensions if the object being
/// iterated over is a `list` or some other iterable, however. Ruff currently has
/// limited type inference, so cannot reliably infer if the type of the object being
/// iterated over makes the suggestion valid.
```

---

_@AlexWaygood reviewed on 2024-10-07 13:05_

---

_@zanieb reviewed on 2024-10-07 15:58_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:45 on 2024-10-07 15:58_

I took parts of this, but edited it for clarity and for consistency with #13629

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:45 on 2024-10-07 15:59_

```suggestion
/// >>> dict(d1)               # Suggested (incorrect) fix
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:50 on 2024-10-07 16:00_

It's not really just the autofix that's incorrect in these cases: the message and suggestion of the rule's diagnostic is also incorrect in these cases

```suggestion
/// When the comprehension iterates over a sequence, the rule's suggestion is correct. 
/// However, Ruff cannot consistently infer if the iterable type is a sequence or a mapping.
```

---

_@AlexWaygood approved on 2024-10-07 16:00_

Nice!

---

_@zanieb reviewed on 2024-10-07 16:02_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:45 on 2024-10-07 16:02_

I wrote it this way initially as "Incorrect suggested fix" but I'm not sure it's better.

---

_@zanieb reviewed on 2024-10-07 16:03_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:50 on 2024-10-07 16:03_

I mean, it should be `dict(iterable.keys())` right? I think the rule is correct and just the fix is wrong?

---

_@AlexWaygood reviewed on 2024-10-07 16:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:45 on 2024-10-07 16:04_

I think otherwise it could possibly be misread by somebody skim-reading as saying that it's what we're suggesting here, in the docs. And that's obviously not the case 😄

---

_@AlexWaygood reviewed on 2024-10-07 16:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:50 on 2024-10-07 16:04_

Oh. Yeah, great point :)

---

_@zanieb reviewed on 2024-10-07 16:05_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:45 on 2024-10-07 16:05_

I pushed a fix that should resolve review comments.

---

_@AlexWaygood approved on 2024-10-07 16:05_

---

_Comment by @zanieb on 2024-10-07 16:06_

I think we should try to improve C416 before it's stabilized, so it's not wrong so often. The fix could be safe for non-dict comprehensions if we don't mind dropping some comments.

---

_Comment by @AlexWaygood on 2024-10-07 16:08_

The policy I've been using for the minor releases I've been shepherding has been not to stabilise any rules that have significant open issues on the tracker

---

_Merged by @zanieb on 2024-10-07 16:20_

---

_Closed by @zanieb on 2024-10-07 16:20_

---

_Branch deleted on 2024-10-07 16:20_

---
