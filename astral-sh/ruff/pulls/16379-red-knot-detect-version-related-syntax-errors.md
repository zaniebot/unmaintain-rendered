```yaml
number: 16379
title: "[red-knot] Detect version-related syntax errors"
type: pull_request
state: merged
author: ntBre
labels:
  - ty
assignees: []
merged: true
base: main
head: brent/syntax-errors-red-knot
created_at: 2025-02-25T18:19:19Z
updated_at: 2025-04-17T19:34:57Z
url: https://github.com/astral-sh/ruff/pull/16379
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Detect version-related syntax errors

---

_@ntBre_

## Summary
This PR extends version-related syntax error detection to red-knot. The main changes here are:

1. Passing `ParseOptions` specifying a `PythonVersion` to parser calls
2. Adding a `python_version` method to the `Db` trait to make this possible
3. Converting `UnsupportedSyntaxError`s to `Diagnostic`s
4. Updating existing mdtests  to avoid unrelated syntax errors

My initial draft of (1) and (2) in #16090 instead tried passing a `PythonVersion` down to every parser call, but @MichaReiser suggested the `Db` approach instead [here](https://github.com/astral-sh/ruff/pull/16090#discussion_r1969198407), and I think it turned out much nicer.

All of the new `python_version` methods look like this:

```rust
fn python_version(&self) -> ruff_python_ast::PythonVersion {
    Program::get(self).python_version(self)
}
```

with the exception of the `TestDb` in `ruff_db`, which hard-codes `PythonVersion::latest()`.

## Test Plan

Existing mdtests, plus a new mdtest to see at least one of the new diagnostics.

---

_Label `red-knot` added by @ntBre on 2025-02-25 18:19_

---

_Renamed from "[`red-knot`] Detect version-related syntax errors" to "[red-knot] Detect version-related syntax errors" by @ntBre on 2025-02-25 18:19_

---

_Comment by @codspeed-hq[bot] on 2025-02-25 19:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fsyntax-errors-red-knot)

### Merging #16379 will **not alter performance**

<sub>Comparing <code>brent/syntax-errors-red-knot</code> (2ede1e0) with <code>main</code> (d2ebfd6)</sub>



### Summary

`✅ 33` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-02-25 19:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @github-actions[bot] on 2025-04-15 21:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
psycopg (https://github.com/psycopg/psycopg)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/async_to_sync.py:243:9: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/async_to_sync.py:345:13: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/async_to_sync.py:355:9: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/async_to_sync.py:371:9: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/async_to_sync.py:379:13: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/async_to_sync.py:390:9: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/async_to_sync.py:416:13: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/async_to_sync.py:459:9: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ error[invalid-syntax] /tmp/mypy_primer/projects/psycopg/tools/bump_version.py:103:9: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
- Found 1589 diagnostics
+ Found 1598 diagnostics

```
</details>


---

_Marked ready for review by @ntBre on 2025-04-16 17:13_

---

_Review requested from @carljm by @ntBre on 2025-04-16 17:13_

---

_Review requested from @AlexWaygood by @ntBre on 2025-04-16 17:13_

---

_Review requested from @sharkdp by @ntBre on 2025-04-16 17:13_

---

_Review requested from @dcreager by @ntBre on 2025-04-16 17:13_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-16 17:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:6 on 2025-04-17 06:32_

Maybe move this down to the `Using `typing.ParamSpec`` section?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:7 on 2025-04-17 06:35_

Maybe move this down to the `Tuple annotations are understood` section?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:6 on 2025-04-17 06:36_

Can we move this to the `Classmethods mixed with other decorators` test?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclasses.md`:6 on 2025-04-17 06:36_

Can we move this to the `Generic dataclasses` test?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:6 on 2025-04-17 06:38_

Maybe move this to the `Stub function` section?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:6 on 2025-04-17 06:41_

Move this down to the "Esoteric definitions and redefinintions" section?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:6 on 2025-04-17 06:42_

Move this to "PEP 695 generic" test?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:6 on 2025-04-17 06:42_

Move this to "Narrowing for generic classes"?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:8 on 2025-04-17 06:43_

Move this to the "Cyclical class definition" test just below?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:6 on 2025-04-17 06:44_

Move to "Class, module and function literals"?

---

_@sharkdp approved on 2025-04-17 06:48_

This looks great — thank you @ntBre!

As you suggested in the PR description, I think we should try to be a bit more fine grained with increasing Python versions in the tests. Python typing has evolved a lot between 3.9 and 3.13, and we want to make sure that a large part of our tests still work for 3.9.

---

_Comment by @ntBre on 2025-04-17 14:02_

Thanks @sharkdp! Sorry about the mdtests, I didn't mean to make you go through them all, but I appreciate it. I'll update those now.

---

_Comment by @sharkdp on 2025-04-17 17:41_

> Thanks @sharkdp! Sorry about the mdtests, I didn't mean to make you go through them all, but I appreciate it. I'll update those now.

No worries! I was interested in how it affected particular parts of our test suite and looked at it anyway.



Interestingly, we now see some ecosystem changes (I merged a PR which added some new projects this morning). And they are correct, because https://github.com/psycopg/psycopg does not specify a python-version. I would like to make some improvements to how the ecosystem checks work (how exactly we run red knot on projects), but for now, I think we can just ignore it. Might even be interesting to have a few true positives in there.

---

_Comment by @ntBre on 2025-04-17 17:53_

> Interestingly, we now see some ecosystem changes (I merged a PR which added some new projects this morning). And they are correct, because https://github.com/psycopg/psycopg does not specify a python-version. I would like to make some improvements to how the ecosystem checks work (how exactly we run red knot on projects), but for now, I think we can just ignore it. Might even be interesting to have a few true positives in there.

Oh good catch, I hadn't scrolled back up to check these after merging `main`. 

So go ahead and merge? I can also leave it open a bit longer in case anyone else wants to review it.

(I see some emotes from @AlexWaygood, so I can consider that two reviews :laughing:)

---

_Comment by @sharkdp on 2025-04-17 17:58_

I say, go ahead an merge it! :smile: Others can still do post-merge reviews, if they want.

---

_Merged by @ntBre on 2025-04-17 18:00_

---

_Closed by @ntBre on 2025-04-17 18:00_

---

_Branch deleted on 2025-04-17 18:00_

---

_@dhruvmanila reviewed on 2025-04-17 19:34_

Looks good!

---
