```yaml
number: 12595
title: "Display fixes by default (#7352)"
type: pull_request
state: closed
author: chriskrycho
labels:
  - cli
assignees: []
base: main
head: diff-check-by-default
created_at: 2024-07-31T15:28:01Z
updated_at: 2024-09-04T21:57:14Z
url: https://github.com/astral-sh/ruff/pull/12595
synced_at: 2026-01-12T15:55:41Z
```

# Display fixes by default (#7352)

---

_@chriskrycho_

## Summary

Use the existing `printer::Flags::SHOW_FIX_DIFF` to enable printing fixable diffs in `check` mode automatically. Enable it whenever the output format is `Full`, but keep a distinct flag for it (a) because output format and whether to print diffs are really separate concerns, (b) because it makes it easy to make changes to what sets the flag to true if/as that is needful, and (c) because it minimally perturbs the rest of the code.

Fixes #7352.

## Details

This changes the result of running `ruff check` by adding the diff for possible fixes, and adds calls to action for those messages.

Note that this currently displays *only* safe fixes to users. There are a number of design questions around when and how to display them; see #12598.

<details><summary>a file with three unused imports</summary>

Input:

```python
import b
import a
import c
```

Output:

```
/Users/chris/Desktop/lol.py:1:8: F401 [*] `b` imported but unused
  |
1 | import b
  |        ^ F401
2 | import a
3 | import c
  |
  = help: Remove unused import: `b`

Suggested fix:
1   |-import b
2 1 | import a
3 2 | import c
4 3 |

    Run `ruff check --fix` to apply this fix.

/Users/chris/Desktop/lol.py:2:8: F401 [*] `a` imported but unused
  |
1 | import b
2 | import a
  |        ^ F401
3 | import c
  |
  = help: Remove unused import: `a`

Suggested fix:
1 1 | import b
2   |-import a
3 2 | import c
4 3 |

    Run `ruff check --fix` to apply this fix.

/Users/chris/Desktop/lol.py:3:8: F401 [*] `c` imported but unused
  |
1 | import b
2 | import a
3 | import c
  |        ^ F401
  |
  = help: Remove unused import: `c`

Suggested fix:
1 1 | import b
2 2 | import a
3   |-import c
4 3 |

    Run `ruff check --fix` to apply this fix.

Found 3 errors.
[*] 3 fixable with the `--fix` option.
```

</details>

<details><summary>The test file <code>crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi</code></summary>

New output:

