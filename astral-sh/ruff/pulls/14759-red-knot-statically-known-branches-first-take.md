```yaml
number: 14759
title: "[red-knot] Statically known branches (first take)"
type: pull_request
state: closed
author: sharkdp
labels:
  - great writeup
  - ty
assignees: []
draft: true
base: main
head: david/statically-known-branches
created_at: 2024-12-03T15:07:47Z
updated_at: 2025-01-23T16:50:15Z
url: https://github.com/astral-sh/ruff/pull/14759
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Statically known branches (first take)

---

_@sharkdp_

## Summary

Rendered version of the test suite including a proper introduction to the topic / motivation: [**click**](https://github.com/astral-sh/ruff/blob/david/statically-known-branches/crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md).

This changeset adds support for precise type-inference and boundness-handling of definitions inside control-flow branches with statically-known conditions, i.e. test-expressions whose truthiness we can unambiguously infer as *always false* or *always true*. In code:

```py
x = 1

if "z" in "haystack":  # Literal[False]
    x = 2

reveal_type(x)  # revealed: Literal[1]
```

and:

```py
x = 1

if "y" in "haystack":  # Literal[True]
    x = 2

reveal_type(x)  # revealed: Literal[2]
```

One option to implement this would have been to add special handling for a limited set of test-expressions in semantic index-building. We would then analyze expressions like `sys.version_info >= (x, y)`, `typing.TYPE_CHECKING`, `True` and `False` without any type inference and consequently close down (or unconditionally open) branches whose truthiness we can analyze in this way. This would simplify the implementation, but is much less general than the approach taken here.

Instead, we collect all necessary information during semantic index building, and then re-analyze the control flow during type-checking. The way this works is by recording the list of 'active' branching conditions for each introduced binding. Consider this example:

```py
if test1:
    x = 1  # active conditions: [test1]

    if test2:
        x = 2  # active conditions: [test1, test2]

if test3:
    x = 3  # active conditions: [test3]

use(x)
```

In semantic-index building, when we reach the `use(x)` statement, we have three live bindings. But when we later analyze the test-conditions in type inference, we may potentially conclude that some of them are not present/visible.

One possibility to do so is to detect test-conditions to be *always false*. For example, if `test3` is statically known to be false, we can infer that the `x = 3` branch is never taken. This binding is then not considered in type inference and in boundness-handling. For the `x = 2` binding, we need to consider the possibility that *either* `test1` *or* `test2` can be statically false. In summary, if any of the active branching conditions is statically false, the binding is cancelled.

Another possibility is that we can infer a test-condition to be *always true*. This can also hide bindings. For example, if we can infer `test2` to always be true, we know that the `x = 1` binding is never visible. This is slightly more complex to handle, as we haven't even seen the `test2` condition when we analyze the `x = 1` binding in semantic-index building.

The way we solve this in type-inference is by going through all live bindings in reverse order (starting at `x = 3`), stopping whenever we detect a binding that is *unconditionally visible*. A binding is unconditionally visible if all of its active conditions are statically known to be true. This is slightly complicated by the fact that we have unconditional branching as well. For example, consider:

```py
if test1:
    for _ in iterable():
        x = 1
```

This binding is *not* unconditionally visible, even if `test1` is always true, as the loop body may never be executed. This is currently handled by adding a `BranchingCondition::Unconditional` marker to the active conditions of the binding, which always evaluates to an ambiguous truthiness.

Some of this might sound similar to what we achieve with the `may_be_unbound` and `may_be_undeclared` flags. However, the new mechanism does not allow us to get rid of these flags, as there are also cases like this:
```py
if test:  # type: bool
    x = 1
else:
    x = 2
```
The symbol `x` is always bound, but seeing the `x = 1` or `x = 2` definitions with a branching condition of `test: bool` is not enough to see that. We also need the global information that these two branches will be merged (where we logically OR the two `may_be_unbound = false` flags to a global `may_be_unbound = false`).

This branch also includes:
- statically-known branches handling for Boolean expressions and while loops
- support for `typing.TYPE_CHECKING`
- support for narrowing in `while` loops
- new `target-version` requirements in some Markdown tests which were now required due to the understanding of `sys.version_info` branches.

closes astral-sh/ruff#12700 
closes astral-sh/ruff#14170
closes astral-sh/ruff#14861

## Test plan

- New Markdown tests for the main feature in `statically-known-branches.md`
- New Markdown tests for `typing.TYPE_CHECKING` in `known_constants.md`
- New Markdown tests for `while`-loop narrowing in `narrow/while.md`
- Regression test for boundness-handling in while loops
- Adapted tests for `EllipsisType`, `Never`, etc

## Performance investigation

(thanks to @MichaReiser for some input!)

### Measurements

I can reproduce the regression locally. It's not as pronounced as on codspeed, which could be attributed to the fact that the codspeed results do not involve any I/O overhead (they are based on CPU counters, not wall time). So the local results might be "diluted" a bit:

**tomllib**, -13%

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./red_knot_main` | 21.8 ± 1.5 | 18.8 | 27.0 | 1.00 |
| `./red_knot_feature` | 24.6 ± 1.6 | 21.2 | 29.1 | 1.13 ± 0.11 |

