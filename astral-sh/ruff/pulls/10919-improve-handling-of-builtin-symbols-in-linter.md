```yaml
number: 10919
title: Improve handling of builtin symbols in linter rules
type: pull_request
state: merged
author: AlexWaygood
labels:
  - linter
assignees: []
merged: true
base: main
head: improve-builtins-handling
created_at: 2024-04-13T18:09:32Z
updated_at: 2024-04-16T10:37:34Z
url: https://github.com/astral-sh/ruff/pull/10919
synced_at: 2026-01-12T15:55:33Z
```

# Improve handling of builtin symbols in linter rules

---

_@AlexWaygood_

## Summary

A pattern that shows up a lot in the `ruff_linter` crate currently is to do something like this to determine whether an `ast::Expr` node refers to a builtin symbol:

```rs
fn do_the_check(checker: &mut Checker, expr: &ast::Expr) {
    let ast::Expr::Name(ast::ExprName { id, .. }) = expr else {
        return;
    }
    if id != "zip" {
        return;
    }
    if !checker.semantic().is_builtin("zip") {
        return;
    }
    // and now we do the check that only applies to the builtin zip function...
}
```

This has two problems:
1. It's pretty verbose and repetitive, and _lots_ of our checks have some interaction with a builtin function or class in some way
2. This pattern doesn't account for the fact that it's perfectly possible to explicitly import the `builtins` module and e.g. refer to `zip` by using `builtins.zip` rather than just `zip`

This PR adds a new method to the semantic model that means that this kind of logic becomes both more concise and more accurate. The above logic can now be replaced with this, which will also (unlike the old function) recognise `builtins.zip` as being a reference to the builtin `zip` function:

```rs
fn do_the_check(checker: &mut Checker, expr: &ast::Expr) {
    if !checker.semantic().match_builtin_expr(expr, "zip") {
        return;
    }
    // and now we do the check that only applies to the builtin zip function...
}
```

A new method is also added to `crates/ruff_linter/src/importer/mod.rs` to enable us to provide autofixes involving builtin functions/classes, even when they've been shadowed by a user-defined function or class in the current scope.

## Test Plan

`cargo test`. Several fixtures have been extended with new examples to test the new functionality.

This PR should be easiest to review commit-by-commit: each commit passes the full test suite by itself.


---

_Renamed from "Add a new method to check whether an Expr refers to a builtin symbol" to "Improve handling of builtin symbols in linter rules" by @AlexWaygood on 2024-04-13 18:10_

---

_Label `linter` added by @AlexWaygood on 2024-04-13 18:11_

---

_@charliermarsh reviewed on 2024-04-13 18:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:271 on 2024-04-13 18:18_

As written I believe this method will do two lookups in the symbol table in some cases (`self.is_builtin` performs one lookup, and `self. resolve_qualified_name` performs another).

I think you could modify it to look something like this:

