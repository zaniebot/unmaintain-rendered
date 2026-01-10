```yaml
number: 15943
title: "[red-knot] Make `Symbol::or_fall_back_to()` lazy"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/symbol-fallback-builder
created_at: 2025-02-04T17:31:55Z
updated_at: 2025-02-05T16:51:50Z
url: https://github.com/astral-sh/ruff/pull/15943
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Make `Symbol::or_fall_back_to()` lazy

---

_Pull request opened by @AlexWaygood on 2025-02-04 17:31_

## Summary

This PR allows us to conditionally chain different fallback symbols together. (The chain breaks as soon as we reach a symbol that is definitely bound.) The new API means we're lazy by default (I realized we were eagerly executing several `ModuleType::to_class_literal(db).member(db, name)` calls which would be immediately thrown away in several situations). Lastly, it means that in some situations we construct union types where the elements are arranged in a more intuitive order than we currently have on main.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-02-04 17:31_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-04 17:31_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-04 17:31_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-04 17:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-04 17:43_

Nit: I'm leaning towards calling this `or_else` 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:87 on 2025-02-04 17:47_

The `and` is slightly misleading` because it doesn't "and" any existing condition. E.g. the API suggests that `symbol.or_possibly_unbound(db).and(|| false).and(|| true)` would be possible where it isn't. 

I don't know a better naming or have a suggestion just now. I'd have to think it through in detail. How important is that we change this now or could we improve this later or just leave as is?


---

_@MichaReiser reviewed on 2025-02-04 17:48_

---

_@AlexWaygood reviewed on 2025-02-04 17:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-04 17:52_

The idea is that you can read the method chaining "like a sentence". E.g. the usage in `infer.rs` in this PR:

```rs
            Symbol::Unbound
                // No nonlocal binding? Check the module's globals.
                // Avoid infinite recursion if `self.scope` already is the module's global scope.
                .or_if_possibly_unbound(db)
                .and(|| !file_scope_id.is_global())
                .then_fall_back_to(|| global_symbol(db, self.file(), name))
                // Not found in globals? Fallback to builtins
                // (without infinite recursion if we're already in builtins.)
                .or_if_possibly_unbound(db)
                .and(|| Some(self.scope()) != builtins_module_scope(self.db()))
                .then_fall_back_to(|| builtins_symbol(self.db(), name))
                // Still not found? It might be `reveal_type`...
                .or_if_possibly_unbound(db)
                .and(|| name == "reveal_type")
                .then_fall_back_to(|| {
                    self.context.report_lint(
                        &UNDEFINED_REVEAL,
                        name_node.into(),
                        format_args!(
                            "`reveal_type` used without importing it; \
                        this is allowed for debugging convenience but will fail at runtime"
                        ),
                    );
                    typing_extensions_symbol(self.db(), name)
                })
```

---

_@MichaReiser reviewed on 2025-02-04 17:53_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-04 17:53_

Hmm. I find this very hard to parse. There's so much going on with nested lambdas and such. 

---

_@AlexWaygood reviewed on 2025-02-04 17:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-04 17:56_

that's a shame :( I find it much nicer than the `if`/`else` nesting we have on `main`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:87 on 2025-02-04 18:13_

> How important is that we change this now

it's not crucial, but:
1. I do think we're doing some things eagerly at the moment where we don't have to
2. I'm _slightly_ worried this unnecessary eagerness will matter more if I start doing TODOs like this, since I think instance attributes are more expensive to lookup than class attributes https://github.com/astral-sh/ruff/blob/64e64d26812601b840953a186d8e4d66da17594b/crates/red_knot_python_semantic/src/types.rs#L272-L274
3. I do find the nested `if`-conditions and early returns on `main` quite hard to follow in places like `types::infer::TypeInferenceBuidler::lookup_name()`, which this PR rewrites
4. I'm pretty interested in seeing if we could use builders like this in other places too: for example, falling back to different code paths depending on whether a dunder method is defined, possibly defined, or undefined. (I consider this PR linked to my `Outcome` refactor work; it's touching on similar themes.)

---

_@AlexWaygood reviewed on 2025-02-04 18:13_

---

_@MichaReiser reviewed on 2025-02-04 18:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-04 18:20_

I do like that it reads like a sentence but it also reads like a very long German sentence with three subsentences without any punctuation. The comments act as some sort of punctuation, but it is still very unclear where one filtering begins or ends and on what type these operations operate.

I also think that your current approach of returning some other struct that then exposes more methods is probably the right direction, similar to the `HashMap::entry` API. Maybe the returned value should be an enum? But I don't know how such an API would look specifically. It also reminds me of my [`ParsedSyntax`](https://github.com/biomejs/biome/blob/b35c9ed4910a1fca9eac2ac1dbed5b5067725efc/crates/biome_parser/src/parsed_syntax.rs#L169) API in Rome's parser.

Thanks for explaining your motivation below. Exploring it if it's related to your `Outcome` work is reasonable. I only want to avoid that we're persuing this just to make things "nicer" because it's currently rarely used and the eagerness isn't a problem today (as the neutral benchmarks show)

---

_@AlexWaygood reviewed on 2025-02-04 18:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-04 18:23_