**Black codebase**, -7%

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./red_knot_main` | 123.4 ± 5.4 | 115.6 | 135.4 | 1.00 |
| `./red_knot_feature` | 131.6 ± 3.5 | 123.7 | 137.3 | 1.07 ± 0.05 |

### Analysis (ongoing)

* With this feature enabled, we need to perform more work. For every binding/declaration, we need to infer the type of all active branching conditions and call `Type::bool` on it (which may involve looking up other symbols). This can entail loading entire new modules. For example: in the `tomllib` benchmark, we do not need to resolve `sys` on main, but we do on this branch. The reason for this are various `sys.version_info >= (M, m)` conditions in `builtins.pyi` which now need to be resolved. I tried to account for this by adding a `import sys; sys.version_info` line in a local copy of `tomllib`, so the version on main would also have to import `sys`. This reduces the difference from 13% to 11%, but the comparison is still not fair. There are a lot more symbol-lookups on this feature branch (mostly `__bool__`).
* In semantic-index building, we need to collect, store (and clone, when snapshotting) more information with the addition of the branching-condition data.

## To do

- [ ] Investigate performance regression


---

_Label `red-knot` added by @sharkdp on 2024-12-03 15:07_

---

_Label `no-test` added by @sharkdp on 2024-12-03 15:08_

---

_Comment by @codspeed-hq[bot] on 2024-12-03 15:13_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fstatically-known-branches)

### Merging astral-sh/ruff#14759 will **degrade performances by 21.62%**

<sub>Comparing <code>david/statically-known-branches</code> (0a0e5e6) with <code>main</code> (c3a64b4)</sub>



### Summary

`❌ 2` regressions  
`✅ 30` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fstatically-known-branches)._

### Benchmarks breakdown

|     | Benchmark | `main` | `david/statically-known-branches` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `red_knot_check_file[cold]` | 68 ms | 86.8 ms | -21.62% |
| ❌ | `red_knot_check_file[incremental]` | 4.2 ms | 4.4 ms | -4.31% |


---

_Comment by @github-actions[bot] on 2024-12-03 15:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-12-05 12:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:277 on 2024-12-05 12:16_

Sorry for looking at the pr :) Just an idea: Maybe consider adding a `debug(db)` method that returns a type that implements `Debug`. I think this could be useful outside of tests too

---

_@sharkdp reviewed on 2024-12-05 12:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:277 on 2024-12-05 12:18_

My plan was to eventually remove this entirely. But I needed something to understand the use def map. If it turns out to be more generally useful, I'll consider such a refactoring, thanks.

---

_@MichaReiser reviewed on 2024-12-05 12:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:277 on 2024-12-05 12:20_

I'd be very thankful for it if I ever have to debug the use def map!

---

_@dhruvmanila reviewed on 2024-12-06 14:58_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:277 on 2024-12-06 14:58_

I did something similar for control flow (`FlowSnapshot`) to understand what's visible at each point although it's more of an ad hoc format but it was really useful for me to understand #13966.

---

_Label `no-test` removed by @sharkdp on 2024-12-08 21:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/never.md`:73 on 2024-12-09 21:33_

I'm not sure I understand how the linked bug is relevant to our failure to emit a diagnostic here. `Never` is not unconditionally declared in `typing.pyi`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:6 on 2024-12-09 21:36_

