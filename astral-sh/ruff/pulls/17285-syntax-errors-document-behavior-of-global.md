```yaml
number: 17285
title: "[syntax-errors] Document behavior of `global` declarations in `try` nodes before 3.13"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/syn-global-in-try
created_at: 2025-04-07T19:51:49Z
updated_at: 2025-04-09T16:54:23Z
url: https://github.com/astral-sh/ruff/pull/17285
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Document behavior of `global` declarations in `try` nodes before 3.13

---

_Pull request opened by @ntBre on 2025-04-07 19:51_

Summary
--

This PR extends the documentation of the `LoadBeforeGlobalDeclaration` check to specify the behavior on versions of Python before 3.13. Namely, on Python 3.12, the `else` clause of a `try` statement is visited before the `except` handlers:

```pycon
Python 3.12.9 (main, Feb 12 2025, 14:50:50) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> a = 10
>>> def g():
...     try:
...         1 / 0
...     except:
...         a = 1
...     else:
...         global a
...
>>> def f():
...     try:
...         pass
...     except:
...         global a
...     else:
...         print(a)
...
  File "<stdin>", line 5
SyntaxError: name 'a' is used prior to global declaration

```

The order is swapped on 3.13 (see [CPython#111123](https://github.com/python/cpython/issues/111123)):

```pycon
Python 3.13.2 (main, Feb  5 2025, 08:05:21) [GCC 14.2.1 20250128] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> a = 10
... def g():
...     try:
...         1 / 0
...     except:
...         a = 1
...     else:
...         global a
...
  File "<python-input-0>", line 8
    global a
    ^^^^^^^^
