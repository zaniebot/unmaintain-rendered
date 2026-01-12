```yaml
number: 17702
title: "[red-knot] Fix control flow for `assert` statements"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/assert-narrowing
created_at: 2025-04-29T10:53:13Z
updated_at: 2025-04-30T15:28:08Z
url: https://github.com/astral-sh/ruff/pull/17702
synced_at: 2026-01-12T15:56:04Z
```

# [red-knot] Fix control flow for `assert` statements

---

_@AlexWaygood_

## Summary

@sharkdp and I realised in our 1:1 this morning that our control flow for `assert` statements isn't quite accurate at the moment. Namely, for something like this:

```py
def _(x: int | None):
    assert x is None, reveal_type(x)
```

we currently reveal `None` for `x` here, but this is incorrect. In actual fact, the `msg` expression of an `assert` statement (the expression after the comma) will only be evaluated if the test (`x is None`) evaluates to `False`. As such, we should be adding a constraint of `~None` to `x` in the `msg` expression, which should simplify the inferred type of `x` to `int` in that context (`(int | None) & ~None` -> `int`).

## Test Plan

Mdtests added.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-29 10:53_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-29 10:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-29 10:53_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-29 10:53_

---

_Comment by @github-actions[bot] on 2025-04-29 10:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
PyWinCtl (https://github.com/Kalmat/PyWinCtl)
- error[lint:unresolved-import] src/pywinctl/_pywinctl_macos.py:21:8: Cannot resolve import `AppKit`
- error[lint:unresolved-import] src/pywinctl/_pywinctl_macos.py:22:8: Cannot resolve import `Quartz`
- error[lint:unresolved-import] src/pywinctl/_pywinctl_macos.py:25:6: Cannot resolve import `pywinbox`
- error[lint:unresolved-import] src/pywinctl/_pywinctl_macos.py:25:6: Cannot resolve import `pywinbox`
- error[lint:unresolved-import] src/pywinctl/_pywinctl_macos.py:25:6: Cannot resolve import `pywinbox`
- error[lint:unresolved-import] src/pywinctl/_pywinctl_macos.py:25:6: Cannot resolve import `pywinbox`
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:28:13: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:29:12: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:68:35: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:120:29: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:168:109: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:379:60: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:379:90: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:396:62: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:396:96: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_macos.py:421:31: Variable of type `Never` is not allowed in a type expression
- warning[lint:possibly-unresolved-reference] src/pywinctl/_pywinctl_macos.py:1546:54: Name `hSubMenu` used when possibly not defined
- error[lint:unresolved-import] src/pywinctl/_pywinctl_win.py:28:6: Cannot resolve import `pywinbox`
- error[lint:unresolved-import] src/pywinctl/_pywinctl_win.py:28:6: Cannot resolve import `pywinbox`
- error[lint:unresolved-import] src/pywinctl/_pywinctl_win.py:28:6: Cannot resolve import `pywinbox`
- error[lint:unresolved-import] src/pywinctl/_pywinctl_win.py:28:6: Cannot resolve import `pywinbox`
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_win.py:48:35: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_win.py:74:29: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_win.py:109:146: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_win.py:286:42: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_win.py:301:48: Variable of type `Never` is not allowed in a type expression
- error[lint:unresolved-attribute] src/pywinctl/_pywinctl_win.py:314:16: Type `<module 'ctypes'>` has no attribute `windll`
- error[lint:unresolved-attribute] src/pywinctl/_pywinctl_win.py:366:9: Type `<module 'ctypes'>` has no attribute `windll`
- error[lint:unresolved-attribute] src/pywinctl/_pywinctl_win.py:371:9: Type `<module 'ctypes'>` has no attribute `windll`
- error[lint:invalid-type-form] src/pywinctl/_pywinctl_win.py:453:69: Variable of type `Never` is not allowed in a type expression
- error[lint:unresolved-attribute] src/pywinctl/_pywinctl_win.py:461:9: Type `<module 'ctypes'>` has no attribute `windll`
- error[lint:unresolved-attribute] src/pywinctl/_pywinctl_win.py:521:27: Type `<module 'ctypes'>` has no attribute `windll`
- error[lint:unresolved-attribute] src/pywinctl/_pywinctl_win.py:522:27: Type `<module 'ctypes'>` has no attribute `windll`
- Found 83 diagnostics
+ Found 50 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- warning[lint:possibly-unresolved-reference] tests/execution/test_map_async_iterable.py:274:19: Name `anext` used when possibly not defined
- Found 734 diagnostics
+ Found 733 diagnostics

optuna (https://github.com/optuna/optuna)
- error[lint:invalid-type-form] tests/pruners_tests/test_successive_halving.py:116:26: Variable of type `Never` is not allowed in a type expression
- Found 2432 diagnostics
+ Found 2431 diagnostics

apprise (https://github.com/caronc/apprise)
- error[lint:unresolved-attribute] test/test_apprise_config.py:467:20: Type `Literal[ConfigBase]` has no attribute `parse_url`
- Found 3597 diagnostics
+ Found 3596 diagnostics

rotki (https://github.com/rotki/rotki)
- warning[lint:unresolved-reference] rotkehlchen/tests/unit/test_ethereum_inquirer.py:425:9: Name `etherscan_tx_or_tx_receipt_calls` used when not defined
- Found 3396 diagnostics
+ Found 3395 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:30:20: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:31:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:34:30: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:42:42: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:156:24: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:383:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:399:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:485:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/bytecode_injection/core.py:590:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_10.py:15:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_10.py:15:48: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:37:57: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:55:31: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:55:49: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:68:53: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:122:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:193:33: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:207:47: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:251:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_11.py:251:48: Variable of type `Never` is not allowed in a type expression
- warning[lint:possibly-unresolved-reference] ddtrace/internal/coverage/instrumentation_py3_11.py:445:30: Name `ext_instr` used when possibly not defined
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_12.py:27:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_12.py:27:48: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_12.py:40:31: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_12.py:40:55: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_12.py:59:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_12.py:59:27: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_13.py:21:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/coverage/instrumentation_py3_13.py:21:48: Variable of type `Never` is not allowed in a type expression
- Found 7030 diagnostics
+ Found 7001 diagnostics

```
</details>


---

_@sharkdp reviewed on 2025-04-29 13:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1374 on 2025-04-29 13:11_

I'm trying to understand why it is correct to take this snapshot after visiting the expression? Should it also be renamed to `post_test` or `pre_msg`?

---

_@sharkdp reviewed on 2025-04-29 13:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/assert.md`:114 on 2025-04-29 13:11_

Can we also add a test for visibility constraint handling? For example:

```py
assert True, (x := 1)

# error: [unresolved-reference]
x
```

---

_@AlexWaygood reviewed on 2025-04-29 13:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1374 on 2025-04-29 13:27_

Ah, great point. Yes, this is a snapshot of the state after we've visited the `test` but before we've visited the `msg`. It's the state we need to return to after we've visited the `msg`, since all code following the `assert` statement can only be reached if `test` evaluated to `True` (which means that the `msg` expression would not have been evaluated). I'll rename the variable.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/assert.md`:114 on 2025-04-29 13:28_

Isn't that what I test in the `one()` function in the `## Assertions with definitions inside the message` section?

---

_@AlexWaygood reviewed on 2025-04-29 13:28_

---

_@AlexWaygood reviewed on 2025-04-29 13:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/assert.md`:114 on 2025-04-29 13:28_

Oh, no, I see what you mean, thanks!

---

_@sharkdp approved on 2025-04-29 13:35_

Thank you!

---

_@sharkdp reviewed on 2025-04-29 13:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/assert.md`:114 on 2025-04-29 13:38_

I should have explained that better... the idea would be to have a test that has a statically known test condition. Seeing that `unresolved-reference` error there gives us certainty that the `x := 1` binding has a visibility constraint on it that evaluates to `AlwaysFalse`. Improper handling would lead to "possibly-unresolved-reference" or no error at all.

---

_@AlexWaygood reviewed on 2025-04-29 13:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/assert.md`:114 on 2025-04-29 13:50_

yes, it looks like we did indeed have some false-positive `unresolved-reference` diagnostics in unreachable code. Mind double-checking the reachability constraints I added in https://github.com/astral-sh/ruff/pull/17702/commits/c8e3145534e3dfd20ab987c92cafcb891d46fa8f to fix them? I'm not very familiar with the reachability-constraint handling we have

---

_Closed by @AlexWaygood on 2025-04-29 13:51_

---

_Reopened by @AlexWaygood on 2025-04-29 13:51_

---

_Review request for @carljm removed by @carljm on 2025-04-29 13:55_

---

_Comment by @AlexWaygood on 2025-04-29 17:00_

Oh, it looks like https://github.com/astral-sh/ruff/pull/17702/commits/56fa086bfa0925fb42c92caec7b82403afd660da introduced lots of new mypy_primer hits @sharkdp

---

_Comment by @carljm on 2025-04-29 18:11_

Just spot-checking the very first error (from parso), it looks to me like this PR is now wrongly suggesting that there is a possible control flow path from the assertion message to the following code, where in fact, the assertion message control flow path is always terminal.

---

_Comment by @sharkdp on 2025-04-29 18:48_

Should not have pushed just before leaving. I'll fix that, of course.

---

_Closed by @sharkdp on 2025-04-29 19:28_

---

_Reopened by @sharkdp on 2025-04-29 19:28_

---

_Comment by @sharkdp on 2025-04-29 19:50_

Ok, this looks much better. The three new diagnostics on `attrs` are extremely weird — looking into it.

---

_@carljm approved on 2025-04-29 20:58_

This looks great to me (pending investigation of the new `attrs` diagnostics.)

The new psycopg2 diagnostics look related at least in part to not understanding cyclic control flow, though it's not clear to me why this PR surfaces them.

---

_Comment by @sharkdp on 2025-04-29 21:02_

> The new psycopg2 diagnostics look related at least in part to not understanding cyclic control flow, though it's not clear to me why this PR surfaces them.

I think it's related to #17723. I will look into that tomorrow.

---

_Comment by @sharkdp on 2025-04-30 07:51_

@AlexWaygood The new fuzz check claims it found a new panic on this branch. The following snippet panics indeed:
```py
class name_3[name_2: name_0]:
    pass
name_3.name_4
assert name_3[name_0]
```
but it also panics on `main`. It's my understanding that we still detect a lot of panics with the fuzzer in general, so I'm going to ignore that "additional" failure — but let me know if you think that it should be reported separately.

If it's just the fuzzer test that's being flaky, maybe that's something to be aware of / to look into.

---

_Comment by @sharkdp on 2025-04-30 07:57_

> Ok, this looks much better. The three new diagnostics on `attrs` are extremely weird — looking into it.

Turns out this was also related to https://github.com/astral-sh/ruff/issues/17723. After fixing this, these new diagnostics are gone again.

> The new psycopg2 diagnostics look related at least in part to not understanding cyclic control flow, though it's not clear to me why this PR surfaces them.

It's true that we don't completely understand that code. But in the absence of cyclic control flow, we should not emit an error, because the `else` branch is "unreachable":
```py
assert …  # some assertion that affected reachability constraints below (without the fix for #17723)

first = True
for params in params_seq:
    if first:  # always true, in the absence of cyclic control flow
        pgq = …
        first = False
    else:
        use(pgq)  # unreachable, in the absence of cyclic control flow
```

After fixing #17723, we now "correctly" identify that `else` branch as unreachable again, and no diagnostics are emitted.

---

_Merged by @sharkdp on 2025-04-30 07:57_

---

_Closed by @sharkdp on 2025-04-30 07:57_

---

_Branch deleted on 2025-04-30 07:57_

---

_Branch restored on 2025-04-30 11:11_

---

_Comment by @AlexWaygood on 2025-04-30 11:21_

> @AlexWaygood The new fuzz check claims it found a new panic on this branch. The following snippet panics indeed:
> 
> ```python
> class name_3[name_2: name_0]:
>     pass
> name_3.name_4
> assert name_3[name_0]
> ```

It's weird to me that it's highlighting this snippet specifically. It looks like something that _could_ well be affected by this branch, since it includes an `assert` statement on the final line. But I agree with you -- it looks like it panics in exactly the same way on the commit prior to this one.

> but it also panics on `main`. It's my understanding that we still detect a lot of panics with the fuzzer in general, so I'm going to ignore that "additional" failure — but let me know if you think that it should be reported separately.
> 
> If it's just the fuzzer test that's being flaky, maybe that's something to be aware of / to look into.

Yes, this seems to be flakiness in the fuzzer test. Passing the `--only-new-bugs` flag (which is what we're doing in the CI job) should mean that it ignores pre-existing panics, and only reports _new_ panics relative to the `main`-branch baseline. I'm looking into it.

---

_Branch deleted on 2025-04-30 11:21_

---

_Comment by @AlexWaygood on 2025-04-30 11:47_

@sharkdp there _is_ a bug in the fuzzer script here... but it's not quite what it seemed.

There is indeed code that involves `assert`s which now panics following this PR, but did not panic prior to this PR. Try running red-knot on https://gist.github.com/AlexWaygood/0925516f7b39516cf02f5dac6fca541e for a repro.

What happened here is the following:
1. The fuzzer script ran the new version of red-knot on a large file of randomly generated code (the gist I linked to above), and spotted that red-knot panicked.
2. The fuzzer script then ran the baseline version of red-knot on the same file, noticed that the old version of red-knot did not panic, and deduced that the panic constituted a "new red-knot bug".
3. The fuzzer script then used a different Python library to minimize the large file of randomly generated code to a snippet of code that was as small as possible but still reproduced a panic from red-knot. It accidentally "overly minimized" the snippet: the minimal repro is now something that the baseline version of red-knot also panics on.

I'll update the minimization logic in the fuzzer script to be more sophisticated about this in the future, so that it doesn't produce such confusing results. I don't know if it's worth looking into the panic from https://gist.github.com/AlexWaygood/0925516f7b39516cf02f5dac6fca541e right now or not. As you say, we have plenty of pre-existing cases involving `assert`s and/or PEP-695 classes that produce panics. I'll leave that decision to you :-)

---

_Comment by @sharkdp on 2025-04-30 13:14_

> The fuzzer script then used a different Python library to minimize the large file of randomly generated code to a snippet of code that was as small as possible but still reproduced a panic from red-knot. It accidentally "overly minimized" the snippet: the minimal repro is now something that the baseline version of red-knot also panics on.

Ahh — that makes sense. Thank you for the analysis. I think I'm going to skip analyzing this further for now.

---

_Comment by @AlexWaygood on 2025-04-30 15:13_

Here is a minimized repro of something that did not panic prior to this PR, but does now (courtesy of a fixed version of the fuzzer script that I have locally):

```py
class name_3[name_2: name_0]:
    pass
name_3.name_4
assert name_3[name_0]
import name_0
```

---

_Comment by @AlexWaygood on 2025-04-30 15:14_

I think it is probably still the case that this PR just exposed a pre-existing issue rather than actually introducing a new issue. So I think it's still okay to just leave this for now.

---

_Comment by @sharkdp on 2025-04-30 15:17_

I also think so. The following snippet also panics (introduces the same reachability constraints than the `assert`ion).
```py
class name_3[name_2: name_0]:
    pass
name_3.name_4
if name_3[name_0]:
    import name_0
```

---

_Comment by @AlexWaygood on 2025-04-30 15:28_

And here's a fix for the fuzzer issue: https://github.com/astral-sh/ruff/pull/17739

---