```rust
if self.seen_module(Modules::BUILTINS) || expr.as_name_expr().is_some_and(|name| name.id == symbol) {
    self
        .resolve_qualified_name(expr)
         // This next line is pseudo-code.
        .is_some_and(|qualified_name| qualified_name.segments() == ["" | "builtins", symbol])
} else {
  false
}


---

_Comment by @github-actions[bot] on 2024-04-13 18:22_

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

_@AlexWaygood reviewed on 2024-04-13 18:23_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:271 on 2024-04-13 18:23_

> As written I believe this method will do two lookups in the symbol table in some cases (`self.is_builtin` performs one lookup, and `self. resolve_qualified_name` performs another).

Hmm, will it? Currently:
- If it's an `Expr::Name`, we take the fast(er) path, and we never actually call `resolve_qualified_name()`
  - we return `true` if the `id` of the `Name` matches the symbol and `self.is_builtin(symbol)` is also true
  - else, we return `false`
- Otherwise, in the slow path, we don't actually call `self.is_builtin(symbol)` at all, since as you say it's redundant if we have to call `resolve_qualified_name()`

---

_Comment by @AlexWaygood on 2024-04-13 18:31_

> + [stdlib/asyncio/trsock.pyi:93:29:](https://github.com/python/typeshed/blob/b61d90c6f5dc4600055916f98c0e63deec0546e9/stdlib/asyncio/trsock.pyi#L93) PYI036 The first argument in `__exit__` should be annotated with `object` or `type[BaseException] | None`

Ah, that's a regression: something can be imported explicitly from `builtins` even if it's not an `ast::Name` node. My fast path doesn't work as it's currently written.

---

_@charliermarsh reviewed on 2024-04-13 18:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:271 on 2024-04-13 18:35_

Oh, then the function logic is incorrect, right? Since a name node could be imported from builtins, as in `from builtins import bool`?

---

_@AlexWaygood reviewed on 2024-04-13 18:38_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:271 on 2024-04-13 18:38_

Correct. Fixed in https://github.com/astral-sh/ruff/pull/10919/commits/f11bf9807199ac286098b4d7a07af17044aaf3c6 :) Thanks!

---

_Comment by @AlexWaygood on 2024-04-13 18:47_

Seems like this leads to a 1% perf regression overall, with regressions of 2-3% on some linter benchmarks: https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:improve-builtins-handling

I don't know whether that's worth it or not — it's less repetitive code for us and it's more principled in that we're handling the semantics of Python more accurately, but importing `builtins` explicitly also isn't that common.

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-04-13 18:51_

---

_@charliermarsh reviewed on 2024-04-13 18:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:266 on 2024-04-13 18:58_

I would remove this `return`, it's roughly what tripped me up in the first read. E.g., I find this clearer:

```rust
/// Return `true` if `member` is a reference to `builtins.$target`,
/// i.e. either `object` (where `object` is not overridden in the global scope),
/// or `builtins.object` (where `builtins` is imported as a module at the top level)
pub fn match_builtin_expr(&self, expr: &Expr, symbol: &str) -> bool {
    debug_assert!(!symbol.contains('.'));
    if self.seen_module(Modules::BUILTINS) {
        self.resolve_qualified_name(expr)
            .is_some_and(|qualified_name| match qualified_name.segments() {
                ["builtins" | "", member] => *member == symbol,
                _ => false,
            })
    } else {
        match expr {
            Expr::Name(ast::ExprName { id, .. }) => id == symbol && self.is_builtin(symbol),
            _ => false,
        }
    }
}
```

---

_Comment by @charliermarsh on 2024-04-13 19:01_

Would you have expected ~no change here for benchmarks that don't import `builtins`?

---

_Comment by @AlexWaygood on 2024-04-13 19:05_

> Would you have expected ~no change here for benchmarks that don't import `builtins`?

yeah. I would have hoped that the fast path from the `if self.seen_modules(Modules::BUILTINS)` check would mean that performance would be basically unchanged for those benchmarks.

Any idea what might be the cause of the slowdown? `pyflakes::rules::unused_import` seems to be showing up in a bunch of the codspeed flamegraphs, which is weird, because I haven't touched that function in this PR

---

_@AlexWaygood reviewed on 2024-04-13 19:29_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:264 on 2024-04-13 19:29_

Let me know if this isn't the right thing to do here, but it improved performance by 3-4% on some benchmarks locally

---

_@AlexWaygood reviewed on 2024-04-13 19:35_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:264 on 2024-04-13 19:35_

But apparently not enough to make much of a dent on how codspeed sees things... https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:improve-builtins-handling

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:264 on 2024-04-13 21:46_

Ah yeah, I don't think this should be necessary. If anything you might consider finding a way to mark the `if seen(BUILTINS)` branch as cold, but we don't really do that anywhere else.

---

_@charliermarsh reviewed on 2024-04-13 21:47_

---

_Comment by @charliermarsh on 2024-04-13 21:48_

No, I'm not sure. I looked through the changes again and my expectation would've been no change. You can try just pushing again and see if the benchmarks move in the other direction, since it might just be noise. I wouldn't read into the flamegraph diff though -- I think CodSpeed is just confused.

---

_Comment by @charliermarsh on 2024-04-13 21:49_

The other thing you might consider doing is benchmarking locally with Hyperfine. E.g., you could build a release build on main, stash it (`mv ./target/release/ruff ./target/release/main`), then build on your branch and do something like:

```
hyperfine --warmup 10 --runs 100 \
    "./target/release/main ./crates/ruff/resources/test/cpython/ --no-cache --silent -e" \ 
    "./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e" 
