```yaml
number: 16239
title: "improve diagnostics for \"invalid argument type\""
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/multi-span
created_at: 2025-02-18T19:25:07Z
updated_at: 2025-02-20T13:12:22Z
url: https://github.com/astral-sh/ruff/pull/16239
synced_at: 2026-01-12T15:55:54Z
```

# improve diagnostics for "invalid argument type"

---

_@BurntSushi_

This PR takes a quick route toward prototyping multi-span diagnostics.
As one concrete improvement, the diagnostics for invalid argument types
are improved by adding a secondary span. Specifically, red-knot will
now show not just the call site (which remains as the primary span
in the diagnostic output), but also the function definition with the
corresponding parameter annotated.

The form and definition of secondary diagnostic spans in this PR
is _not_ meant to be where we end up. We will want something more
sophisticated pretty soon, and the version in this PR is very limited.
For example, it doesn't let you attach multiple annotations to the
same snippet of code. Every diagnostic message is one snippet with one
annotation. We obviously want things to be more flexible than that.
Still, this was a useful exericse to go through for myself, and also I
think comes with one concrete improvement.

One comparison point with `rustc` that I found interesting is how dense
`rustc` error messages can be. For example, given this Rust code:

```rust
fn main() {
    foo("a", "b", "c");
}

fn foo(x: i32, y: i32, z: i32) -> i32 {
    x * y * z
}
```

`rustc` will emit:

```
error[E0308]: arguments to this function are incorrect
 --> src/main.rs:2:5
  |
2 |     foo("a", "b", "c");
  |     ^^^ ---  ---  --- expected `i32`, found `&str`
  |         |    |
  |         |    expected `i32`, found `&str`
  |         expected `i32`, found `&str`
  |
note: function defined here
 --> src/main.rs:5:4
  |
5 | fn foo(x: i32, y: i32, z: i32) -> i32 {
  |    ^^^ ------  ------  ------
```

In contrast, in Red Knot presently, a similar program with similar
faults actually results in three different diagnostics being rendered:

```
error: lint:invalid-argument-type
 --> /home/andrew/astral/ruff/play/diags/play/test.py:4:11
  |
4 | other.foo("a", "b", "c")
  |           ^^^ Object of type `Literal["a"]` cannot be assigned to parameter 1 (`x`) of function `foo`; expected type `int`
  |
 ::: /home/andrew/astral/ruff/play/diags/play/other.py:1:9
  |
1 | def foo(x: int, y: int, z: int) -> int:
  |         ------ info: function defined here
2 |     return x * y * z
  |

error: lint:invalid-argument-type
 --> /home/andrew/astral/ruff/play/diags/play/test.py:4:16
  |
4 | other.foo("a", "b", "c")
  |                ^^^ Object of type `Literal["b"]` cannot be assigned to parameter 2 (`y`) of function `foo`; expected type `int`
  |
 ::: /home/andrew/astral/ruff/play/diags/play/other.py:1:17
  |
1 | def foo(x: int, y: int, z: int) -> int:
  |                 ------ info: function defined here
2 |     return x * y * z
  |

error: lint:invalid-argument-type
 --> /home/andrew/astral/ruff/play/diags/play/test.py:4:21
  |
4 | other.foo("a", "b", "c")
  |                     ^^^ Object of type `Literal["c"]` cannot be assigned to parameter 3 (`z`) of function `foo`; expected type `int`
  |
 ::: /home/andrew/astral/ruff/play/diags/play/other.py:1:25
  |
1 | def foo(x: int, y: int, z: int) -> int:
  |                         ------ info: function defined here
2 |     return x * y * z
  |
```

Assuming for the sake of argument that we all agree that `rustc`'s
diagnostic output is better in this case, this is the sort of thing
I'll be thinking about as I work on a proposal for a high level
diagnostic design. But it is definitely not something that is tackled
by this PR. (I considered it, but I think it requires a fair bit of
work, and I didn't want to spend a bunch of time on that during a
prototype.) While `rustc`'s output requires be able to attach multiple
annotations to the same snippet of code, it may also require deeper
changes to how diagnostics themselves are emitted. (For example,
should such things be merged by the diagnostic rendering, or should
the type checker somehow know to merge them?)


---

_Review requested from @carljm by @BurntSushi on 2025-02-18 19:25_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-02-18 19:25_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-02-18 19:25_

---

_Review requested from @sharkdp by @BurntSushi on 2025-02-18 19:25_

---

_Comment by @BurntSushi on 2025-02-18 19:26_

Here's a screenshot of what this diagnostic improvement looks like:

![diag-example](https://github.com/user-attachments/assets/f26c278c-7bff-4524-b350-6b46379d7452)


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:329 on 2025-02-18 19:30_

I think something like this might be a better message? Since the range highlights the parameter declaration rather than the function definition

```suggestion
                            "parameter declared here",
```

or:

```suggestion
                            "parameter declared in function definition here",
```

---

_Comment by @github-actions[bot] on 2025-02-18 19:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-02-18 19:32_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ag%2Fmulti-span)

