```yaml
number: 17976
title: Implement a recursive check for RUF060
type: pull_request
state: merged
author: naslundx
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: recursive-check-in-ruf060
created_at: 2025-05-09T08:23:22Z
updated_at: 2025-05-15T20:24:16Z
url: https://github.com/astral-sh/ruff/pull/17976
synced_at: 2026-01-10T18:51:01Z
```

# Implement a recursive check for RUF060

---

_Pull request opened by @naslundx on 2025-05-09 08:23_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
The existing implementation of RUF060 (InEmptyCollection) is not recursive, meaning that although set([]) results in an empty collection, the existing code fails it because set is taking an argument.

The updated implementation allows set and frozenset to take empty collection as positional argument (which results in empty set/frozenset).

## Test Plan

Added test cases for recursive cases + updated snapshot (see RUF060.py).

---

_Comment by @github-actions[bot] on 2025-05-09 08:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-05-09 16:25_

Thanks!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/in_empty_collection.rs`:78 on 2025-05-09 16:34_

Actually, is this part correct? I tested locally and 
```python
frozenset() in {frozenset()}
```

is getting a diagnostic now, even though Python returns `True`:

```pycon
>>> frozenset() in {frozenset()}
True
```

I think we may need to limit the recursive case to calls like down below.

---

_@ntBre reviewed on 2025-05-09 16:34_

---

_@naslundx reviewed on 2025-05-09 19:56_

---

_Review comment by @naslundx on `crates/ruff_linter/src/rules/ruff/rules/in_empty_collection.rs`:78 on 2025-05-09 19:56_

Oh, that's a very good find. Indeed, I did not consider the difference between how the two ways of constructing a set differ

```
>>> set([frozenset()]) == {frozenset()}  # note the [...]
True
```

Will fix

---

_@naslundx reviewed on 2025-05-09 20:00_

---

_Review comment by @naslundx on `crates/ruff_linter/src/rules/ruff/rules/in_empty_collection.rs`:78 on 2025-05-09 20:00_

Did not expect this either, but...

```
>>> len({""})
1

>>> len(set(""))
0
```

---

_Comment by @naslundx on 2025-05-09 20:21_

Thanks @ntBre - I reduced the PR but added some more test cases from comments above. (changes in new commit)

---

_Label `rule` added by @dylwil3 on 2025-05-11 16:29_

---

_Label `preview` added by @dylwil3 on 2025-05-11 16:29_

---

_@ntBre reviewed on 2025-05-12 20:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/in_empty_collection.rs`:78 on 2025-05-12 20:13_

Ah that is a funny one, calling `set` on a string will create a set of the characters:

```pycon
>>> set("123")
{'1', '2', '3'}
```

so the empty string gives the empty set.

---

_@ntBre approved on 2025-05-12 20:15_

Thanks!

---

_Merged by @ntBre on 2025-05-12 20:17_

---

_Closed by @ntBre on 2025-05-12 20:17_

---

_@Avasam reviewed on 2025-05-15 20:24_

---

_Review comment by @Avasam on `crates/ruff_linter/src/rules/ruff/rules/in_empty_collection.rs`:78 on 2025-05-15 20:24_

Classic `str` is an `Iterable[str]` ^-^

---