```

I'd suggest running it a few times and switching the order of `main` and `ruff` around to reach a safe conclusion.

---

_Comment by @AlexWaygood on 2024-04-13 22:20_

> since it might just be noise.

I'm getting similar results from running `cargo benchmark` locally (tried it a few times earlier), so I don't _think_ it's just noise :/ I'll try tomorrow with hyperfine to see if the macrobenchmark shows anything different to the microbenchmarks...

---

_Comment by @charliermarsh on 2024-04-13 22:51_

If you run the micro-benchmarks locally, I suggest running them a few times consecutively for each PR. At least on my machine, the second run of the same binary is always significant faster.

---

_Comment by @carljm on 2024-04-14 11:26_

It seems like this PR is doing two separable things: improving detection of what is a builtin, and allowing auto-fixes that want to use a builtin to import it via `builtins` module if the name is shadowed. 

Have you benchmarked those changes in isolation, to clarify which one the regression is coming from? IIUC we always eagerly generate fixes, so it seems like a change that allows more fixes in more cases could easily cause a regression?

---

_Comment by @charliermarsh on 2024-04-14 11:43_

Yeah, although we should only be generating more diagnostics (and therefore more fixes) for files that import builtins, and we don’t have any such files in our benchmarks (IIUC).

It _could_ be that computing the fixes become more expensive (the use of get_or_import_symbol will be more expensive) even in the case that the file doesn’t import builtins, though I don’t have intuition on whether that could account for the delta here. You could try removing the changes that involve modifying the fixes, to see if the benchmarks return to normal as Carl suggests.

---

_Comment by @carljm on 2024-04-14 16:37_

It looks to me like all the rules that are changed to use `try_set_fix` with the new `get_or_import_builtin_symbol` (e.g. `unreliable_call_check` is one example) will now generate more fixes than before in any file where the builtin they want to add a reference to is shadowed. E.g. in a module that contains `callable = ...`, `unreliable_call_check` will now generate a fix that imports `builtins` and uses `builtins.callable`, rather than bailing and not generating a fix at all. Similar for a number of other rules. So this would generate more fixes even in files that don't currently import `builtins`.

I haven't checked whether the benchmarks in question do this, but this change seems entirely separable from the better detection of imported builtins, so if there are any mysteries it seems like a good first step is to separate the changes and narrow down the cause. (And if it's still a mystery, perhaps further separate by only changing one rule at a time.)

---

_Comment by @charliermarsh on 2024-04-14 17:16_

Oh, that's true! Good call. I would be surprised if we had any instances of those in our benchmark files (and especially not in _all_ of them) but it's possible there are a few and it's definitely a good idea to isolate the changes.

---

_Comment by @AlexWaygood on 2024-04-14 18:07_

Yet another possibility: the change to the fixes also involves changing a bunch of fixes to use `Fix::safe_edits` instead of `Fix::safe_edit`, even if a builtin symbol is not overridden in the relevant scope. It's possible `safe_edits` is slower than `safe_edit`, even if only a single edit is passed? Anyway, I didn't get to it today, but I'll do some benchmarking tomorrow without the changes to the fixes, as @carljm suggests :-) 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:274 on 2024-04-15 07:16_

Nit, you could use `matches!` here
```suggestion
            matches!(expr, Expr::Name(ast::ExprName { id, .. }) if id == symbol && self.is_builtin(symbol))
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:265 on 2024-04-15 07:18_

This is the case where I don't like our clippy rule that switches the `if` and` else if the if condition is negated. Naturally, I would expect that the "fast" path comes first (and that's also what branch predictors assume by default, as far as I know). 

You could try to use 

```
        // fast path
        if !self.seen_module(Modules::BUILTINS) {
            return matches!(expr, Expr::Name(ast::ExprName { id, .. }) if id == symbol && self.is_builtin(symbol));
        }

        self.resolve_qualified_name(expr)
            .is_some_and(|qualified_name| match qualified_name.segments() {
                ["builtins" | "", member] => *member == symbol,
                _ => false,
            })
```

which gets you around the clippy rule and lays out the code so that the "default" branch comes first. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/mutable_fromkeys_value.rs`:82 on 2024-04-15 07:22_

Is it intentional that `is_builtin` uses `lookup_symbol` but the new implementation now uses `resolve_qualified_name` (which also works for member expression which the old implementation didn't)?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/mutable_fromkeys_value.rs`:76 on 2024-04-15 07:24_

Will this generate a syntatically correct fix if we have `builtins.dict.fromkeys()`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:119 on 2024-04-15 07:27_

I think this change negatively impacts performance. The idea was to have all cheap tests early. Calling `resolve_symbol` isn't cheap. It might be worth adding a "fast" path in the slow path of `match_builtin_expr` that exits early if `expr` is a name but doesn't match the builtin name (except if you want to support `from builtins import float as f`)