```suggestion
`False` at runtime. In typeshed, it is annotated as `bool`. This test makes sure that we infer
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:42 on 2024-12-09 21:42_

So this is weird, not mentioned in the typing spec, and contrary to the behavior of every other known constant, but [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=02d2b9621a82c46fac926e3ea7c042c4) and [pyright](https://pyright-play.net/?enableExperimentalFeatures=true&code=CoTQCgog%2BgwgEhGBpAkgOQOIAIC8WBiAhgDYDOApgFCUCWAZlqJLAsuhgFyVY9YAeuLKQAuAJ269KATw79BAIjoB7JfOqjyAN3IkowqQAdyACikBKapaA) both actually respect any variable named `TYPE_CHECKING`, no matter where it is defined. The reason for this is that if people are using `if TYPE_CHECKING` to avoid extra runtime import costs from typing, they may also want to avoid the cost of importing the `typing` module itself, so `TYPE_CHECKING = False; if TYPE_CHECKING: ...` is the pseudo-officially recommended pattern in this case.

Curious what @AlexWaygood thinks, but I suspect we will have to emulate this behavior. Sorry I didn't think to mention it sooner.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_alias.md`:3 on 2024-12-09 21:46_

There are other forms of type alias (implicit type aliases, and those annotated with `typing.TypeAlias`) available in earlier versions, so it's clearer to be explicit here. (Probably we should rename the file and h1 to be clearer also.)

```suggestion
PEP 695 type aliases are only available in Python 3.12 and later:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:87 on 2024-12-09 21:56_

Just a stylistic nit: I would probably prefer to use nested headings rather than filenames here? And save filenames for actual multi-file tests (that is, tests with imports). But I don't feel strongly; I realize the headings take up more vertical space.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:632 on 2024-12-09 21:59_

Maybe a test like this but where the `for` loop has a `break` in it? Also tests where the `break` is inside an `if True`, etc?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:316 on 2024-12-09 22:13_

This should have the same behavior without the `== 1`, right? Worth testing?

---

_@carljm reviewed on 2024-12-09 22:15_

Impressively comprehensive tests, thank you!!

Per request, I only looked at the mdtests.

Just a few comments, nothing significant.

---

_@AlexWaygood reviewed on 2024-12-09 23:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:42 on 2024-12-09 23:27_

Hrm... not sure... I feel like I'd prefer for users to explicitly opt into something like that using something like mypy's [`--always-true` option](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-always-true). Pyright also has a similar option. It has the advantage that it's more general, more explicit, more configurable and less magical.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:42 on 2024-12-09 23:29_

Looks like pyright's version of this configuration knob is the `defineConstant` setting: https://github.com/microsoft/pyright/blob/main/docs/configuration.md#environment-options

---

_@AlexWaygood reviewed on 2024-12-09 23:29_

---

_@carljm reviewed on 2024-12-09 23:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:42 on 2024-12-09 23:57_

Hmm, I'm curious where else (and why else) people use this feature, other than for `TYPE_CHECKING`. But apparently people do, if pyright and mypy both support it?

I'm fine with leaving this as a separate feature to be handled later. According to GH code search, it seems like there's a reasonable amount of `TYPE_CHECKING = False` out there; I won't be surprised if we end up feeling like we need to support it by default.

---

_@AlexWaygood reviewed on 2024-12-10 00:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:42 on 2024-12-10 00:04_

I know that people who use both mypy and pyright often use it for cross-type-checker compatibility. You can configure mypy with `--always-true=MYPY` and pyright with `defineConstant = {"MYPY": False}`, and then do things like this so that mypy will only see the first branch and pyright will only see the second:

```py
if MYPY:
    ...
else:
    ...
```

---

_@sharkdp reviewed on 2024-12-10 08:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:316 on 2024-12-10 08:35_

Correct. Changed to the more concise form.

---

_@sharkdp reviewed on 2024-12-10 10:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/never.md`:73 on 2024-12-10 10:48_

You're right. I was able to fix this. It's not related to #14297

---

_@sharkdp reviewed on 2024-12-10 11:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:42 on 2024-12-10 11:10_

> mypy's [`--always-true` option](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-always-true).

> pyright's version of this configuration knob is the `defineConstant` setting

Yes. And @MichaReiser planned a feature for this in Red Knot here: https://www.notion.so/astral-sh/CLI-14548797e1ca80bc98abd50730bbf63a?pvs=4#14948797e1ca80189ad6c8073b45ebf7

This is why I called the test file `known_constants.md`.

> I'm fine with leaving this as a separate feature to be handled later

Ok. Let me know if you think any changes should be made to the test here.

---

_@carljm reviewed on 2024-12-10 15:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:42 on 2024-12-10 15:53_

I think the test is fine as-is for now; we can add the known-constants feature separately, and later evaluate if we feel we need to follow mypy and pyright in making `TYPE_CHECKING` a known-constant by default.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:277 on 2024-12-10 20:21_

