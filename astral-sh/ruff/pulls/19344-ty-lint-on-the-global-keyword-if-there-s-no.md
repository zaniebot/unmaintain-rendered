```yaml
number: 19344
title: "[ty] lint on the `global` keyword if there's no explicit definition in the…"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: jack/global_must_refer_to_definition
created_at: 2025-07-15T00:09:00Z
updated_at: 2025-07-15T23:57:56Z
url: https://github.com/astral-sh/ruff/pull/19344
synced_at: 2026-01-12T15:56:37Z
```

# [ty] lint on the `global` keyword if there's no explicit definition in the…

---

_@oconnor663_

We currently lint on examples like this (even though it's allowed at runtime, as long as you call `f` before `g`):

```py
def f():
    global x
    x = 5
def g():
    print(x)  # error: [unresolved-reference]: Name `x` used when not defined
```

However, linting only on the `print` is kind of confusing. This PR adds a lint on the `global` statement itself (edit: updated after review feedback):

```
error[unresolved-global]: Invalid global declaration of `x`
 --> test.py:2:5
  |
1 | def f():
2 |     global x
  |     ^^^^^^^^ `x` has no declarations or bindings in the global scope
3 |     x = 42
  |
info: This limits ty's ability to make accurate inferences about the boundness and types of global-scope symbols
info: Consider adding a declaration to the global scope, e.g. `x: int`
info: rule `unresolved-global` is enabled by default
```

Because this is legal Python, I expect this is going to have a lot of new hits in the `ecosystem-analyzer` (and I had to fix some of our mdtests). I don't know what would count as an "excessive" number of new failures, though, and I'll need advice from reviewers :)

---

_Review requested from @carljm by @oconnor663 on 2025-07-15 00:09_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-07-15 00:09_

---

_Review requested from @sharkdp by @oconnor663 on 2025-07-15 00:09_

---

_Label `ty` added by @oconnor663 on 2025-07-15 00:09_

---

_Review requested from @dcreager by @oconnor663 on 2025-07-15 00:09_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-15 00:09_

---