> I do like that it reads like a sentence but it also reads like a very long German sentence with three subsentences without any punctuation. The comments act as some sort of punctuation, but it is still very unclear where one filtering begins or ends and on what type these operations operate.

Hmm, that sounds like an argument for my earlier approach in https://github.com/astral-sh/ruff/pull/15931 -- which feels less readable to me, and has deeper indentation but has the advantage that it's clearer where one filtering begins and ends

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-04 18:58_

What would you think about something like this?

```rs
            Symbol::Unbound
                // No nonlocal binding? Check the module's globals.
                // Avoid infinite recursion if `self.scope` already is the module's global scope.
                .with_possible_fallback(
                    db,
                    SymbolFallback {
                        fallback_if: || !file_scope_id.is_global(),
                        fallback_to: || global_symbol(db, self.file(), name),
                    },
                )
                // Not found in globals? Fallback to builtins
                // (without infinite recursion if we're already in builtins.)
                .with_possible_fallback(SymbolFallback {
                    fallback_if: || Some(self.scope()) != builtins_module_scope(db),
                    fallback_to: || builtins_symbol(db, name),
                })
                // Still not found? It might be `reveal_type`...
                .with_possible_fallback(SymbolFallback {
                    fallback_if: || name == "reveal_type",
                    fallback_to: || {
                        self.context.report_lint(
                            &UNDEFINED_REVEAL,
                            name_node.into(),
                            format_args!(
                                "`reveal_type` used without importing it; \
                            this is allowed for debugging convenience but will fail at runtime"
                            ),
                        );
                        typing_extensions_symbol(db, name)
                    },
                })
                .build()
```

---

_@AlexWaygood reviewed on 2025-02-04 18:58_

---

_@MichaReiser reviewed on 2025-02-05 07:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-05 07:18_

I think we don't need the `fallback_if`, or at least, we can use the normal `if` expression to accomplish it, the same as for `or_else` by returning the lower bound value (`Err`/`None`/`Unbound`):

```rust
Symbol::Unbound
	.or_else(|| {
		if file_scope_id.is_global() {
			Symbol::Unbound
		} else {
			global_symbol(db, self.file(), name)
		}
	})
	.or_else(|| {
		if builtins_module_scope(db) == Some(self.scope()) {
			Symbol::Unbound
		} else {
			builtins_symbol(db, name)
		}
	})
	.or_else(|| {
		if name == "reveal_type" {
			...
		} else  {
			Symbol::Unbound
		}
	})
```

I do prefer this because it removes the amount of lambdas and it also simplifies the case where there's no condition to just `Symbol::or_else(|| global_symbol(db, self.file(), name)`. 

However, I'm not sure if I'm happy with the name `or_else` because the operation doesn't to `a` or `b`, it does `a` or `b` and sometimes merge `a` and `b`... 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-05 13:13_

> I do prefer this because it removes the amount of lambdas

I considered this (in fact, it was the version I initially prototyped before filing any PRs), but decided against it. It feels very annoyingly repetitive that every time we use this method we have to have an `if`/`else` branch inside it. And the whole purpose of this PR is to move away from an imperative API and towards a declarative API where I can easily see from the code that Python's semantics are being implemented correctly without having to wade throught the visual noise of `if`/`else` everywhere.

I also don't much like that we have to explicitly encode that the fallback should be `Symbol::Unbound` every time we use the method. As well as being repetitive, this will make it harder to change the internal representation of `Symbol` in the future (e.g., if we had something more similar to a `Result`, where a fully bound type is `Ok()` but partially-bound and unbound are both `Err()` subvariants -- this is something I'm prototyping for my `Outcome` refactor). If we changed the `Symbol` representation like that, we'd have to change every use of these methods.

> it also simplifies the case where there's no condition

we currently have no places outside of unit tests where we would use this API without a condition.

---

Having said all of the above, however, I am okay with this as a compromise if you strongly dislike all my other suggestions. I do think it's better than the status quo on `main`.

---

_@AlexWaygood reviewed on 2025-02-05 13:13_

---

_@MichaReiser reviewed on 2025-02-05 13:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-05 13:20_

I would find it interesting to hear some other opinions but I do prefer this solution even if it's somewhat more verbose because it uses a pattern that's familiar even if you have never worked with `Symbol` and I find the explicit `if` and `Symbol::Unbound` easier to read than having to jump to the method declaration to understand what important details it is hidding from me. 

The solution to hide the nested `if`s would be to add a `bool.then_symbol` method but I consider it overkill, considering how rare this code pattern even appears. 

Overall, my preference is to move on. I feel like we're spending too much time on this.

---

_@AlexWaygood reviewed on 2025-02-05 13:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:104 on 2025-02-05 13:23_

Alright, I'll go with your suggestion as a compromise solution then

---

_Renamed from "[red-knot] Add a `SymbolFallbackBuilder` to allow symbol fallbacks to be elegantly chained together" to "[red-knot] Make `Symbol::or_fall_back_to()` lazy" by @AlexWaygood on 2025-02-05 14:46_

---

_Merged by @AlexWaygood on 2025-02-05 14:51_

---

_Closed by @AlexWaygood on 2025-02-05 14:51_

---

_Branch deleted on 2025-02-05 14:51_

---

_@carljm reviewed on 2025-02-05 16:51_

Nice!!

---
