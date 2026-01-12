```yaml
number: 15793
title: "[`refurb`] Add coverage for using set(...) in `single-item-membership-test` (`FURB171`)"
type: pull_request
state: closed
author: naslundx
labels:
  - rule
  - preview
assignees: []
base: main
head: single-item-membership-test-missing-set-constructor
created_at: 2025-01-28T20:14:50Z
updated_at: 2025-05-04T18:26:57Z
url: https://github.com/astral-sh/ruff/pull/15793
synced_at: 2026-01-12T15:55:52Z
```

# [`refurb`] Add coverage for using set(...) in `single-item-membership-test` (`FURB171`)

---

_@naslundx_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds coverage of using `set(...)` in addition to `{...} in SingleItemMembershipTest.

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #15792

## Test Plan

Updated unit test and snapshot.
Steps to reproduce are in the issue linked above.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-01-28 20:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @naslundx on 2025-01-28 21:01_

Ah, I see, it doesn't work if the single argument to `set()` is an enum, for example. :(

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/refurb/FURB171.py`:28 on 2025-01-28 21:03_

hmm, this fails at runtime:

```pycon
>>> set(1)
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    set(1)
    ~~~^^^
TypeError: 'int' object is not iterable
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/refurb/FURB171.py`:66 on 2025-01-28 21:04_

this also fails at runtime:

```pycon
>>> set(1, 2)
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    set(1, 2)
    ~~~^^^^^^
TypeError: set expected at most 1 argument, got 2
```

---

_@AlexWaygood requested changes on 2025-01-28 21:05_

Thanks! However, I think it would be incorrect for this rule to trigger on `set(1)`, since that doesn't create a set with a single member at runtime. It raises an exception.

---

_Comment by @AlexWaygood on 2025-01-29 11:10_

To clarify, @naslundx, all of these succeed at runtime, and I'd be okay with adding support for these to `FURB171`!

```py
set([1])
set((1,))
set({1})
```

You'd need to check that the object being passed to the `set()` call is an `Expr::List`, `Expr::Tuple` or `Expr::Set` that has exactly one member in its `elts` field.

---

_Comment by @InSyncWithFoo on 2025-01-29 12:43_

> You'd need to check that the object [...] has exactly one member in its `elts` field.

Additionally, that element should be literal/hashable, so that errors like this one are not masked:

```python
if a in set([{}]): ...  # TypeError: unhashable type: 'dict'
if a == {}: ...         # No errors
```

---

_Comment by @dylwil3 on 2025-01-30 13:30_

> > You'd need to check that the object [...] has exactly one member in its `elts` field.
> 
> Additionally, that element should be literal/hashable, so that errors like this one are not masked:
> 
> ```python
> if a in set([{}]): ...  # TypeError: unhashable type: 'dict'
> if a == {}: ...         # No errors
> ```

I think we should open a separate issue about this because that behavior is already present in the current scope of the rule:

```python
1 in {set()} # TypeError: unhashable type: 'set'
1 == set() # No errors
```

---

_Label `rule` added by @dylwil3 on 2025-01-30 13:33_

---

_Label `preview` added by @dylwil3 on 2025-01-30 13:33_

---

_Renamed from "(FURB171) Add coverage of using set(...) in SingleItemMembershipTest" to "[`refurb`] Add coverage for using set(...) in `single-item-membership-test` (`FURB171`)" by @dylwil3 on 2025-01-30 13:46_

---

_Comment by @tdulcet on 2025-01-30 20:47_

Maybe check for `frozenset` as well:
```py
frozenset([1])
frozenset((1,))
frozenset({1})
```

---

_Comment by @naslundx on 2025-03-03 20:22_

I did not see the updated comments on this PR - I will bring it back up to speed. Thanks for comments!

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-04 09:01_

---

_Review requested from @dylwil3 by @AlexWaygood on 2025-03-04 10:30_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-03-04 12:52_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:141 on 2025-03-05 11:51_

You should use this method to make sure the function matches the builtin that you expect:

https://github.com/astral-sh/ruff/blob/d94a78a134c438c063d144e11f2ae8c1657dfa99/crates/ruff_python_semantic/src/model.rs#L316-L319

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/refurb/FURB171.py`:20 on 2025-03-05 11:58_

Could you add some examples with nested calls to `set`/`frozenset` since your implementation is recursive? Like

```python
set(set([1]))
```

Also a few non-examples, like:

```python
set(1,) # this is a TypeError so we shouldn't do anything with it
set(1,2) # this is also a TypeError
set((x for x in range(2))) # this is legal but not one element
```

---

_@dylwil3 requested changes on 2025-03-05 11:58_

Looking good, thank you! Let's add a little more test coverage, and handle the case where someone is shadowing or explicitly importing the builtin.

---

_@naslundx reviewed on 2025-03-05 18:01_

---

_Review comment by @naslundx on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:141 on 2025-03-05 18:01_

Did not know about this, that's great!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-17 16:45_

---

_Comment by @MichaReiser on 2025-04-28 07:23_

Thanks @naslundx for working on this. I'll close this PR because it has become stale. For anyone interested, feel free to open a new PR implementing this change, incorporating the feedback from this review.

---

_Closed by @MichaReiser on 2025-04-28 07:23_

---

_Comment by @naslundx on 2025-05-04 18:26_

A little late to the party but I made the fixes, and will put up an updated PR

---