### Merging #16239 will **degrade performances by 17.42%**

<sub>Comparing <code>ag/multi-span</code> (5db203d) with <code>main</code> (e84985e)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[incremental] `` | 4.7 ms | 5.7 ms | -17.42% |


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:81 on 2025-02-18 19:32_

```suggestion
## Tests for a variety of argument types
```

---

_Comment by @BurntSushi on 2025-02-18 19:33_

Do we expect that codspeed regression to be real?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:1 on 2025-02-18 19:36_

Could you add a test for the case where the "definition" of the function is actually found in one of our stub files (that we vendor from typeshed, found in https://github.com/astral-sh/ruff/tree/main/crates/red_knot_vendored) for the standard library? I'd be curious to see what those diagnostics look like

---

_@AlexWaygood reviewed on 2025-02-18 19:38_

Very nice!

---

_@BurntSushi reviewed on 2025-02-18 19:56_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:1 on 2025-02-18 19:56_

Done! Actually looks pretty okay?

---

_Label `red-knot` added by @MichaReiser on 2025-02-18 20:02_

---

_Comment by @MichaReiser on 2025-02-18 20:08_

The regression is certainly real. The red knot benchmark have 0-1% flakiness at best! 

The regression comes from what we discussed on Discord about incrementality and multi-file diagnostics. Red Knot currently incorrectly emits 3 `invalid-argument-type` diagnostics in `_parser.py` but the function is defined in `_re.py`. That means, we now have to re-infer parts of `_parser.py` because the diagnostic code created a dependency from `_parser.py` to `_re.py`'s AST and our benchmark simulates a no-op change to `_re.py`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Tests_for_a_variety_of_argument_types_-_Variadic_keyword_arguments.snap`:30 on 2025-02-18 20:11_

I think you called this out in your summary. But we probably want different messages for the primary span and the diagnostic itself (the diagnostic now lacks a message)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:86 on 2025-02-18 20:15_

I think you called this out too. We probably want something that scales a little better than a separate function for every combination of arguments :) But I'm fine with this for now

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:280 on 2025-02-18 20:16_

I think it's fine. The `fatal` label should be distinct enough

---

_@MichaReiser approved on 2025-02-18 20:18_

This is cool. I agree that the type checker has to do some more heavy lifting to collapse argument errors for a single function into a single diagnostic. But that's not related to your PR

---

_@AlexWaygood reviewed on 2025-02-18 22:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:1 on 2025-02-18 22:33_

Yeah! Much better than I feared haha 😄

Ideally I think we'd add some kind of note there that explains that we're showing a snippet from typeshed's stubs there rather than the _actual_ definition of the function in the standard library. (We lookup standard-library symbols in the vendored typeshed stubs instead of the actual standard library because the actual standard library doesn't include type information in its function signatures — and also because much of the actual standard library is written in C). But that can wait for another PR. This looks fine for now!

---

_@sharkdp reviewed on 2025-02-19 08:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call/bind.rs`:323 on 2025-02-19 08:56_

In the presence of synthetic arguments like `self`, this matching of argument indices to parameter indices is not always correct, I'm afraid. Consider this example:
```py
class C:
    def __call__(self, x: int) -> int:
        return 1


c = C()
c("wrong")
```

which currently results in a wrong highlighting of the `self` parameter:

![image](https://github.com/user-attachments/assets/cf821f4f-82d4-4cea-80af-fa9645d73f19)

It looks like this can be fixed by taking the information from `parameter.index` instead?

```suggestion
                        let range = func_def
                            .parameters
                            .iter()
                            .nth(parameter.index)
```

This would also lead to more correct ranges for functions with variadic arguments, where we currently highlight *all parameters* if the argument index is (seemingly) "out of bounds":

```py
def f(x: int, *y: str):
    pass

f(1, "foo", None)
```

![image](https://github.com/user-attachments/assets/a04cafaa-ec3d-42fd-b110-0ddad6b8671c)


---

_@BurntSushi reviewed on 2025-02-19 12:35_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/call/bind.rs`:323 on 2025-02-19 12:35_