I removed my print function as it was painful to update it while working on the code. A debug method sounds interesting, especially if I'm not the only one with that need. But I'll postpone that to another day/PR.

---

_@sharkdp reviewed on 2024-12-10 20:21_

---

_@sharkdp reviewed on 2024-12-10 21:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:87 on 2024-12-10 21:06_

Done. We now have five levels of nesting, but it does look nicer, I agree.

---

_@sharkdp reviewed on 2024-12-10 22:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:632 on 2024-12-10 22:05_

> Maybe a test like this but where the `for` loop has a `break` in it?

Yes, thanks! 

Story time: That actually revealed a missing unconditional-branching point for `else`-clauses in for loops. I had that in the code previously, tried to write a test for it, and couldn't get it to fail. I then removed that line of code (somewhat puzzled why it wasn't required), but the new test clearly shows that it is in fact required, which makes a lot more sense to me.

> Also tests where the `break` is inside an `if True`

I was about to comment: *"are you looking for trouble, sir?"*, but then we settled things on Discord :smile:. We'll postpone things like support for `if False: break` to another PR.

---

_Marked ready for review by @sharkdp on 2024-12-11 16:51_

---

_@sharkdp reviewed on 2024-12-11 18:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/conditional/match.md`:12 on 2024-12-11 18:34_

We are "too smart" now and infer `Literal[3]` here, as we can see that the first pattern can never match. To keep the original intent of the test, I changed the match target to an unknown `int`.

---

_@sharkdp reviewed on 2024-12-11 18:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/loops/while_loop.md`:73 on 2024-12-11 18:39_

This is a regression-test for something that was broken mid-way into this implementation.

---

_@sharkdp reviewed on 2024-12-11 19:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:822 on 2024-12-11 19:20_

I just realized that this is problematic in cases with multiple "always True" patterns. At runtime, we'll take the first of these branches. But in our current implementation here, since we always walk definitions backwards, we'll take the last instead:
```py
x = 1

match "a":
    case "a" if 1 == 1:
        x = 2
    case "a":
        x = 3

reveal_type(x)  # we infer Literal[3], but it should be Literal[2]
```

Before I attempt to fix this, it would be good to get some feedback on whether or not we want static-truthiness checking in `match` statements at all. I thought it was a nice stretch-task, but maybe it causes more trouble than it's worth. The examples presented here are not particularly useful, but there might be some use cases involving enums? Not sure.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/loops/while_loop.md`:83 on 2024-12-11 23:36_

nit: I think putting the code inside a function is a nicer way to get a value of a less-precise type? The other thing I like about it is that it's resilient to the possibility that in the future we do some inlining / return-type inference that lets us infer `True` as the return value of `bool_instance()` (though I'm not sure we'd ever do that when it has an annotated return type)
```suggestion
def _(flag: bool):
    while flag:
        x = 1

    # error: [possibly-unresolved-reference]
    x
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:13 on 2024-12-11 23:43_

We should probably have an `import sys` here? I assume we get away without it because of https://github.com/astral-sh/ruff/issues/14099
```suggestion
import sys
if sys.version_info >= (3, 9):
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:796 on 2024-12-11 23:56_

Would make this test clearer if we didn't have two different `x = 1` in it
```suggestion
        x = 2
    case "b":
        x = 3
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:208 on 2024-12-12 00:00_

What about multiple `elif` branches, where multiple of them are always true?

```py
if flag():
    x = 1
elif True:
    x = 2
elif True:
    x = 3