You may want to move this check further down. The same applies for other places where we now perform this check as the very first thing in the function.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:57 on 2024-04-15 07:30_

I think it would be good to add additional tests that cover the newly added logic. E.g. that it now supports `builtins.open` or `from bultins import open as o` (assuming this is indeed now supported). 

Another test could be 

```
with (a as open, open(b) as b):
    ...
```

The added tests would also demonstrate that the genrated fixes work correctly.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_open_mode.rs`:98 on 2024-04-15 07:32_

Is this check still necessary considering that `match_builtin_expr` should test that it is the open function?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_open_mode.rs`:99 on 2024-04-15 07:34_

Nit: This code confused me a lot because the line above rebinds `func` which I assumed was the `open` function. It might be worth giving it another name.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:75 on 2024-04-15 07:36_

Nit: Consider adding a method `is_builtin` to `QualifiedName` that tests the segments

```
if qualified_name.is_builtin("min") {
    Some(MinMax::Min)
} else if qualified_name.is_builtin("max") {
    Some(MinMax::Max)
} else {
    None
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:295 on 2024-04-15 07:42_

I wonder if we could instead change the method to return a `QualifiedName` where constructing the `QualifiedName` short-circuits if `Modules::BUILTINS` isn't imported. 

I'm bringing this up because `QualifiedName` already has methods for handling builtins (that might be incorrect?)

```
pub fn is_builtin(&self) -> bool {
    matches!(self.segments(), ["", ..])
}
```

So what I would have in mind is

```
model.resolve_builtin_name(expr).is_some_and(|builtin| builtin.is_builtin_name("open"))
```

We could still have a helper as you have today to avoid some of the repetition (`is_some_and` is somewhat annoying). But it would build up on the same concepts.

---

_@MichaReiser reviewed on 2024-04-15 07:42_

---

_@AlexWaygood reviewed on 2024-04-15 07:48_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:265 on 2024-04-15 07:48_

Heh, that's what I had originally! But @charliermarsh complained ;) https://github.com/astral-sh/ruff/pull/10919#discussion_r1564197567

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:75 on 2024-04-15 07:49_

`QualifiedName` already has an `is_builtin` method, which does something slightly different right now:

https://github.com/astral-sh/ruff/blob/cbd500141f1b1dd30467945700cba3469646cd80/crates/ruff_python_ast/src/name.rs#L50-L54

---

_@AlexWaygood reviewed on 2024-04-15 07:49_

---

_@AlexWaygood reviewed on 2024-04-15 07:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/bad_open_mode.rs`:98 on 2024-04-15 07:53_

Yes. `match_builtin_expr` above checks whether the func represents either `open("foo")` or `builtins.open("foo")`, both of which are calls to the `builtins.open()` function. A this point, we're now checking to see whether the func represents either `Path("foo").open()` or `pathlib.Path("foo").open()`, which are different function calls but have similar semantics to `builtins.open()`, so are treated equivalently by this pylint rule.

---

_@AlexWaygood reviewed on 2024-04-15 07:54_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:274 on 2024-04-15 07:54_

TIL :-)

---

_@AlexWaygood reviewed on 2024-04-15 07:56_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:119 on 2024-04-15 07:56_

> except if you want to support `from builtins import float as f`

I do, and we already support that in several of our rules. I tried the fast path you're suggesting here in the initial version of this PR, and it caused a regression in the ecosystem check: https://github.com/astral-sh/ruff/pull/10919#issuecomment-2053725755

Also Charlie told me off for not getting Python's semantics correct :-)

---

_@AlexWaygood reviewed on 2024-04-15 07:56_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:119 on 2024-04-15 07:56_

> You may want to move this check further down. The same applies for other places where we now perform this check as the very first thing in the function.

that makes sense, I'll look at moving these checks down

---

_@AlexWaygood reviewed on 2024-04-15 07:58_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/mutable_fromkeys_value.rs`:76 on 2024-04-15 07:58_

I think so

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/mutable_fromkeys_value.rs`:76 on 2024-04-15 08:11_

I added a test for it in `crates/ruff_linter/resources/test/fixtures/ruff/RUF024.py`, and the snapshot looks fine to me

---

_@AlexWaygood reviewed on 2024-04-15 08:11_

---

_Comment by @AlexWaygood on 2024-04-15 09:06_

@MichaReiser's suggestions to move the symbol lookups lower down the function for each rule got rid of the performance regression 🥳🥳