```
crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:2:40: F401 [*] `collections.abc.Set` imported but unused
  |
1 | def f():
2 |     from collections.abc import Set as AbstractSet  # Ok
  |                                        ^^^^^^^^^^^ F401
3 | 
4 | def f():
  |
  = help: Remove unused import: `collections.abc.Set`

Suggested fix:
1 1 | def f():
2   |-    from collections.abc import Set as AbstractSet  # Ok
  2 |+    pass  # Ok
3 3 | 
4 4 | def f():
5 5 |     from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:4:5: F811 Redefinition of unused `f` from line 1
  |
2 |     from collections.abc import Set as AbstractSet  # Ok
3 | 
4 | def f():
  |     ^ F811
5 |     from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  |
  = help: Remove definition: `f`

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:5:33: F401 [*] `collections.abc.Container` imported but unused
  |
4 | def f():
5 |     from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  |                                 ^^^^^^^^^ F401
6 | 
7 | def f():
  |
  = help: Remove unused import

Suggested fix:
2 2 |     from collections.abc import Set as AbstractSet  # Ok
3 3 | 
4 4 | def f():
5   |-    from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  5 |+    pass  # Ok
6 6 | 
7 7 | def f():
8 8 |     from collections.abc import Set  # PYI025

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:5:44: F401 [*] `collections.abc.Sized` imported but unused
  |
4 | def f():
5 |     from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  |                                            ^^^^^ F401
6 | 
7 | def f():
  |
  = help: Remove unused import

Suggested fix:
2 2 |     from collections.abc import Set as AbstractSet  # Ok
3 3 | 
4 4 | def f():
5   |-    from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  5 |+    pass  # Ok
6 6 | 
7 7 | def f():
8 8 |     from collections.abc import Set  # PYI025

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:5:58: F401 [*] `collections.abc.Set` imported but unused
  |
4 | def f():
5 |     from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  |                                                          ^^^^^^^^^^^ F401
6 | 
7 | def f():
  |
  = help: Remove unused import

Suggested fix:
2 2 |     from collections.abc import Set as AbstractSet  # Ok
3 3 | 
4 4 | def f():
5   |-    from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  5 |+    pass  # Ok
6 6 | 
7 7 | def f():
8 8 |     from collections.abc import Set  # PYI025

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:5:71: F401 [*] `collections.abc.ValuesView` imported but unused
  |
4 | def f():
5 |     from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  |                                                                       ^^^^^^^^^^ F401
6 | 
7 | def f():
  |
  = help: Remove unused import

Suggested fix:
2 2 |     from collections.abc import Set as AbstractSet  # Ok
3 3 | 
4 4 | def f():
5   |-    from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
  5 |+    pass  # Ok
6 6 | 
7 7 | def f():
8 8 |     from collections.abc import Set  # PYI025

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:7:5: F811 Redefinition of unused `f` from line 4
  |
5 |     from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
6 | 
7 | def f():
  |     ^ F811
8 |     from collections.abc import Set  # PYI025
  |
  = help: Remove definition: `f`

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:8:33: F401 [*] `collections.abc.Set` imported but unused
   |
 7 | def f():
 8 |     from collections.abc import Set  # PYI025
   |                                 ^^^ F401
 9 | 
10 | def f():
   |
   = help: Remove unused import: `collections.abc.Set`

Suggested fix:
5 5 |     from collections.abc import Container, Sized, Set as AbstractSet, ValuesView  # Ok
6 6 | 
7 7 | def f():
8   |-    from collections.abc import Set  # PYI025
  8 |+    pass  # PYI025
9 9 | 
10 10 | def f():
11 11 |     from collections.abc import Container, Sized, Set, ValuesView  # PYI025

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:10:5: F811 Redefinition of unused `f` from line 7
   |
 8 |     from collections.abc import Set  # PYI025
 9 | 
10 | def f():
   |     ^ F811
11 |     from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   |
   = help: Remove definition: `f`

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:11:33: F401 [*] `collections.abc.Container` imported but unused
   |
10 | def f():
11 |     from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   |                                 ^^^^^^^^^ F401
12 | 
13 | def f():
   |
   = help: Remove unused import

Suggested fix:
8  8  |     from collections.abc import Set  # PYI025
9  9  | 
10 10 | def f():
11    |-    from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   11 |+    pass  # PYI025
12 12 | 
13 13 | def f():
14 14 |     """Test: local symbol renaming."""

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:11:44: F401 [*] `collections.abc.Sized` imported but unused
   |
10 | def f():
11 |     from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   |                                            ^^^^^ F401
12 | 
13 | def f():
   |
   = help: Remove unused import

Suggested fix:
8  8  |     from collections.abc import Set  # PYI025
9  9  | 
10 10 | def f():
11    |-    from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   11 |+    pass  # PYI025
12 12 | 
13 13 | def f():
14 14 |     """Test: local symbol renaming."""

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:11:51: F401 [*] `collections.abc.Set` imported but unused
   |
10 | def f():
11 |     from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   |                                                   ^^^ F401
12 | 
13 | def f():
   |
   = help: Remove unused import

Suggested fix:
8  8  |     from collections.abc import Set  # PYI025
9  9  | 
10 10 | def f():
11    |-    from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   11 |+    pass  # PYI025
12 12 | 
13 13 | def f():
14 14 |     """Test: local symbol renaming."""

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:11:56: F401 [*] `collections.abc.ValuesView` imported but unused
   |
10 | def f():
11 |     from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   |                                                        ^^^^^^^^^^ F401
12 | 
13 | def f():
   |
   = help: Remove unused import

Suggested fix:
8  8  |     from collections.abc import Set  # PYI025
9  9  | 
10 10 | def f():
11    |-    from collections.abc import Container, Sized, Set, ValuesView  # PYI025
   11 |+    pass  # PYI025
12 12 | 
13 13 | def f():
14 14 |     """Test: local symbol renaming."""

    Run `ruff check --fix` to apply this fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:13:5: F811 Redefinition of unused `f` from line 10
   |
11 |     from collections.abc import Container, Sized, Set, ValuesView  # PYI025
12 | 
13 | def f():
   |     ^ F811
14 |     """Test: local symbol renaming."""
15 |     if True:
   |
   = help: Remove definition: `f`

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:20:5: F841 Local variable `x` is assigned to but never used
   |
18 |         Set = 1
19 | 
20 |     x: Set = set()
   |     ^ F841
21 | 
22 |     x: Set
   |
   = help: Remove assignment to unused variable `x`

Suggested fix:
17 17 |     else:
18 18 |         Set = 1
19 19 | 
20    |-    x: Set = set()
21 20 | 
22 21 |     x: Set
23 22 | 

    Run `ruff check --fix --unsafe-fixes` to apply this unsafe fix.

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:27:15: F821 Undefined name `Set`
   |
26 |     def f():
27 |         print(Set)
   |               ^^^ F821
28 | 
29 |         def Set():
   |

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:27:15: F823 Local variable `Set` referenced before assignment
   |
26 |     def f():
27 |         print(Set)
   |               ^^^ F823
28 | 
29 |         def Set():
   |

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:33:1: E402 Module level import not at top of file
   |
31 |         print(Set)
32 | 
33 | from collections.abc import Set
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
34 | 
35 | def f():
   |

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:35:5: F811 Redefinition of unused `f` from line 13
   |
33 | from collections.abc import Set
34 | 
35 | def f():
   |     ^ F811
36 |     """Test: global symbol renaming."""
37 |     global Set
   |
   = help: Remove definition: `f`

crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI025_1.pyi:42:5: F811 Redefinition of unused `f` from line 35
   |
40 |     print(Set)
41 | 
42 | def f():
   |     ^ F811
43 |     """Test: nonlocal symbol renaming."""
44 |     from collections.abc import Set
   |
   = help: Remove definition: `f`

Found 20 errors.
[*] 10 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

</details>

## Test Plan

This does not introduce *new* tests, but does update the existing tests to account for the new default output. Of particular interest among the many snapshots, see the updated snapshot for `PYI025_1.pyi`][snap], which does some truncation of diffs.

[snap]: https://github.com/astral-sh/ruff/blob/b824fd328a997e06038aaa8627e06303031f270e/crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI025_PYI025_1.pyi.snap


---

_Comment by @github-actions[bot] on 2024-07-31 15:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @chriskrycho on 2024-07-31 16:52_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1558 on 2024-07-31 17:15_

I would remove the newline (so each violation is a singe block) and drop "Suggested fix" (I think the "help" is a sufficient introduction).

---

_@zanieb reviewed on 2024-07-31 17:15_

---

_@zanieb reviewed on 2024-07-31 17:16_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1584 on 2024-07-31 17:16_

It looks like we need to do something special for violations without help messages? We want a dedicated test case for that and a test case for violations without source context.

---

_@zanieb reviewed on 2024-07-31 17:18_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1587 on 2024-07-31 17:18_

Looking at this more holistically, I wonder if this message is necessary since we have a call to action at the end:

> [*] 1 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-07-31 17:18_

---

_@chriskrycho reviewed on 2024-07-31 17:18_

---

_Review comment by @chriskrycho on `crates/ruff/tests/integration_test.rs`:1587 on 2024-07-31 17:18_

I saw that as well, but my intuition was that it is helpful to have both the details and the summary.

---

_@chriskrycho reviewed on 2024-07-31 17:19_

---

_Review comment by @chriskrycho on `crates/ruff/tests/integration_test.rs`:1584 on 2024-07-31 17:19_

Ah, I missed that one. I’ll see about adding a test for those. What’s a scenario where you would end up without source context?

---

_@zanieb reviewed on 2024-07-31 17:20_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1593 on 2024-07-31 17:20_

Given the message below this ("1 hidden fix..."), I wonder if we should be hiding unsafe fixes unless the opt-in is provided. I think this would solve the "how do we display applicability" problem, to an extent?

If so, I think we could figure out what to do with "display-only" fixes later and just _never_ show them to start (to limit the scope of this pull request)

---

_@zanieb reviewed on 2024-07-31 17:21_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1593 on 2024-07-31 17:21_

This would require a bit of futzing to retain the existing coverage in our test suite, maybe just using `--unsafe-fixes` by default in some places?

---

_@chriskrycho reviewed on 2024-07-31 17:22_

---

_Review comment by @chriskrycho on `crates/ruff/tests/integration_test.rs`:1593 on 2024-07-31 17:22_

Hmm, yeah—I can definitely back out those parts of the change if that’s preferable!

---

_@chriskrycho reviewed on 2024-07-31 17:23_

---

_Review comment by @chriskrycho on `crates/ruff/tests/integration_test.rs`:1558 on 2024-07-31 17:23_

Done in 1f07f5f539f3.

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1584 on 2024-07-31 17:24_

Well, for example, there's no source context for this test rule right? I think if the violation is attached to an empty range we elide the source context. This behavior was recently modified in https://github.com/astral-sh/ruff/pull/12304

---

_@zanieb reviewed on 2024-07-31 17:24_

---

_@zanieb reviewed on 2024-07-31 17:29_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1593 on 2024-07-31 17:29_

It feels like the current design (prior to this pull request) _hides_ unsafe fixes and allowing _opt-in_ to their display, at which point we don't need to indicate that they're unsafe? It seems easiest to follow that pattern for now and consider if and how we want to display unsafe fixes by default separately, i.e., I think more discussion in an issue would be prudent.

---

_@chriskrycho reviewed on 2024-07-31 17:36_

---

_Review comment by @chriskrycho on `crates/ruff/tests/integration_test.rs`:1593 on 2024-07-31 17:36_

In terms of handling applicability, we could do that in a couple of ways:

- The `Diff` itself could take a configuration level and its `Display` implementation could use that to decide what to print.
- The caller could decide whether to print based on message applicability at a higher level, keeping it out of the `Diff`.

The latter is a slightly smaller scope and IMO better isolation of concerns; it keeps it properly in the `TextEmitter`, whereas the `Diff` can be concerned entirely with the presentation of a given diff.

If we go that way, we still have the choice whether to keep the applicability message in the diff. I think I am still mildly inclined toward showing that CTA, because of the differences when you *do* include `--unsafe-fixes`; it seems like it would still be nice to know which changes are which.

---

_@zanieb reviewed on 2024-07-31 17:45_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1593 on 2024-07-31 17:45_

It seems reasonable to decide whether or not the `Diff` should be displayed outside its display implementation (the latter option).

Regarding the CTA, I have a preference for splitting it out of this pull request so we can merge this sooner and have focused discussion on each objective, like:

1. Display available fixes by default
2. Disambiguate safe and unsafe fixes when displayed (i.e., via a CTA or other label)
3. Display unsafe fixes by default (needs discussion)
4. Determine how to make display-only fixes user-facing

What do you think?

---

_@chriskrycho reviewed on 2024-07-31 17:56_

---

_Review comment by @chriskrycho on `crates/ruff/tests/integration_test.rs`:1593 on 2024-07-31 17:56_

That makes good sense to me. I will split out issues (Edit: https://github.com/astral-sh/ruff/issues/12598) for further discussion on those points and update this to simply display fixes only for the `Applicability::Safe` level, in a slightly clunky way that is amenable to cleaning up by way of broader refactors once some of those other design questions are fleshed out.

---

_@chriskrycho reviewed on 2024-07-31 18:10_

---

_Review comment by @chriskrycho on `crates/ruff/tests/integration_test.rs`:1584 on 2024-07-31 18:10_

I'm going to open a new issue tracking what to do in each of those cases, with the existing baseline implementation here as a useful starting point.

- For missing help: https://github.com/astral-sh/ruff/issues/12599
- For missing source: https://github.com/astral-sh/ruff/issues/12600

---

_Label `cli` added by @MichaReiser on 2024-07-31 20:03_

---

_Review comment by @MichaReiser on `crates/ruff/src/lib.rs`:290 on 2024-07-31 20:03_

Should this be gated behind preview mode to get some user feedback before enabling it for everyone?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/diff.rs`:47 on 2024-07-31 20:05_