```

It seems like this _could_ be subject the same problem as you describe below for match patterns, where we'd infer `3` here when it should be `2`? Apparently it isn't, but perhaps it's still worth testing?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:822 on 2024-12-12 00:03_

I don't think we should spend a lot of extra time on implementation work that is specific to statically-known branches with match patterns; it's nice to have, but not clear how often it will be relevant.

I'm curious why this seems to affect match statements with multiple always-matching cases, but not `elif` chains with multiple always-true branches? The two would seem to have a lot in common. In both cases the key fact is that an always-true condition in an `elif` or `case` does not mean "we definitely take this path" (we might have already taken a previous one instead). It only means "we definitely do not take any branch after this one." 

---

_Label `great writeup` added by @carljm on 2024-12-12 00:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:935 on 2024-12-12 01:14_

I find this name a bit confusing. An "unconditional branch" I would usually understand to mean "a branch that is always taken." Here it seems to instead mean "a branch that may or may not be taken, and we aren't recording any test expression (a condition) whose truthiness we can later check to see whether we know if it was taken or not."

(Note that this isn't a fundamental limitation: in principle we could also use type inference to decide that an iterable expression in a `for` loop is definitely empty, or definitely non-empty, in some cases, so we could also know if that branch is never or always taken. We just aren't choosing to do that analysis. And I don't necessarily think we should, possibly not ever, certainly not in this PR. But a `for` branch is a conditional branch just like an `if`, it's just one we aren't currently choosing to analyze for statically-known branching.)

I would suggest naming this method `record_possible_branching()` or `record_ambiguous_branching()` or something similar to that instead.

---

_@carljm reviewed on 2024-12-12 05:24_

Haven't finished my review yet, but submitting a few minor comments I have so far, along with a couple higher-level thoughts/questions on the implementation approach.

(It is possible I'm rehashing here something we already discussed in 1:1 that doesn't work, in which case I apologize for wasting your time rehashing it. Alternatively, if this does make sense, I apologize for not thinking through this carefully enough to suggest it earlier in our 1:1s!)

I wonder if we can do this entirely with the existing constraints support in the use-def map, without the need to add a parallel system for tracking branch-conditions in the use-def map, or the need for the Branch::Ambiguous thing, or the need for the reverse-walking definitions with cutoff.

The constraints system already has the ability to add a constraint to all currently-live definitions. I think this is all we need, _if_ we add the constraints at the right moment.

We would create a new `VisibilityConstraint` kind of constraint (name to-be-improved?), which can be either positive or negative (like any other constraint). Then in code like this:

```py
x = 1
y = 1

if test:
    x = 2
```

Right before we merge the two control flow paths (so at the _end_ of the `if` block) we would add a positive `VisibilityConstraint(test)` to all live definitions in the "branch taken" path (so `y = 1` and `x = 2` would get this constraint), and add a negative `VisibilityConstraint(~test)` to all live definitions in the "branch not taken" path (that's `x = 1` and `y = 1`).

In type inference, if a positive `VisibilityConstraint` evaluates to definitely falsy, or a negative `VisibilityConstraint` evaluates to definitely truthy, it eliminates the definition it applies to.

Note that when we merge control flow paths, if the same definition is live in both paths, any constraints not present for that definition on _both_ paths are dropped. This will mean that in the above code we'll correctly end up with an unconstrained visibility of `y = 1` everywhere (at the merge point, it will have two different constraints from the two paths, and so both will be dropped). After the `if` block, we'll have visibility of `x = 2` constrained by positive `VisibilityConstraint(test)`, and visibility of `x = 1` constrained by negative `VisibilityConstraint(~test)`.

I _think_ this can handle all cases correctly? To take a slightly more complex case:

```py
x = 1
y = 1

if True:
    y = 2
else:
    x = 2
```

Here, right before merging the post-body state we'd apply a `VisibilityConstraint(True)` to all live definitions in that path: `x = 1` and `y = 2`. Right before merging the post-else state we'd apply a `VisibilityConstraint(~True)` to all live definitions in that path: `y = 1` and `x = 2`. This would give us the correct visible definitions of both at the end: `x = 1` and `y = 2` (because `y = 1` and `x = 2` would be eliminated by visibility).

What I like about this approach is a) it fits more neatly into the existing use-def map infrastructure, without needing to add as many new concepts to it, b) it eliminates the need for the "walk definitions backwards and stop when we reach some definition" logic, which still makes me feel a bit uncomfortable (I can't totally put my finger on it, but it feels like it's somehow breaking the abstraction of the use-def map in a way that might cause us problems later), and seems to cause issues at least in the match-statement case.

What do you think of this? Have I missed some important factor?

The other thing I wonder about is handling of boundness. If we take the above approach for definitions, I wonder if we can make boundness just fall out naturally by treating it as just another possible live definition for a symbol -- always the sole initial live definition for a symbol, until another definition is encountered. (I considered this when originally building the use-def map, but decided it was heavier than needed and a simple boolean would suffice, because narrowing constraints don't apply to the "unbound" "definition" -- but now we would have constraints that _do_ apply to it, so the calculus changes.) In this scenario, checking boundness would be a simple matter of "if the 'unbound' definition is live, we are maybe-unbound; if no other definition is live, we are definitely-unbound."

Again, curious for your thoughts on this!

---

_@sharkdp reviewed on 2024-12-12 08:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:935 on 2024-12-12 08:11_

> I find this name a bit confusing. An "unconditional branch" I would usually understand to mean "a branch that is always taken."

Right. I was not happy with the name either. I introduced it for `try`…`except`, but it's confusing for something like a `for` loop. Changing to `record_ambiguous_branching` for now.

> Note that this isn't a fundamental limitation: in principle we could also use type inference to decide that an iterable expression in a for loop is definitely empty, or definitely non-empty, in some cases, so we could also know if that branch is never or always taken. We just aren't choosing to do that analysis. And I don't necessarily think we should, possibly not ever, certainly not in this PR. But a for branch is a conditional branch just like an if, it's just one we aren't currently choosing to analyze for statically-known branching.

Yes. I don't see any value in doing that either. I just added it for `while` branches because it was trivial to do so, not because I think it's a particularly helpful feature.

---

_@sharkdp reviewed on 2024-12-12 08:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/loops/while_loop.md`:83 on 2024-12-12 08:15_