---

_@charliermarsh reviewed on 2024-04-15 11:41_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:265 on 2024-04-15 11:41_

I misread that version which suggests to me that others will do the same in the future. I find this much clearer, but don't feel strongly and not important enough to debate it so defer to you. If you use that version, I'd suggest at least adding comments and a newline between the closing `}` and the method call as above.

---

_@AlexWaygood reviewed on 2024-04-15 11:43_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:265 on 2024-04-15 11:43_

To be clear, I'm personally fine with either; I can see both points of view here!

---

_Comment by @charliermarsh on 2024-04-15 11:44_

That's great! In my head those lookups didn't matter since they were only occurring when we saw the "right" name (e.g., `open` for a rule that cares about open), but of course that's not true -- we were doing them far more frequently on this branch.

---

_@AlexWaygood reviewed on 2024-04-15 11:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/mutable_fromkeys_value.rs`:82 on 2024-04-15 11:47_

I _don't_ think there's a need for the new abstraction to work with member expressions. I just used `resolve_qualified_name` because it seemed like the simplest way to implement the new abstraction. Do you think using `lookup_symbol` would be more efficient here? (_Goes to compare the two definitions..._)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:72 on 2024-04-15 11:47_

The counterpoint is that we're now doing _other_ work that isn't strictly necessary (for example, if this is a name, and builtins isn't imported, and the name isn't `getattr`, then the call to `is_mangled_private` wasn't necessary -- we could've exited much earlier; similarly, we risk calling `is_identifier` on many more function calls that won't ever match).

---

_@charliermarsh reviewed on 2024-04-15 11:47_

---

_@charliermarsh reviewed on 2024-04-15 11:48_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:72 on 2024-04-15 11:48_

This is clearly still the right tradeoff, but that's the tradeoff.

---

_@AlexWaygood reviewed on 2024-04-15 11:48_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:72 on 2024-04-15 11:48_

Correct

---

_@charliermarsh reviewed on 2024-04-15 11:49_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:72 on 2024-04-15 11:49_

The only way around this would be to, like, have some `could_be_builtin("getattr")` method you call at the top that returns `false` if builtins isn't imported, it's not a name, or the name isn't `getattr`, but IDK, that seems not great / worthwhile.

---

_@charliermarsh reviewed on 2024-04-15 11:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:72 on 2024-04-15 11:50_

These things (like `is_identifier`) _do_ show up in traces sometimes because we tend to be calling these rules for _every_ function call, which is why being able to gate on something as cheap as "the name of the function" is useful.

---

_@charliermarsh reviewed on 2024-04-15 11:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:72 on 2024-04-15 11:50_

I don't think we need to change anything here but wanted to raise visibility on it.

---

_@charliermarsh reviewed on 2024-04-15 11:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:265 on 2024-04-15 11:52_

Me too :)

---

_@charliermarsh approved on 2024-04-15 11:53_

---

_@AlexWaygood reviewed on 2024-04-15 11:53_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:265 on 2024-04-15 11:53_

I'll leave this as-is for now; the status quo wins ;)

---

_Comment by @AlexWaygood on 2024-04-15 11:56_

Benchmarks are now showing an... improvement? Not sure how that's possible, but I'll take it https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:improve-builtins-handling

---

_@AlexWaygood reviewed on 2024-04-15 11:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/mutable_fromkeys_value.rs`:82 on 2024-04-15 11:59_

I'm not sure how we would be able to use `lookup_symbol` here, as `lookup_symbol` takes `&str` (representing the name of the symbol) rather than an AST node. And we don't know the name of the symbol -- even if it's a builtin, as you can do `from builtins import open as o` -- unless we resolve the qualified name

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:295 on 2024-04-15 12:57_

I think https://github.com/astral-sh/ruff/pull/10919/commits/4f627176cbc3a3b55787b9906c02d6bfdd3820bf goes some way to addressing this comment, though I haven't implemented your suggestion in exactly the way you suggested. What do you think of that solution?

---

_@AlexWaygood reviewed on 2024-04-15 12:57_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-04-15 12:58_

---

_@MichaReiser reviewed on 2024-04-16 10:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:295 on 2024-04-16 10:32_

I like it.

---

_Merged by @AlexWaygood on 2024-04-16 10:37_

---

_Closed by @AlexWaygood on 2024-04-16 10:37_

---

_Branch deleted on 2024-04-16 10:37_

---