Does this mean that we won't see unsafe fixes in snapshot tests anymore?

I would find this concerning because it means we loose all test coverage for unsafe fixes (of which there are plenty)

---

_@MichaReiser reviewed on 2024-07-31 20:06_

I'm concerned about the fact that this change removes all test coverage for unsafe fixes. I don't think we should land this before recovering the test coverage. 

---

_@zanieb reviewed on 2024-07-31 21:33_

---

_Review comment by @zanieb on `crates/ruff_linter/src/message/diff.rs`:47 on 2024-07-31 21:33_

Yes, we need to address this before merging.

---

_@zanieb reviewed on 2024-07-31 21:33_

---

_Review comment by @zanieb on `crates/ruff/src/lib.rs`:290 on 2024-07-31 21:33_

I don't think it's strictly necessary, but we may want to do so if it's going to change significantly (i.e. by being split into follow-ups)

---

_Assigned to @zanieb by @zanieb on 2024-07-31 21:44_

---

_@chriskrycho reviewed on 2024-08-01 14:02_

---

_Review comment by @chriskrycho on `crates/ruff_linter/src/message/diff.rs`:47 on 2024-08-01 14:02_

Yeah, we realized this late in our time working on it yesterday. It may warrant going ahead and doing the bit I highlighted in #12597, etc. I think this was a case where trying to keep the change minimal actually makes it harder to land—this particularly bit of coupling wasn’t obvious to me until the very end when we changed from including all the CTAs to including *none* of them, and turning off.

I think it probably needs the aforementioned refactor (where the `TextEmitter` handles what to print or not) plus setting the relevant flags based on both `--unsafe-fixes` *and* whether it is testing.

---

_Closed by @chriskrycho on 2024-09-04 21:57_

---
