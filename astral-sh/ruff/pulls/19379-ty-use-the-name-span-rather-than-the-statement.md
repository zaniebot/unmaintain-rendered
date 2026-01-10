```yaml
number: 19379
title: "[ty] use the name span rather than the statement span for unresolved `global` lints"
type: pull_request
state: merged
author: oconnor663
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: jack/warning_name_span
created_at: 2025-07-16T00:14:39Z
updated_at: 2025-07-16T15:35:19Z
url: https://github.com/astral-sh/ruff/pull/19379
synced_at: 2026-01-10T17:58:13Z
```

# [ty] use the name span rather than the statement span for unresolved `global` lints

---

_Pull request opened by @oconnor663 on 2025-07-16 00:14_

This is a follow-up to https://github.com/astral-sh/ruff/pull/19344 that improves the error formatting slightly. For example with this program:

```py
def f():
    global foo, bar
```

Before we printed:

```
1 | def f():
2 |     global foo, bar
  |     ^^^^^^^^^^^^^^^ `foo` has no declarations or bindings in the global scope
...
1 | def f():
2 |     global foo, bar
  |     ^^^^^^^^^^^^^^^ `bar` has no declarations or bindings in the global scope
```

Now we print:

```
1 | def f():
2 |     global foo, bar
  |            ^^^ `foo` has no declarations or bindings in the global scope
...
1 | def f():
2 |     global foo, bar
  |                 ^^^ `bar` has no declarations or bindings in the global scope
```

---

_Review requested from @carljm by @oconnor663 on 2025-07-16 00:14_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-07-16 00:14_

---

_Review requested from @sharkdp by @oconnor663 on 2025-07-16 00:14_

---

_Review requested from @dcreager by @oconnor663 on 2025-07-16 00:14_

---

_Label `ty` added by @oconnor663 on 2025-07-16 00:14_

---

