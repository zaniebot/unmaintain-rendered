```yaml
number: 19821
title: "[ty] now that inference sees nested bindings, allow unresolved globals"
type: pull_request
state: closed
author: oconnor663
labels:
  - ty
assignees: []
base: jack/semantic-index-nested-bindings
head: jack/allow-unresolved-globals
created_at: 2025-08-08T03:44:43Z
updated_at: 2025-08-08T21:44:54Z
url: https://github.com/astral-sh/ruff/pull/19821
synced_at: 2026-01-12T15:56:48Z
```

# [ty] now that inference sees nested bindings, allow unresolved globals

---

_@oconnor663_

This is an optional follow-up PR on top of https://github.com/astral-sh/ruff/pull/19820. Now that we track all bindings from nested scopes, it could be practical to allow `global` variables without an explicit definition in the module scope. For example:

```py
def f():
    global x
    x = 1

def g():
    print(x)
```

Before:

```
warning[unresolved-global]: Invalid global declaration of `x`
 --> test.py:2:12
  |
1 | def f():
2 |     global x
  |            ^ `x` has no declarations or bindings in the global scope
3 |     x = 1
  |
info: This limits ty's ability to make accurate inferences about the boundness and types of global-scope symbols
info: Consider adding a declaration to the global scope, e.g. `x: int`
info: rule `unresolved-global` is enabled by default

error[unresolved-reference]: Name `x` used when not defined
 --> test.py:6:11
  |
5 | def g():
6 |     print(x)
  |           ^
  |
info: rule `unresolved-reference` is enabled by default
```

After:

```
All checks passed!
```

---

_Review requested from @carljm by @oconnor663 on 2025-08-08 03:44_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-08-08 03:44_

---

_Review requested from @sharkdp by @oconnor663 on 2025-08-08 03:44_

---

_Review requested from @dcreager by @oconnor663 on 2025-08-08 03:44_

---

