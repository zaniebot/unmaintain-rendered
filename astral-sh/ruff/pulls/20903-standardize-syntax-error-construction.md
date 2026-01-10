```yaml
number: 20903
title: Standardize syntax error construction
type: pull_request
state: merged
author: ntBre
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: brent/standardize-syntax-error-message
created_at: 2025-10-15T18:30:46Z
updated_at: 2025-10-16T15:56:34Z
url: https://github.com/astral-sh/ruff/pull/20903
synced_at: 2026-01-10T17:34:34Z
```

# Standardize syntax error construction

---

_Pull request opened by @ntBre on 2025-10-15 18:30_

Summary
--

This PR unifies the two different ways Ruff and ty construct syntax errors. Ruff has been storing the primary message in the diagnostic itself, while ty attached the message to the primary annotation:

```
> ruff check try.py
invalid-syntax: name capture `x` makes remaining patterns unreachable
 --> try.py:2:10
  |
1 | match 42:
2 |     case x: ...
  |          ^
3 |     case y: ...
  |

Found 1 error.
> uvx ty check try.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                 
error[invalid-syntax]
 --> try.py:2:10
  |
1 | match 42:
2 |     case x: ...
  |          ^ name capture `x` makes remaining patterns unreachable
3 |     case y: ...
  |

Found 1 diagnostic
```

I think there are benefits to both approaches, and I do like ty's version, but I feel like we should pick one (and it might help with #20901 eventually). I slightly prefer Ruff's version, so I went with that. Hopefully this isn't too controversial, but I'm happy to close this if it is.

Note that this shouldn't change any other diagnostic formats in ty because [`Diagnostic::primary_message`](https://github.com/astral-sh/ruff/blob/98d27c412810e157f8a65ea75726d66676628225/crates/ruff_db/src/diagnostic/mod.rs#L177) was already falling back to the primary annotation message if the diagnostic message was empty. As a result, I think this change will partially resolve the FIXME therein.

Test Plan
--

Existing tests with updated snapshots


---

_Label `ty` added by @ntBre on 2025-10-15 18:30_

---

_Label `diagnostics` added by @ntBre on 2025-10-15 18:30_

---

_Comment by @github-actions[bot] on 2025-10-15 18:32_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-15 18:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-15 18:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-10-15 18:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fstandardize-syntax-error-message)

### Merging #20903 will **improve performances by 6.51%**

<sub>Comparing <code>brent/standardize-syntax-error-message</code> (d2e8d76) with <code>main</code> (9de34e7)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>main</code> (fd568f0) during the generation of this report, so 9de34e7 was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`⚡ 1` improvement  
`✅ 50` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/brent%2Fstandardize-syntax-error-message?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime) | 11.2 s | 10.5 s | +6.51% |


---

_Marked ready for review by @ntBre on 2025-10-15 18:51_

---

_Review requested from @carljm by @ntBre on 2025-10-15 18:51_

---

_Review requested from @AlexWaygood by @ntBre on 2025-10-15 18:51_

---

_Review requested from @sharkdp by @ntBre on 2025-10-15 18:51_

---

_Review requested from @dcreager by @ntBre on 2025-10-15 18:51_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-15 18:51_

---

_Comment by @AlexWaygood on 2025-10-15 18:53_

> Merging #20903 will **improve performances by 6.51%**

yeah, that's just a merge race with https://github.com/astral-sh/ruff/pull/20377, judging by the footnote that Codspeed put at the bottom of that comment 😆

---

_Comment by @ntBre on 2025-10-15 18:55_

Yes good call, I don't think this should affect performance :laughing: 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/semantic_syntax_erro…_-_Semantic_syntax_erro…_-_`async`_comprehensio…_-_Python_3.10_(96aa8ec77d46553d).snap`:44 on 2025-10-15 18:55_

Something like this might be nice here (but not blocking):

```
error[invalid-syntax]: cannot use an asynchronous comprehension inside of a synchronous comprehension on Python 3.10
 --> src/mdtest_snippet.py:6:19
  |
4 | async def f():
5 |     # error: 19 [invalid-syntax] "cannot use an asynchronous comprehension inside of a synchronous comprehension on Python 3.10 (syntax…
6 |     return {n: [x async for x in elements(n)] for n in range(3)}
  |                   ^^^^^^^^^^^^^^^^^^^^^^^^^^ Syntax was added in 3.11
7 | async def test():
8 |     # error: [not-iterable] "Object of type `range` is not async-iterable"
```

---

_@AlexWaygood approved on 2025-10-15 18:55_

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/snapshots/semantic_syntax_erro…_-_Semantic_syntax_erro…_-_`async`_comprehensio…_-_Python_3.10_(96aa8ec77d46553d).snap`:44 on 2025-10-15 18:58_

I was thinking about that too, or an info sub-diagnostic. I think either option will need something like https://github.com/astral-sh/ruff/issues/20901, though.

---

_@ntBre reviewed on 2025-10-15 18:58_

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/snapshots/semantic_syntax_erro…_-_Semantic_syntax_erro…_-_`async`_comprehensio…_-_Python_3.10_(96aa8ec77d46553d).snap`:44 on 2025-10-15 18:59_

(or just a separate constructor from the very generic `invalid_syntax` for unsupported syntax errors)

---

_@ntBre reviewed on 2025-10-15 18:59_

---

_@MichaReiser approved on 2025-10-15 19:18_

---

_Merged by @ntBre on 2025-10-16 15:56_

---

_Closed by @ntBre on 2025-10-16 15:56_

---

_Branch deleted on 2025-10-16 15:56_

---