_Comment by @github-actions[bot] on 2025-07-15 00:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
ignite (https://github.com/pytorch/ignite)
+ warning[unresolved-global] tests/ignite/handlers/test_base_logger.py:320:5: Invalid global declaration of `close_counter`: `close_counter` has no declarations or bindings in the global scope
- Found 2125 diagnostics
+ Found 2126 diagnostics

asynq (https://github.com/quora/asynq)
+ warning[unresolved-global] asynq/tests/test_contexts.py:105:5: Invalid global declaration of `expected_change_amount_base`: `expected_change_amount_base` has no declarations or bindings in the global scope
+ warning[unresolved-global] asynq/tests/test_contexts.py:111:9: Invalid global declaration of `expected_change_amount_base`: `expected_change_amount_base` has no declarations or bindings in the global scope
- Found 186 diagnostics
+ Found 188 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
+ warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:52:5: Invalid global declaration of `calendarParentModule`: `calendarParentModule` has no declarations or bindings in the global scope
+ warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:119:5: Invalid global declaration of `videoControlModule`: `videoControlModule` has no declarations or bindings in the global scope
+ warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:119:5: Invalid global declaration of `videoControlFileName`: `videoControlFileName` has no declarations or bindings in the global scope
+ warning[unresolved-global] Pythonwin/pywin/test/test_pywin.py:57:9: Invalid global declaration of `teared_down`: `teared_down` has no declarations or bindings in the global scope
+ warning[unresolved-global] win32/Lib/win32serviceutil.py:600:5: Invalid global declaration of `g_debugService`: `g_debugService` has no declarations or bindings in the global scope
- Found 2008 diagnostics
+ Found 2013 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ warning[unresolved-global] test/helper_tools/memoryleak.py:32:5: Invalid global declaration of `ssl`: `ssl` has no declarations or bindings in the global scope
- Found 1796 diagnostics
+ Found 1797 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
+ warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:329:13: Invalid global declaration of `asked`: `asked` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:332:17: Invalid global declaration of `asked`: `asked` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:357:13: Invalid global declaration of `mgfcalls`: `mgfcalls` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:361:17: Invalid global declaration of `mgfcalls`: `mgfcalls` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:9: Invalid global declaration of `DSA`: `DSA` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:9: Invalid global declaration of `Random`: `Random` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:9: Invalid global declaration of `bytes_to_long`: `bytes_to_long` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:9: Invalid global declaration of `size`: `size` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:9: Invalid global declaration of `RSA`: `RSA` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:9: Invalid global declaration of `Random`: `Random` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:9: Invalid global declaration of `bytes_to_long`: `bytes_to_long` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Util/test_Counter.py:33:9: Invalid global declaration of `Counter`: `Counter` has no declarations or bindings in the global scope
- Found 1476 diagnostics
+ Found 1488 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ warning[unresolved-global] openlibrary/plugins/openlibrary/status.py:107:5: Invalid global declaration of `feature_flags`: `feature_flags` has no declarations or bindings in the global scope
- Found 679 diagnostics
+ Found 680 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ warning[unresolved-global] ddtrace/vendor/ply/lex.py:868:5: Invalid global declaration of `lexer`: `lexer` has no declarations or bindings in the global scope
+ warning[unresolved-global] ddtrace/vendor/ply/lex.py:874:5: Invalid global declaration of `token`: `token` has no declarations or bindings in the global scope
+ warning[unresolved-global] ddtrace/vendor/ply/lex.py:874:5: Invalid global declaration of `input`: `input` has no declarations or bindings in the global scope
+ warning[unresolved-global] ddtrace/vendor/ply/yacc.py:3224:5: Invalid global declaration of `parse`: `parse` has no declarations or bindings in the global scope
+ warning[unresolved-global] tests/internal/test_forksafe.py:351:9: Invalid global declaration of `service`: `service` has no declarations or bindings in the global scope
- Found 6822 diagnostics
+ Found 6827 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ warning[unresolved-global] misc/python/materialize/scalability/executor/benchmark_executor.py:203:9: Invalid global declaration of `next_worker_id`: `next_worker_id` has no declarations or bindings in the global scope
+ warning[unresolved-global] misc/python/materialize/scalability/executor/benchmark_executor.py:300:9: Invalid global declaration of `next_worker_id`: `next_worker_id` has no declarations or bindings in the global scope
- Found 3228 diagnostics
+ Found 3230 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ warning[unresolved-global] sklearn/linear_model/tests/test_ransac.py:223:5: Invalid global declaration of `cause_skip`: `cause_skip` has no declarations or bindings in the global scope
+ warning[unresolved-global] sklearn/linear_model/tests/test_ransac.py:227:9: Invalid global declaration of `cause_skip`: `cause_skip` has no declarations or bindings in the global scope
- Found 2059 diagnostics
+ Found 2061 diagnostics

scipy (https://github.com/scipy/scipy)
+ warning[unresolved-global] scipy/io/tests/test_mmio.py:29:5: Invalid global declaration of `mminfo`: `mminfo` has no declarations or bindings in the global scope
+ warning[unresolved-global] scipy/io/tests/test_mmio.py:30:5: Invalid global declaration of `mmread`: `mmread` has no declarations or bindings in the global scope
+ warning[unresolved-global] scipy/io/tests/test_mmio.py:31:5: Invalid global declaration of `mmwrite`: `mmwrite` has no declarations or bindings in the global scope
- Found 6649 diagnostics
+ Found 6652 diagnostics

sympy (https://github.com/sympy/sympy)
+ warning[unresolved-global] sympy/ntheory/partitions_.py:13:5: Invalid global declaration of `_factor`: `_factor` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/ntheory/partitions_.py:13:5: Invalid global declaration of `_totient`: `_totient` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/testing/runtests.py:2233:9: Invalid global declaration of `text`: `text` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/testing/runtests.py:2233:9: Invalid global declaration of `linelen`: `linelen` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/testing/runtests.py:2238:13: Invalid global declaration of `text`: `text` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/testing/runtests.py:2238:13: Invalid global declaration of `linelen`: `linelen` has no declarations or bindings in the global scope
- Found 13502 diagnostics
+ Found 13508 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:4702 on 2025-07-15 10:50_

Could we give a better error message here than "used when not defined"? Currently the diagnostic looks like this:

```
error[unresolved-reference]: Name `x` used when not defined
 --> foo.py:2:5
  |
1 | def f():
2 |     global x
  |     ^^^^^^^^
  |
info: rule `unresolved-reference` is enabled by default
```

But I think I'd ideally prefer something like this (a dedicated rule with a clear error message and explanatory subdiagnostics for why we don't permit this):

```
error[invalid-global-declaration]: Invalid global declaration of `x`
 --> foo.py:2:5
  |
1 | def f():
2 |     global x
  |     ^^^^^^^^  `x` has no declarations or bindings in the global scope
  |
info: This limits ty's ability to make accurate inferences about the boundness and types of global-scope symbols
info: Consider adding a declaration to the global scope, e.g. `x: int`
info: rule `unresolved-reference` is enabled by default
```

The dedicated diagnostic should probably have `Severity::WARN`, since this coding pattern does work at runtime, it just limits ty's ability to make accurate inferences elsewhere

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:4693 on 2025-07-15 10:53_

I'm not sure we should exclude builtins from this check: declaring a symbol as `global` in an inner scope and assigning to it from that inner scope will insert a symbol into the global scope that will shadow the symbol from the builtin scope, greatly complicating ty's ability to accurately infer boundness and types of symbols in the global scope. Implicit globals _should_ be excluded from this check, though, as they _are_ defined directly in the module's global scope.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/star.md`:1305 on 2025-07-15 10:56_

Yeah, I would remove this TODO, the one a few lines below it in `b.py`, and rework the prose above `a.py` to explain that we forbid the `global` statement in an inner scope if it refers to a symbol not declared or bound in the global scope, and that this is why we don't consider such symbols imported as a result of a `*` import in another module :-)

---

_@AlexWaygood reviewed on 2025-07-15 10:58_

Nice! This looks pretty good to me. I haven't gone through the mypy_primer hits, but it doesn't look prohibitively large to me.

The main thing I think we need to do before landing this is to improve the diagnostic to make it clearer what the problem is, and why we object to this coding pattern

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-07-15 14:58_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-15 14:58_

---

_Comment by @github-actions[bot] on 2025-07-15 15:06_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-reference` | 39 | 0 | 0 |
| **Total** | **39** | **0** | **0** |

**[Full report with detailed diff](https://jack-global-must-refer-to-de.ecosystem-663.pages.dev/diff)**


---

_Review requested from @MichaReiser by @oconnor663 on 2025-07-15 16:49_

---

_@oconnor663 reviewed on 2025-07-15 16:50_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer.rs`:4702 on 2025-07-15 16:50_

291f1047d9bddccb6ada5df6c59bb599a8c8e4bd

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer.rs`:4693 on 2025-07-15 16:54_

b582cf65a0dabdc5fff291bc52767f6d31e73caf

---

_@oconnor663 reviewed on 2025-07-15 16:54_

---

_@AlexWaygood reviewed on 2025-07-15 17:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1585 on 2025-07-15 17:04_

since this is more about "this screws up the accuracy of our analysis elsewhere" rather than "this will raise an exception at runtime", I think `Warn` is a more appropriate default severity here:

```suggestion
        default_level: Level::Warn,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:4701 on 2025-07-15 17:05_

```suggestion
            // This variable isn't explicitly defined in the global scope, nor is it an
            // implicit global from `types.ModuleType`, so we consider this `global` statement invalid.
```

---

_@AlexWaygood reviewed on 2025-07-15 17:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:4677 on 2025-07-15 17:06_

```suggestion
        // However, allowing this pattern would make it hard for us to guarantee
        // accurate analysis about the types and boundness of global-scope symbols,
        // so we require the variable to be explicitly defined (either bound or declared)
        // in the global scope.
```

---

_@AlexWaygood reviewed on 2025-07-15 17:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1566 on 2025-07-15 17:06_

```suggestion
    /// Detects variables declared as `global` in an inner scope that have no explicit
    /// bindings or declarations in the global scope.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1571 on 2025-07-15 17:07_

```suggestion
    /// it hard for static analysis tools to infer the types of globals without
    /// explicit definitions or declarations.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1582 on 2025-07-15 17:12_

```suggestion
    /// ```
    ///
    /// Use instead:
    /// ```python
    /// x: int
    ///
    /// def f():
    ///     global x
    ///     x = 42
    ///
    /// def g():
    ///     print(x)
    /// ```
    ///
    /// Or:
    /// ```python
    /// x: int | None = None
    ///
    /// def f():
    ///     global x
    ///     x = 42
    ///
    /// def g():
    ///     print(x)
    /// ```
```

---

_@AlexWaygood approved on 2025-07-15 17:13_

Great work! (Haven't reviewed the mypy_primer hits other than the quantity -- I'm trusting you to check that they're all as expected!)

---

_@oconnor663 reviewed on 2025-07-15 18:18_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/import/star.md`:1305 on 2025-07-15 18:18_

7f2145e920f3dafcdd6691100cb28ceb32a5e703

---

_Comment by @oconnor663 on 2025-07-15 18:36_

Most of the new lints in the ecosystem are as expected, `global` statements that are creating a new global variable. But there's another case that shows up a couple times, [like this](https://github.com/Legrandin/pycryptodome/blob/2c3a8905a7929335a9b2763e18d6e9ed516b8a38/lib/Crypto/SelfTest/Util/test_Counter.py#L31-L34):

```python
class CounterTests(unittest.TestCase):
    def setUp(self):
        global Counter
        from Crypto.Util import Counter
```

So that's doing a global import, but not in the global scope. If the user is sure they want to do this sort of thing, is there an alternative way for them to do it that will type-check? (Maybe something better than `Counter: Any`, since that might be bad for completions? Not sure.)

---

_Comment by @oconnor663 on 2025-07-15 23:56_

Ok, folks who want to do `global ... import ...` tricks can at least put an `Any` declaration in the global scope and not be worse off than before. That was the only surprising case (and any other surprises we run into down the road can probably use the same workaround?), so I'm going to go ahead and land this.

---

_Merged by @oconnor663 on 2025-07-15 23:56_

---

_Closed by @oconnor663 on 2025-07-15 23:56_

---

_Branch deleted on 2025-07-15 23:56_

---