Yes, thanks. This test predates function parameter inference, and I wasn't "notified" by Git when rebasing onto main that the structure of the tests changed here. Did so now.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:109 on 2024-12-12 08:29_

 I'm leaning towards making this a hard assert and add a debug message. It shouldn't add any significant performance cost
 because it just replaces the bounds check on line 115 with the assert here.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:102 on 2024-12-12 08:35_

Do we push a branching condition for every live binding? If we're only pushing an entry for bindings that are known to be always false or true, then I think it might be worth to consider using a `HashMap` instead (or a `Vec` and to linear search) because most code will only have very few "conditions" (the list is not dense).

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:224 on 2024-12-12 08:37_

I don't fully crasp how this works yet, so I might be very wrong here. 

I understand that we'll now iterate from the back when ever looking up a binding. Does that mean that checking now becomes `O(n^2)` if we have something like

```python
x = 1
x += 1 # (iterates over x = 1)
x += 1 # (iterates over x +1, x=1)
x += 1 # (iterates over x + 1, x + 1, x = 1)
... 
```


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:137 on 2024-12-12 08:50_

This is not really related to your change but I'm surprised that `symbol` isn't a `salsa::tracked`... It is called across modules, but it might be fine, at least for as long as `symbol_by_id` never references any AST nodes (or anything that is directly derived from). 

However, it does seem like that `symbol_id` now calls `node_ref` methods in `StaticTruthiness`

https://github.com/astral-sh/ruff/blob/224e4b95fc0ee3d6a33a5f931dc57d180b949d86/crates/red_knot_python_semantic/src/types/static_truthiness.rs#L72-L76

We can't do that because it makes type inference of a single file dependent on the AST nodes of another file. 

This might already be a problem with how we use `Constraint`s today

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:346 on 2024-12-12 08:54_

TODO ;)

---

_@MichaReiser reviewed on 2024-12-12 08:57_

This is great. I haven't reviewed the changes in detail but I left a few understand questions. 

I think it would be great to have a test that demonstrates that statically-known dead branches don't emit diagnostics (or at least demonstrate that they do and have a TODO that we have to handle it). We need to avoid emitting diagnostics for known-dead branches so that it's e.g. possible to use newer python apis in branches that are known to support them without emitting diagnostics if checking for an older python version



---

_@sharkdp reviewed on 2024-12-12 09:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:224 on 2024-12-12 09:46_

> Does that mean that checking now becomes `O(n^2)` if we have something like

I don't think so. We would only ever iterate over one *live* binding in this example. Rewriting using `x = x + 1` to distinguish stores and reads:
```py
x = 1   # binding 1
x = x + 1  # binding 2; read of x only has one visible binding: 1
x = x + 1  # binding 3; read of x only has one visible binding: 2
```

In general, we never iterate over more bindings than we did before on this branch (there is more work to be done in each iteration though). We only changed the order of the iteration. But we might iterate over *fewer* bindings due to short circuiting in an example like this:
```py
x = 1

if True:
    x = 2

x
```
Previously, we would iterate over both bindings of `x`. Now, while iterating in reverse, we can stop at the `x = 2` binding, because nothing else is visible "beyond" this binding.

---

_@sharkdp reviewed on 2024-12-12 10:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:102 on 2024-12-12 10:53_

> Do we push a branching condition for every live binding? If we're only pushing an entry for bindings that are known to be always false or true

We don't know that yet while building the semantic index. So we push an empty branching conditions bitset for every binding we encounter.