_Comment by @github-actions[bot] on 2025-07-16 00:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
ignite (https://github.com/pytorch/ignite)
- warning[unresolved-global] tests/ignite/handlers/test_base_logger.py:320:5: Invalid global declaration of `close_counter`: `close_counter` has no declarations or bindings in the global scope
+ warning[unresolved-global] tests/ignite/handlers/test_base_logger.py:320:12: Invalid global declaration of `close_counter`: `close_counter` has no declarations or bindings in the global scope

asynq (https://github.com/quora/asynq)
- warning[unresolved-global] asynq/tests/test_contexts.py:105:5: Invalid global declaration of `expected_change_amount_base`: `expected_change_amount_base` has no declarations or bindings in the global scope
+ warning[unresolved-global] asynq/tests/test_contexts.py:105:12: Invalid global declaration of `expected_change_amount_base`: `expected_change_amount_base` has no declarations or bindings in the global scope
- warning[unresolved-global] asynq/tests/test_contexts.py:111:9: Invalid global declaration of `expected_change_amount_base`: `expected_change_amount_base` has no declarations or bindings in the global scope
+ warning[unresolved-global] asynq/tests/test_contexts.py:111:16: Invalid global declaration of `expected_change_amount_base`: `expected_change_amount_base` has no declarations or bindings in the global scope

pywin32 (https://github.com/mhammond/pywin32)
- warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:52:5: Invalid global declaration of `calendarParentModule`: `calendarParentModule` has no declarations or bindings in the global scope
+ warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:52:12: Invalid global declaration of `calendarParentModule`: `calendarParentModule` has no declarations or bindings in the global scope
- warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:119:5: Invalid global declaration of `videoControlModule`: `videoControlModule` has no declarations or bindings in the global scope
+ warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:119:12: Invalid global declaration of `videoControlModule`: `videoControlModule` has no declarations or bindings in the global scope
- warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:119:5: Invalid global declaration of `videoControlFileName`: `videoControlFileName` has no declarations or bindings in the global scope
+ warning[unresolved-global] Pythonwin/pywin/Demos/ocx/ocxtest.py:119:32: Invalid global declaration of `videoControlFileName`: `videoControlFileName` has no declarations or bindings in the global scope
- warning[unresolved-global] Pythonwin/pywin/test/test_pywin.py:57:9: Invalid global declaration of `teared_down`: `teared_down` has no declarations or bindings in the global scope
+ warning[unresolved-global] Pythonwin/pywin/test/test_pywin.py:57:16: Invalid global declaration of `teared_down`: `teared_down` has no declarations or bindings in the global scope
- warning[unresolved-global] win32/Lib/win32serviceutil.py:600:5: Invalid global declaration of `g_debugService`: `g_debugService` has no declarations or bindings in the global scope
+ warning[unresolved-global] win32/Lib/win32serviceutil.py:600:12: Invalid global declaration of `g_debugService`: `g_debugService` has no declarations or bindings in the global scope

pycryptodome (https://github.com/Legrandin/pycryptodome)
- warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:329:13: Invalid global declaration of `asked`: `asked` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:329:20: Invalid global declaration of `asked`: `asked` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:332:17: Invalid global declaration of `asked`: `asked` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:332:24: Invalid global declaration of `asked`: `asked` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:357:13: Invalid global declaration of `mgfcalls`: `mgfcalls` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:357:20: Invalid global declaration of `mgfcalls`: `mgfcalls` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:361:17: Invalid global declaration of `mgfcalls`: `mgfcalls` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:361:24: Invalid global declaration of `mgfcalls`: `mgfcalls` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:9: Invalid global declaration of `DSA`: `DSA` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:16: Invalid global declaration of `DSA`: `DSA` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:9: Invalid global declaration of `Random`: `Random` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:21: Invalid global declaration of `Random`: `Random` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:9: Invalid global declaration of `bytes_to_long`: `bytes_to_long` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:29: Invalid global declaration of `bytes_to_long`: `bytes_to_long` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:9: Invalid global declaration of `size`: `size` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:44: Invalid global declaration of `size`: `size` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:9: Invalid global declaration of `RSA`: `RSA` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:16: Invalid global declaration of `RSA`: `RSA` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:9: Invalid global declaration of `Random`: `Random` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:21: Invalid global declaration of `Random`: `Random` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:9: Invalid global declaration of `bytes_to_long`: `bytes_to_long` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:29: Invalid global declaration of `bytes_to_long`: `bytes_to_long` has no declarations or bindings in the global scope
- warning[unresolved-global] lib/Crypto/SelfTest/Util/test_Counter.py:33:9: Invalid global declaration of `Counter`: `Counter` has no declarations or bindings in the global scope
+ warning[unresolved-global] lib/Crypto/SelfTest/Util/test_Counter.py:33:16: Invalid global declaration of `Counter`: `Counter` has no declarations or bindings in the global scope

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- warning[unresolved-global] test/helper_tools/memoryleak.py:32:5: Invalid global declaration of `ssl`: `ssl` has no declarations or bindings in the global scope
+ warning[unresolved-global] test/helper_tools/memoryleak.py:32:18: Invalid global declaration of `ssl`: `ssl` has no declarations or bindings in the global scope

openlibrary (https://github.com/internetarchive/openlibrary)
- warning[unresolved-global] openlibrary/plugins/openlibrary/status.py:107:5: Invalid global declaration of `feature_flags`: `feature_flags` has no declarations or bindings in the global scope
+ warning[unresolved-global] openlibrary/plugins/openlibrary/status.py:107:25: Invalid global declaration of `feature_flags`: `feature_flags` has no declarations or bindings in the global scope

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- warning[unresolved-global] sklearn/linear_model/tests/test_ransac.py:223:5: Invalid global declaration of `cause_skip`: `cause_skip` has no declarations or bindings in the global scope
+ warning[unresolved-global] sklearn/linear_model/tests/test_ransac.py:223:12: Invalid global declaration of `cause_skip`: `cause_skip` has no declarations or bindings in the global scope
- warning[unresolved-global] sklearn/linear_model/tests/test_ransac.py:227:9: Invalid global declaration of `cause_skip`: `cause_skip` has no declarations or bindings in the global scope
+ warning[unresolved-global] sklearn/linear_model/tests/test_ransac.py:227:16: Invalid global declaration of `cause_skip`: `cause_skip` has no declarations or bindings in the global scope

materialize (https://github.com/MaterializeInc/materialize)
- warning[unresolved-global] misc/python/materialize/scalability/executor/benchmark_executor.py:203:9: Invalid global declaration of `next_worker_id`: `next_worker_id` has no declarations or bindings in the global scope
+ warning[unresolved-global] misc/python/materialize/scalability/executor/benchmark_executor.py:203:16: Invalid global declaration of `next_worker_id`: `next_worker_id` has no declarations or bindings in the global scope
- warning[unresolved-global] misc/python/materialize/scalability/executor/benchmark_executor.py:300:9: Invalid global declaration of `next_worker_id`: `next_worker_id` has no declarations or bindings in the global scope
+ warning[unresolved-global] misc/python/materialize/scalability/executor/benchmark_executor.py:300:16: Invalid global declaration of `next_worker_id`: `next_worker_id` has no declarations or bindings in the global scope

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- warning[unresolved-global] ddtrace/vendor/ply/lex.py:868:5: Invalid global declaration of `lexer`: `lexer` has no declarations or bindings in the global scope
+ warning[unresolved-global] ddtrace/vendor/ply/lex.py:868:12: Invalid global declaration of `lexer`: `lexer` has no declarations or bindings in the global scope
- warning[unresolved-global] ddtrace/vendor/ply/lex.py:874:5: Invalid global declaration of `token`: `token` has no declarations or bindings in the global scope
+ warning[unresolved-global] ddtrace/vendor/ply/lex.py:874:12: Invalid global declaration of `token`: `token` has no declarations or bindings in the global scope
- warning[unresolved-global] ddtrace/vendor/ply/lex.py:874:5: Invalid global declaration of `input`: `input` has no declarations or bindings in the global scope
+ warning[unresolved-global] ddtrace/vendor/ply/lex.py:874:19: Invalid global declaration of `input`: `input` has no declarations or bindings in the global scope
- warning[unresolved-global] ddtrace/vendor/ply/yacc.py:3224:5: Invalid global declaration of `parse`: `parse` has no declarations or bindings in the global scope
+ warning[unresolved-global] ddtrace/vendor/ply/yacc.py:3224:12: Invalid global declaration of `parse`: `parse` has no declarations or bindings in the global scope
- warning[unresolved-global] tests/internal/test_forksafe.py:351:9: Invalid global declaration of `service`: `service` has no declarations or bindings in the global scope
+ warning[unresolved-global] tests/internal/test_forksafe.py:351:16: Invalid global declaration of `service`: `service` has no declarations or bindings in the global scope
- warning[unresolved-global] tests/internal/test_settings.py:654:9: Invalid global declaration of `call_count`: `call_count` has no declarations or bindings in the global scope
+ warning[unresolved-global] tests/internal/test_settings.py:654:16: Invalid global declaration of `call_count`: `call_count` has no declarations or bindings in the global scope

scipy (https://github.com/scipy/scipy)
- warning[unresolved-global] scipy/io/tests/test_mmio.py:29:5: Invalid global declaration of `mminfo`: `mminfo` has no declarations or bindings in the global scope
+ warning[unresolved-global] scipy/io/tests/test_mmio.py:29:12: Invalid global declaration of `mminfo`: `mminfo` has no declarations or bindings in the global scope
- warning[unresolved-global] scipy/io/tests/test_mmio.py:30:5: Invalid global declaration of `mmread`: `mmread` has no declarations or bindings in the global scope
+ warning[unresolved-global] scipy/io/tests/test_mmio.py:30:12: Invalid global declaration of `mmread`: `mmread` has no declarations or bindings in the global scope
- warning[unresolved-global] scipy/io/tests/test_mmio.py:31:5: Invalid global declaration of `mmwrite`: `mmwrite` has no declarations or bindings in the global scope
+ warning[unresolved-global] scipy/io/tests/test_mmio.py:31:12: Invalid global declaration of `mmwrite`: `mmwrite` has no declarations or bindings in the global scope

sympy (https://github.com/sympy/sympy)
- warning[unresolved-global] sympy/ntheory/partitions_.py:13:5: Invalid global declaration of `_factor`: `_factor` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/ntheory/partitions_.py:13:12: Invalid global declaration of `_factor`: `_factor` has no declarations or bindings in the global scope
- warning[unresolved-global] sympy/ntheory/partitions_.py:13:5: Invalid global declaration of `_totient`: `_totient` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/ntheory/partitions_.py:13:21: Invalid global declaration of `_totient`: `_totient` has no declarations or bindings in the global scope
- warning[unresolved-global] sympy/testing/runtests.py:2233:9: Invalid global declaration of `text`: `text` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/testing/runtests.py:2233:16: Invalid global declaration of `text`: `text` has no declarations or bindings in the global scope
- warning[unresolved-global] sympy/testing/runtests.py:2233:9: Invalid global declaration of `linelen`: `linelen` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/testing/runtests.py:2233:22: Invalid global declaration of `linelen`: `linelen` has no declarations or bindings in the global scope
- warning[unresolved-global] sympy/testing/runtests.py:2238:13: Invalid global declaration of `text`: `text` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/testing/runtests.py:2238:20: Invalid global declaration of `text`: `text` has no declarations or bindings in the global scope
- warning[unresolved-global] sympy/testing/runtests.py:2238:13: Invalid global declaration of `linelen`: `linelen` has no declarations or bindings in the global scope
+ warning[unresolved-global] sympy/testing/runtests.py:2238:26: Invalid global declaration of `linelen`: `linelen` has no declarations or bindings in the global scope

```
</details>
No memory usage changes detected ✅


---

_Comment by @oconnor663 on 2025-07-16 00:41_

(`mypy_primer` changes are just formatting, no new/removed lints)

---

_Label `internal` added by @AlexWaygood on 2025-07-16 06:04_

---

_@AlexWaygood approved on 2025-07-16 06:05_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:4697 on 2025-07-16 06:11_

```suggestion
            let Some(builder) = self.context.report_lint(&UNRESOLVED_GLOBAL, name) else {
```

---

_@MichaReiser approved on 2025-07-16 06:12_

Nice

---

_Label `internal` removed by @MichaReiser on 2025-07-16 06:12_

---

_Label `diagnostics` added by @MichaReiser on 2025-07-16 06:12_

---

_Comment by @AlexWaygood on 2025-07-16 06:15_

(Sorry, I should have said this! I added `internal` because the diagnostic being modified here hasn't appeared in any released version of ty, so I didn't think this PR needed a changelog entry of its own)

---

_Label `diagnostics` removed by @MichaReiser on 2025-07-16 06:56_

---

_Label `internal` added by @MichaReiser on 2025-07-16 06:56_

---

_Merged by @oconnor663 on 2025-07-16 15:34_

---

_Closed by @oconnor663 on 2025-07-16 15:34_

---

_Branch deleted on 2025-07-16 15:34_

---