_Comment by @github-actions[bot] on 2025-08-08 03:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 03:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
ignite (https://github.com/pytorch/ignite)
- tests/ignite/handlers/test_base_logger.py:320:12: warning[unresolved-global] Invalid global declaration of `close_counter`: `close_counter` has no declarations or bindings in the global scope
- Found 2107 diagnostics
+ Found 2106 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/tests/test_contexts.py:104:12: warning[unresolved-global] Invalid global declaration of `expected_change_amount_base`: `expected_change_amount_base` has no declarations or bindings in the global scope
- asynq/tests/test_contexts.py:110:16: warning[unresolved-global] Invalid global declaration of `expected_change_amount_base`: `expected_change_amount_base` has no declarations or bindings in the global scope
- Found 179 diagnostics
+ Found 177 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- Pythonwin/pywin/Demos/ocx/ocxtest.py:52:12: warning[unresolved-global] Invalid global declaration of `calendarParentModule`: `calendarParentModule` has no declarations or bindings in the global scope
- Pythonwin/pywin/Demos/ocx/ocxtest.py:119:12: warning[unresolved-global] Invalid global declaration of `videoControlModule`: `videoControlModule` has no declarations or bindings in the global scope
- Pythonwin/pywin/Demos/ocx/ocxtest.py:119:32: warning[unresolved-global] Invalid global declaration of `videoControlFileName`: `videoControlFileName` has no declarations or bindings in the global scope
- Pythonwin/pywin/test/test_pywin.py:57:16: warning[unresolved-global] Invalid global declaration of `teared_down`: `teared_down` has no declarations or bindings in the global scope
- win32/Lib/win32serviceutil.py:600:12: warning[unresolved-global] Invalid global declaration of `g_debugService`: `g_debugService` has no declarations or bindings in the global scope
- Found 2002 diagnostics
+ Found 1997 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:329:20: warning[unresolved-global] Invalid global declaration of `asked`: `asked` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:332:24: warning[unresolved-global] Invalid global declaration of `asked`: `asked` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:357:20: warning[unresolved-global] Invalid global declaration of `mgfcalls`: `mgfcalls` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:361:24: warning[unresolved-global] Invalid global declaration of `mgfcalls`: `mgfcalls` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:16: warning[unresolved-global] Invalid global declaration of `DSA`: `DSA` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:21: warning[unresolved-global] Invalid global declaration of `Random`: `Random` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:29: warning[unresolved-global] Invalid global declaration of `bytes_to_long`: `bytes_to_long` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:71:44: warning[unresolved-global] Invalid global declaration of `size`: `size` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:16: warning[unresolved-global] Invalid global declaration of `RSA`: `RSA` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:21: warning[unresolved-global] Invalid global declaration of `Random`: `Random` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:90:29: warning[unresolved-global] Invalid global declaration of `bytes_to_long`: `bytes_to_long` has no declarations or bindings in the global scope
- lib/Crypto/SelfTest/Util/test_Counter.py:33:16: warning[unresolved-global] Invalid global declaration of `Counter`: `Counter` has no declarations or bindings in the global scope
- Found 1448 diagnostics
+ Found 1436 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/openlibrary/status.py:117:25: warning[unresolved-global] Invalid global declaration of `feature_flags`: `feature_flags` has no declarations or bindings in the global scope
- Found 700 diagnostics
+ Found 699 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/helper_tools/memoryleak.py:32:18: warning[unresolved-global] Invalid global declaration of `ssl`: `ssl` has no declarations or bindings in the global scope
- Found 1820 diagnostics
+ Found 1819 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- misc/python/materialize/scalability/executor/benchmark_executor.py:203:16: warning[unresolved-global] Invalid global declaration of `next_worker_id`: `next_worker_id` has no declarations or bindings in the global scope
- misc/python/materialize/scalability/executor/benchmark_executor.py:300:16: warning[unresolved-global] Invalid global declaration of `next_worker_id`: `next_worker_id` has no declarations or bindings in the global scope
- Found 3257 diagnostics
+ Found 3255 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/vendor/ply/lex.py:868:12: warning[unresolved-global] Invalid global declaration of `lexer`: `lexer` has no declarations or bindings in the global scope
- ddtrace/vendor/ply/lex.py:874:12: warning[unresolved-global] Invalid global declaration of `token`: `token` has no declarations or bindings in the global scope
- ddtrace/vendor/ply/lex.py:874:19: warning[unresolved-global] Invalid global declaration of `input`: `input` has no declarations or bindings in the global scope
- ddtrace/vendor/ply/yacc.py:3224:12: warning[unresolved-global] Invalid global declaration of `parse`: `parse` has no declarations or bindings in the global scope
- tests/internal/test_forksafe.py:349:16: warning[unresolved-global] Invalid global declaration of `service`: `service` has no declarations or bindings in the global scope
- Found 6508 diagnostics
+ Found 6503 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/linear_model/tests/test_ransac.py:223:12: warning[unresolved-global] Invalid global declaration of `cause_skip`: `cause_skip` has no declarations or bindings in the global scope
- sklearn/linear_model/tests/test_ransac.py:227:16: warning[unresolved-global] Invalid global declaration of `cause_skip`: `cause_skip` has no declarations or bindings in the global scope
- Found 2051 diagnostics
+ Found 2049 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/ntheory/partitions_.py:13:12: warning[unresolved-global] Invalid global declaration of `_factor`: `_factor` has no declarations or bindings in the global scope
- sympy/ntheory/partitions_.py:13:21: warning[unresolved-global] Invalid global declaration of `_totient`: `_totient` has no declarations or bindings in the global scope
- sympy/testing/runtests.py:2233:16: warning[unresolved-global] Invalid global declaration of `text`: `text` has no declarations or bindings in the global scope
- sympy/testing/runtests.py:2233:22: warning[unresolved-global] Invalid global declaration of `linelen`: `linelen` has no declarations or bindings in the global scope
- sympy/testing/runtests.py:2238:20: warning[unresolved-global] Invalid global declaration of `text`: `text` has no declarations or bindings in the global scope
- sympy/testing/runtests.py:2238:26: warning[unresolved-global] Invalid global declaration of `linelen`: `linelen` has no declarations or bindings in the global scope
- Found 12935 diagnostics
+ Found 12929 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/io/tests/test_mmio.py:29:12: warning[unresolved-global] Invalid global declaration of `mminfo`: `mminfo` has no declarations or bindings in the global scope
- scipy/io/tests/test_mmio.py:30:12: warning[unresolved-global] Invalid global declaration of `mmread`: `mmread` has no declarations or bindings in the global scope
- scipy/io/tests/test_mmio.py:31:12: warning[unresolved-global] Invalid global declaration of `mmwrite`: `mmwrite` has no declarations or bindings in the global scope
- Found 6745 diagnostics
+ Found 6742 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@oconnor663 reviewed on 2025-08-08 03:53_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/import/star.md`:1302 on 2025-08-08 03:53_

We could change this (CPython does include these globals in `*` imports), but we'd need to make `ExportFinder` walk function and class bodies, which it currently doesn't. (And doesn't want to?)

---

_Label `ty` added by @MichaReiser on 2025-08-08 08:22_

---

_@AlexWaygood requested changes on 2025-08-08 15:58_

Hmm, I don't think we should do this with our current capabilities. For code like this, which works fine at runtime, we'll still emit an `unresolved-reference` diagnostic, and I think that's confusing without the `unresolved-global` diagnostic:

```py
def f():
    global x
    x = 42

f()
print(x)
```

On this branch, we report:

```
error[unresolved-reference]: Name `x` used when not defined
 --> foo.py:6:7
  |
5 | f()
6 | print(x)
  |       ^
  |
info: rule `unresolved-reference` is enabled by default
```

Similarly, as you pointed out, the query in `re_exports` still won't take account of any `global` symbols in nested scopes -- it deliberately doesn't recurse into subscopes when attempting to find symbols that might be imported by a `*` import.

Changing either of those pieces of behaviour is something we could consider doing, but each would be somewhat involved and would have tradeoffs to consider. I don't think we should remove the `unresolved-global` lint unless we do both of them, however.

---

_Comment by @oconnor663 on 2025-08-08 18:58_

That makes sense, and I think it's reasonable to close this as a won't-fix-for-now-at-least.

But now you've got me thinking about how we currently define the "public type". Right now we have two notions, as I understand them:

- the "local type"(?) taking account of control flow through the local scope
- the "public type" taking account of all reachable bindings in the current scope, and now/soon unioning that with all reachable bindings in nested scopes

What if there was a third notion?

- the union of all reachable bindings in nested scopes _only_ (not including the current scope)

Then maybe when inferring the local type (i.e. calling `infer_local_place_load` from `infer_place_load`), we could fall back to nested bindings _only_ (not later bindings in the current scope) when a local variable is unbound or possibly unbound? For example, we currently do this:

```py
def f():
    global x
    x = 42
reveal_type(x)  # Unknown (unbound)
f()
reveal_type(x)  # Unknown (unbound)              <-- false positive
if secrets.randbelow(2):
    x = 1
reveal_type(x)  # Literal[1] (possibly unbound)  <-- incomplete
x = 2
reveal_type(x)  # Literal[2]
f()
reveal_type(x)  # Literal[2]                     <-- incorrect
```

But if we wanted to, I think we could achieve this without any new analysis:

```py
def f():
    global x
    x = 42
reveal_type(x)  # Literal[42] (possibly unbound)  <-- false negative
f()
reveal_type(x)  # Literal[42] (possibly unbound)
if secrets.randbelow(2):
    x = 1
reveal_type(x)  # Literal[42, 1] (possibly unbound)
x = 2
reveal_type(x)  # Literal[2]
f()
reveal_type(x)  # Literal[2]                      <-- incorrect
```

Would that be worth pursuing? My default reaction is actually no (this is making things more complicated, and I'm not sure how common it is to see code that benefits from any of this), but let me know what you think.

---

_@carljm reviewed on 2025-08-08 20:58_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/star.md`:1302 on 2025-08-08 20:58_

> CPython does include these globals in `*` imports

Well, maybe -- it does if the function that sets the global has been executed before the import occurs, otherwise it doesn't. So I guess we could consider it a "possibly bound" symbol.

---

_Comment by @carljm on 2025-08-08 21:12_

I think we probably should fix the _incorrect_ results in the above examples (not necessarily right now, but at some point). I'm not that tempted by the exact change proposed above because it doesn't achieve that. (It's not clear to me why the nonlocal writes should be considered if the name is locally unbound, but not otherwise.)

The _simplest_ (but imprecise) way to do that is to union all inferred types of names that have nonlocal bindings with a possible binding of `Unknown`. This is basically "pretend we can't see what exactly the nonlocal bindings are, but we know they exist and therefore the name can be bound to _some_ new value from outside the scope", which is how we handle external writes to fully public scopes like classes or modules (where it really isn't feasible for us to see all the possible writes). But it feels weird to do this when in this case we clearly can see the precise nonlocal writes, and we do take them into account in the public type, just not in the local types.

The other approach (which would not be that hard; you were going to do it in the previous PR until I said not to bother) would be to _always_ include nonlocal bindings as part of the possible type for any use of a local name. This is not wrong, but I'm worried about false positives if we just do that much without more precision.

We could perhaps improve this by considering the creation of any nested scope with any nonlocal writes to a symbol to be a conditional write to that symbol in the local scope's control flow. That would mean we would only consider those nonlocal writes in places in control flow that are reachable from the definition of the nested scope. I think this is probably the best answer, but it definitely makes it a bit more complicated to implement.

I don't think we should prioritize any of this right now; I'd rather have our priorities driven by real-world bug reports. But I will open an issue to track it.

---

_Comment by @carljm on 2025-08-08 21:20_

Opened https://github.com/astral-sh/ty/issues/958

---

_Closed by @oconnor663 on 2025-08-08 21:44_

---