> it might be worth to consider using a HashMap instead (or a Vec and to linear search) because most code will only have very few "conditions" (the list is not dense).

I guess you are imagining a ScopedDefinitionId => BitSet HashMap?

There are two questions here:
* How many bindings come with branching conditions attached?
* For those that do, out of the set of all possible branching conditions, how many are set for this binding?

Is the concern that there are many bindings without any branching conditions? Or that there are many conditions and bindings usually only refer to a sparse subset?

If it's the former, I'm not sure that is the case. For every given scope, variables that appear directly in that scope would not have any branching conditions attached. But every binding that appears inside a `try` block, any kind of loop, an `if` block etc. would have at least one such branching condition.

---

_@sharkdp reviewed on 2024-12-12 11:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:208 on 2024-12-12 11:04_

Good suggestion, I added the tests. Thanks. 

> It seems like this _could_ be subject the same problem as you describe below for match patterns, where we'd infer `3` here when it should be `2`? Apparently it isn't,

Right. It works. But there is another case where it doesn't work. I'll explain in the other thread).

---

_@MichaReiser reviewed on 2024-12-12 12:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:102 on 2024-12-12 12:07_

> If it's the former, I'm not sure that is the case. For every given scope, variables that appear directly in that scope would not have any branching conditions attached. But every binding that appears inside a try block, any kind of loop, an if block etc. would have at least one such branching condition.

Okay, I wasn't aware of that. My assumption was that we'd only push a condition if:

* It's known to be true
* It's known to be false
* An enclosing condition is known to be true or false (to now flag that it's unknown)

I then assumed that this is relatively rare and that a different representation might be faster because it is just "empty" for most scopes.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:822 on 2024-12-12 12:08_

> I'm curious why this seems to affect match statements with multiple always-matching cases, but not `elif` chains with multiple always-true branches?

It currently works for `elif` chains but not for match patterns because of negative constraints. The way it currently works is that we record all constraints (including negative ones) when a binding is created:

```py
if flag:
    x = 1  # conditional on flag
elif True:
    x = 2  # conditional on ~flag and True
elif True:
    x = 3  # conditional on ~flag, ~True and True
```

The `x = 3` binding is therefore *not* unconditionally visible, which currently leads us to correctly infer `Literal[1, 2]` for this case.

But now that I write it out like this, I also realize that this reveals a problem with my approach. Consider this example:

```py
x = 1

if flag:
    x = 2  # conditional on flag
elif True:
    x = 3  # conditional on ~flag, True
```

Here, we currently infer `Literal[1, 2, 3]`, while it would be more correct to infer `Literal[2, 3]`. The problem is that we have the negative `~flag` condition in the `x = 3` binding, which prevents us from detecting it as "unconditionally visible". And if we *would* detect it as unconditionally visible, we would run into the problem you mentioned. We would block off any previous bindings and incorrectly infer `Literal[3]`.

We could clearly do better.

I'm currently thinking that there are two problems:
* It seems to me like the set of constraints that we apply for static truthiness analysis needs to be different from the set of constraints that we apply for narrowing. For narrowing, we need that `~flag` constraint. But for static truthiness analysis, we don't care if `flag` has ambiguous truthiness or not, we always take an `elif True` branch.
* When iterating the list of bindings in reverse, we should not stop at the first "unconditionally visible" binding, we need to go back to the earliest possible "unconditionally visible" binding.

---

_@sharkdp reviewed on 2024-12-12 12:08_

---

_@MichaReiser reviewed on 2024-12-12 12:21_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:137 on 2024-12-12 12:21_

Okay, I think I was incorrect when I said that it isn't related to your changes. I did a quick review and it seems that the existing code all ends in a salsa query before accessing the constraint node. We should do the same for static thruthiness OR evaluate if we want to make `symbol_ty` a salsa query and remove the more fine-grained queries instead. 

---

_Comment by @sharkdp on 2024-12-12 12:29_

> (It is possible I'm rehashing here something we already discussed in 1:1 that doesn't work, in which case I apologize for wasting your time rehashing it. Alternatively, if this does make sense, I apologize for not thinking through this carefully enough to suggest it earlier in our 1:1s!)

I appreciate the comment either way. It's also possible we discussed it and I simply didn't fully understand everything.

> The constraints system already has the ability to add a constraint to all currently-live definitions. I think this is all we need, _if_ we add the constraints at the right moment.

> We would create a new `VisibilityConstraint` kind of constraint

> Right before we merge the two control flow paths (so at the _end_ of the `if` block) we would add a positive `VisibilityConstraint(test)` to all live definitions in the "branch taken" path […], and add a negative `VisibilityConstraint(~test)` to all live definitions in the "branch not taken" path […].

Let's consider this example:
```py
x = 1

if True:
    if flag:
        x = 2
```
The correct thing to do is to consider both of these bindings to be potentially visible, i.e. we should infer `Literal[1, 2]`. But if I understand correctly, you would apply a `VisibilityConstraint(~True)` to the `x = 1` binding due to the outer condition. This would render the `x = 1` binding invisible.

The inner condition does not play a role here because the `flag` constraint vanishes as soon as we merge control flow at the end of the inner `if`. If that's confusing, we can also consider something like:
```py
x = 1

if True:
    try:
        may_raise_value_error()
        x = 2
    except ValueError:
        pass
```

---

_Comment by @carljm on 2024-12-12 15:33_

> if I understand correctly, you would apply a `VisibilityConstraint(~True)` to the `x = 1` binding due to the outer condition. This would render the `x = 1` binding invisible.

That constraint would be applied in the outer path, but it would disappear in the outer flow merge because `x = 1` is also live from the inner control flow path, without that constraint. So in this example we would still see `x = 1` as live and infer `Literal[1, 2]`.

But there is still a problem here; not a problem of "not enough visibility" but one of "too much visibility". The logic we use for narrowing "a constraint only applies to a definition after merge if it applies from both paths" is not adequate for visibility. In this example:

```py
x = 1
if True:
    if True:
        x = 2
```

We would have `V(~outerTrue)` for `x = 1` in the outer path, and both `V(outerTrue)` and `V(~innerTrue)` for it in the inner path. All of these would disappear in the outer flow merge, because none of them apply in both paths, so we'd wrongly see `x = 1` as visible. What we actually would need is to preserve a disjunction of the constraints from both paths: `V(~outerTrue) OR (V(outerTrue) AND V(~innerTrue))`.

I think this approach of building up a binary decision diagram of constraints is a reliable way to get the right answer, but it no longer fits cleanly into the existing narrowing-constraints infrastructure in the use-def map; it will require something new.

---

_Converted to draft by @sharkdp on 2024-12-12 15:41_

---

_Comment by @carljm on 2024-12-12 15:44_

> I think it would be great to have a test that demonstrates that statically-known dead branches don't emit diagnostics (or at least demonstrate that they do and have a TODO that we have to handle it)

I think for now this will need to be a test showing we don't suppress diagnostics, with a TODO. (Or we can just file an issue for it and skip the test and TODO.) Detecting dead code is a related, but somewhat different, problem than the one this PR tries to tackle.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:1 on 2024-12-12 15:57_

nit: I think we use snake_case rather than kebab-case for most of our mdtest filenames

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/statically-known-branches.md`:112 on 2024-12-12 15:58_

nit: we support parameter types now ;)

(Same goes for many of other other tests in this file)

```suggestion
def _(flag: bool):
    x = 1

    if flag:
        x = 2

    reveal_type(x)  # revealed: Literal[1, 2]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2612 on 2024-12-12 16:23_

hmm, do we need this, considering we already implement `From<bool>` for Truthiness?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2614 on 2024-12-12 16:23_

might be nice to make this `pub(crate)` as well, just for consistency with the other two you've added

```suggestion
    pub(crate) const fn is_ambiguous(self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/static_truthiness.rs`:31 on 2024-12-12 16:24_

```suggestion
#[derive(Debug)]
pub(crate) struct StaticTruthiness {
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:4064 on 2024-12-12 16:25_

this change seems worth doing on its own -- could consider splitting it off into a separate refactor. (But it's also pretty small, so I definitely don't mind it being part of this PR.)

---

_@AlexWaygood reviewed on 2024-12-12 16:29_

I haven't looked in depth (since Micha and Carl already have), just a couple of very small things from skimming

---

_Comment by @carljm on 2024-12-12 17:06_

Now that this has gone back into Draft and we are exploring other implementation strategies -- it might also make sense to extract the narrowing support for `while` loops and `match` statements into separate PRs, to focus the scope on just the statically-known branches part?

---

_Renamed from "[red-knot] Statically known branches" to "[red-knot] Statically known branches (first take)" by @sharkdp on 2024-12-16 12:30_

---

_Closed by @sharkdp on 2024-12-19 16:25_

---

_Branch deleted on 2025-01-23 16:50_

---