Nice catches. I added a regression test for the first case. And I think the second case is covered by an existing test. (The span still points to all parameters, but now it doesn't include the parentheses.)

---

_@InSyncWithFoo reviewed on 2025-02-19 13:15_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:184 on 2025-02-19 13:15_

A couple more edge cases to consider (not necessarily as a part of this PR though):

1. Lambda

	```python
	f: Callable[[int], int] = lambda lorem: 1
	f('wrong')
	```

2. Lambda in class body

	```python
	class C:
		f: Callable[[Self, int], int] = lambda self, lorem: 1

	C().f('wrong')
	C.f('also', 'wrong')
	```

3. Lambda as static method

	```python
	class C:
		f: Callable[[int], int] = staticmethod(lambda lorem: 1)

	C().f('wrong')
	C.f('wrong')
	```

---

_Merged by @BurntSushi on 2025-02-19 13:24_

---

_Closed by @BurntSushi on 2025-02-19 13:24_

---

_Branch deleted on 2025-02-19 13:24_

---

_@BurntSushi reviewed on 2025-02-19 13:49_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:184 on 2025-02-19 13:49_

It looks like we might not be type checking lambdas yet? Not sure:

```py
from typing import Callable

f: Callable[[int], int] = lambda lorem: 1
f("wrong")
```

Gives:

```
[andrew@duff play]$ run-red-knot pr1 -- check
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.08s
[andrew@duff play]$
```

---

_@AlexWaygood reviewed on 2025-02-19 13:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:184 on 2025-02-19 13:50_

> It looks like we might not be type checking lambdas yet?

Yeah, I think they're blocked on support for `Callable[]` types

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:275 on 2025-02-19 20:48_

Sorry for the post-land review; I'm way behind on notifications this week. I think instead of adding `callable_ty` to every `InvalidArgumentType`, we should just thread it in from `CallBinding::report_diagnostics` to `CallBindingError::report_diagnostic`. We already pass in the `callable_name`, which is derived from the `callable_ty`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:322 on 2025-02-19 20:51_

As Micha noted in Discord, it would be ideal if we can make this cross-module range-finding lazy at diagnostic rendering time instead. This would both make it easier to avoid doing it in a future "concise" output mode, and would help avoid the unnecessary cross-module AST dependence in cases where this diagnostic is disabled or suppressed.

---

_@carljm reviewed on 2025-02-19 20:51_

Very nice!

---

_@BurntSushi reviewed on 2025-02-19 21:43_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/call/bind.rs`:275 on 2025-02-19 21:43_

Happy to do this. Can you say more about the reasoning? Mostly so that I can try to internalize it and apply it to future work as well. As in, what is the thinking between what is put in the error and what is threaded through to the diagnostic handling outside of the error?

---

_@BurntSushi reviewed on 2025-02-19 21:50_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/call/bind.rs`:322 on 2025-02-19 21:50_

I think this could be challenging. My suspicion is that generating diagnostics is going to greatly benefit from the context in which it is generated. Because that's where all of the information is. I imagine this could also create a cliff in producing diagnostics, where a diagnostic across multiple files needs to be written "different" or "specially" than diagnostics within a single file. It would be much nicer, I think, if writing diagnostics didn't need to care about whether it was reaching across multiple files or not.

Another possible design trajectory is to make the diagnostic production itself aware of whether the diagnostic is enabled or not, and if it is, whether "extra" information should be fetched for it. I think I would call this "conditional diagnostics," where as "lazy diagnostics" to me implies capturing some state that is cheap at the time of reporting the diagnostic, and then only later near rendering time to use the state to, e.g., inspect the relevant parts of the AST to produce a better diagnostic. In my mind, the "lazy" approach breaks locality of creating diagnostics.

---

_@carljm reviewed on 2025-02-19 22:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:275 on 2025-02-19 22:01_

There's no deep reasoning, just a small efficiency gain. All errors in a single binding will always share the same callable, it's already always stored on the binding, and individual errors always render their diagnostics through the binding, so the callable is always available from the binding; doesn't seem like there's any compelling reason to duplicate it on each individual error.

---

_@BurntSushi reviewed on 2025-02-19 22:03_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/call/bind.rs`:275 on 2025-02-19 22:03_

Gotya. That's super helpful. I think I had an unknown unknown here about the bigger picture of this piece of the code that you filled in. Thanks!

---

_@carljm reviewed on 2025-02-19 22:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:322 on 2025-02-19 22:09_

I don't think I have a strong opinion between those design directions, would defer to you and @MichaReiser; would just like to reclaim some efficiency in these cases if we can.

(I guess one thing to consider about the concise-output case is the question of whether we want a config change from concise to verbose output to require a full re-check, or if it's actually better to "waste" some work when generating concise output so we can then generate verbose output without re-checking. This maybe becomes more relevant with persistent caching; I can imagine running with concise output by default but then wanting to re-run with verbose output to explore an unclear error.)

Seems like the main difficulty in the lazy approach is how painful it is to store all the needed context, but I suspect in practice it's not too bad, would just be some Salsa IDs since we can take advantage of Salsa memoization to easily re-query large items like AST. For the conditional diagnostics, it would be how painful it is to make the full diagnostic config available wherever we emit diagnostics.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:91 on 2025-02-19 22:12_

For what it's worth, I don't expect this to be a difficult feature on the type-checking side; all the errors will already be collected in the same binding, it would just mean moving a bit of logic out into `CallBinding::report_diagnostics` to "collect" similar errors and emit them together.

---

_@carljm reviewed on 2025-02-19 22:12_

---

_@MichaReiser reviewed on 2025-02-19 22:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:322 on 2025-02-19 22:16_

We discussed this on Discord.

I consider our CallError etc structs as what @BurntSushi describes as lazy. IMO the main priority is to make diagnostics conditional so that we e.g don't waste time constructing diagnostics for disabled rules or third party code. 

---

_@BurntSushi reviewed on 2025-02-20 13:12_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/call/bind.rs`:275 on 2025-02-20 13:12_

Follow-up is here: https://github.com/astral-sh/ruff/pull/16273

---