SyntaxError: name 'a' is assigned to before global declaration
>>> def f():
...     try:
...         pass
...     except:
...         global a
...     else:
...         print(a)
...
>>>
```

The current implementation of PLE0118 is correct for 3.13 but not 3.12: [playground](https://play.ruff.rs/d7467ea6-f546-4a76-828f-8e6b800694c9) (it flags the first case regardless of Python version). 

We decided to maintain this incorrect diagnostic for Python versions before 3.13 because the pre-3.13 behavior is very unintuitive and confirmed to be a bug, although the bug fix was not backported to earlier versions. This can lead to false positives and false negatives for pre-3.13 code, but we also expect that to be very rare, as demonstrated by the ecosystem check (before the version-dependent check was reverted here).

Test Plan
--

N/a

---

_Label `rule` added by @ntBre on 2025-04-07 19:51_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-07 19:51_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-07 19:51_

---

_Comment by @ntBre on 2025-04-07 19:53_

I just realized that this might need to be gated behind preview since it modifies an existing, stable lint rule. Alternatively, I could just use a different `SemanticSyntaxErrorKind` here, which seems a little easier. On the other hand, this could probably be considered a bug in the current rule implementation.

---

_Comment by @github-actions[bot] on 2025-04-07 20:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @ntBre on 2025-04-07 20:04_

Oh, there's a false positive anyway, moving back to draft.

---

_Converted to draft by @ntBre on 2025-04-07 20:04_

---

_Comment by @ntBre on 2025-04-08 16:11_

The false positive should now be handled by tracking a scope depth in the source-order visitor to avoid flagging more-deeply-nested references. I think functions and classes are the only ways to introduce this kind of scope, in which `global` statements are also allowed (I don't think you can put a `global` statement in a comprehension or lambda).

I think `u32` should be huge overkill for the scope depth tracking, so I'm more than happy to drop the size of that to a more reasonable level, but I wasn't sure how low to go. Is `u8` enough?

---

_Marked ready for review by @ntBre on 2025-04-08 16:18_

---

_Comment by @dhruvmanila on 2025-04-08 22:06_

> I think `u32` should be huge overkill for the scope depth tracking, so I'm more than happy to drop the size of that to a more reasonable level, but I wasn't sure how low to go. Is `u8` enough?

`u32` should be fine, we use the same for `ScopeId`:

https://github.com/astral-sh/ruff/blob/058439d5d3f65db515d69359c99838d60776f45e/crates/ruff_python_semantic/src/scope.rs#L188-L194

---

_Comment by @dhruvmanila on 2025-04-08 22:07_

> On Python 3.12, the `else` clause is visited before the `except` handlers:

This is confusing, do we have a source like a changelog entry where the related change is mentioned for context? It might also be useful to add that as comment in the source code.

---

_@dhruvmanila reviewed on 2025-04-08 22:10_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:142 on 2025-04-08 22:10_

We should use the `visit_body` method instead in here, orelse, handlers, and finalbody.

---

_Comment by @ntBre on 2025-04-08 22:11_

> > On Python 3.12, the `else` clause is visited before the `except` handlers:
> 
> This is confusing, do we have a source like a changelog entry where the related change is mentioned for context? It might also be useful to add that as comment in the source code.

This is the [bug report](https://github.com/python/cpython/issues/111123) Alex linked to in #11934. I'll look for anything else and at least link to this in the enum variant docs!

---

_Comment by @dhruvmanila on 2025-04-08 22:14_

> This is the [bug report](https://github.com/python/cpython/issues/111123) Alex linked to in #11934. I'll look for anything else and at least link to this in the enum variant docs!

I think just linking to the CPython issue should be enough.

---

_@dhruvmanila reviewed on 2025-04-08 22:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:142 on 2025-04-08 22:24_

except for the "handlers" loop

---

_Comment by @dhruvmanila on 2025-04-08 22:25_

(I need to head out now, I'll continue with the review later in the evening as I need to look at it a bit more closely to understand.)

---

_Comment by @MichaReiser on 2025-04-09 06:21_

> I just realized that this might need to be gated behind preview since it modifies an existing, stable lint rule. Alternatively, I could just use a different SemanticSyntaxErrorKind here, which seems a little easier. On the other hand, this could probably be considered a bug in the current rule implementation.

Can you tell me more in how the new implementation diverges from the existing one?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:139 on 2025-04-09 06:24_

Using a custom visitor here is probably fine because try statements aren't too deeply nested (they rarely contain nested functions) but this is now a case where the visitor ends up visiting entire suites multiple times: 

1. In the `TryExcept` visitor
2. The outer visitor

I've a weak preference to avoid the double visitation unless it increases complexity by a lot

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:142 on 2025-04-09 06:24_

I think the easiest here is to call `walk_stmt(&visitor, stmt)` because this is an exact reimplementation of visiting a try statement.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1366 on 2025-04-09 06:25_

What about nested `try` statements?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1358 on 2025-04-09 06:26_

What about lambda expressions?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1359 on 2025-04-09 06:30_

I find it unfortunate that we're collecting all identifiers here, even if there isn't a single global statement in the program (which should be the vast majority of programs). 

It also has the downside that globals handling is now handled here but also something that we expect the caller to handle (`context.global(name)`). Could we instead make use that `context` already provides a `global` method or extend the `context` interface to avoid doing this work twice?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1325 on 2025-04-09 06:31_

We should store `name` references here (you have the `'a` lifetime anyway)

---

_@MichaReiser reviewed on 2025-04-09 06:31_

We should add a few more tests and explore if there's a way to avoid duplicating the globals handling between the semantic syntax checker and the caller.

---

_Comment by @ntBre on 2025-04-09 12:09_

> Can you tell me more in how the new implementation diverges from the existing one?

The divergence is just the version-dependence of the branch visiting order. From the summary:

> The current implementation of PLE0118 is correct for 3.13 but not 3.12: [playground](https://play.ruff.rs/d7467ea6-f546-4a76-828f-8e6b800694c9) (it flags the first case regardless of Python version).

Phrasing it that way, it clearly sounds like a bug, but I think the old behavior was so unintuitive that probably nobody runs into it

---

_@ntBre reviewed on 2025-04-09 12:12_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:142 on 2025-04-09 12:12_

`walk_stmt` would only match one of these orders, right? We need to swap the order of the `orelse` and `handlers` visits depending on the version. I could still use it in one of the branches and then inline the whole modified version in the other branch.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1358 on 2025-04-09 12:15_

Could you expand on that? I thought the `SourceOrderVisitor` would handle descending into lambda expressions for me. Or do they have another way of accessing an identifier that's not covered here?

---

_@ntBre reviewed on 2025-04-09 12:15_

---

_@ntBre reviewed on 2025-04-09 12:18_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1366 on 2025-04-09 12:18_

I don't think `try` introduces a new scope:

```pycon
>>> try:
...     x = 1
... except:
...     pass
...
>>> x
1
>>>
```

I think only functions and classes do. Is that what you were asking?

---

_@MichaReiser reviewed on 2025-04-09 13:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1358 on 2025-04-09 13:29_

Yes, the visitor traverses into lambdas. The question is whether it should (e.g. we don't traverse into nested functions or classes)

---

_@ntBre reviewed on 2025-04-09 13:37_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1358 on 2025-04-09 13:37_

I might be getting confused here. Why don't we traverse into nested functions or classes? I thought the `source_order::walk_stmt` call in `visit_stmt` below would handle that.

The idea here was just to collect all identifiers and then compare them to the identifiers in any following `global` statements. If I missed traversing any kind of node that can contain identifiers, then I think it's a mistake, not my intention.

It sounds like I should add more tests with nested structures in any case.

---

_@MichaReiser reviewed on 2025-04-09 14:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:142 on 2025-04-09 14:00_

You're right, we traverse into function statements but we increase the `scope` counter. I have to take another look to understand what the scope counter is even used for but I suspect that different rules apply for names and global statements inside nested functions, classes and it's unclear to me if that also applies to lambdas (can't have any global statements but it may have identifiers).

So what I'm looking for is a test that demonstrates the behavior when lambdas are involved.

---

_@ntBre reviewed on 2025-04-09 14:24_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:139 on 2025-04-09 14:24_

I'll give this a try (no pun intended). The nice thing about the visitor is that it let me control the order in which things are seen, and I could just swap the order of the visits on different Python versions. I think that order-dependence might be hard to capture otherwise, but I'll play with it a bit.

The easiest thing to do would be to move that reordering into the parent visitor, but that seems too intrusive.

---

_Label `documentation` added by @ntBre on 2025-04-09 16:04_

---

_Label `rule` removed by @ntBre on 2025-04-09 16:04_

---

_Comment by @ntBre on 2025-04-09 16:08_

Based on our discussion on Discord, I reverted all of the code changes here and simply extended the rule variant documentation to reflect our decision to enforce the 3.13 clause ordering on all Python versions.

I initially planned to keep the inline tests to demonstrate this, but they would now rely on a proper implementation of `SemanticSyntaxContext::global` for the test context to work, so I just deleted them too. We could add a linter test for PLE0118 showing this, though, if we wanted.

---

_Comment by @ntBre on 2025-04-09 16:17_

The `cargo-shear` failure was an issue in downloading/building it, not an extra dependency. I'll try to rerun it

---

_@MichaReiser approved on 2025-04-09 16:38_

Thanks and sorry for not raising this sooner

---

_Comment by @ntBre on 2025-04-09 16:40_

No problem at all, I thought it was a useful discussion!

---

_Renamed from "[syntax-errors] `global` declarations in `try` nodes before 3.13" to "[syntax-errors] Document behavior of `global` declarations in `try` nodes before 3.13" by @ntBre on 2025-04-09 16:51_

---

_Merged by @ntBre on 2025-04-09 16:54_

---

_Closed by @ntBre on 2025-04-09 16:54_

---

_Branch deleted on 2025-04-09 16:54_

---
