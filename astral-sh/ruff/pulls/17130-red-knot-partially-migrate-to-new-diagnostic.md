```yaml
number: 17130
title: "[red-knot] partially migrate to new `Diagnostic` types, delete old diagnostic code"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/red-knot-new-diagnostics
created_at: 2025-04-01T18:59:24Z
updated_at: 2025-04-02T14:10:03Z
url: https://github.com/astral-sh/ruff/pull/17130
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] partially migrate to new `Diagnostic` types, delete old diagnostic code

---

_@BurntSushi_

This "surgically" replaces Red Knot's use of "old" diagnostic types
with the new `Diagnostic` type. It is surgical because it doesn't
change how Red Knot type checking actually creates diagnostics.
That is, the `InferContext` APIs to report diagnostics all remain
the same. What this PR does is change out the implementation of the
`InferContext` APIs to use the new `Diagnostic` types. This lays
the ground work for refactoring `InferContext` to expose the new
`Diagnostic` data model.

(I had originally planned for this refactor to include moving Red Knot
over to the new `Diagnostic` data model as well, but this got rather
big on its own. So I thought it would be better to split the work up.)

As discussed with @MichaReiser, this PR also removes the "fake" linear
typing for `Diagnostic`. That is, you'll no longer get a panic if you
drop a `Diagnostic` without printing it. It is thought that Salsa
specifically can make this akward to deal with, and there may be cases
in the future where diagnostics get dropped without us having an
opportunity to explicitly ignore them. In any case, while it is perhaps
still a good thing to have, it is not well motivated. Notably, Ruff
doesn't seem to have a problem of accidentally ignoring diagnostics. If
we do end up seeing this become a problem, we can think more carefully
about how to re-introduce the linear typing fakery.

Ref #16809


---

_Review requested from @carljm by @BurntSushi on 2025-04-01 18:59_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-01 18:59_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-01 18:59_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-01 18:59_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-01 18:59_

---

_Label `red-knot` added by @BurntSushi on 2025-04-01 18:59_

---

_Label `diagnostics` added by @BurntSushi on 2025-04-01 18:59_

---

_Comment by @github-actions[bot] on 2025-04-01 19:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-01 19:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/options.rs`:400 on 2025-04-02 06:52_

Should this be `into_diagnostic` to avoid cloning `self` or is it `to_diagnostic` because most salsa queries only return refs? 



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:208 on 2025-04-02 06:56_

I don't think this is necessary anymore, now that we removed the "drop bomb" from diagnostics.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:909 on 2025-04-02 07:04_

I'm not sure what the solution for this is but it may still be desired to wrap the `Diagnostic`s in `self.diagnostics` in an `Arc` because we now create multiple deep-clones of every diagnostic, which I suspect can be fairly expensive (diagnostics aren't lightweight structs). 

The diagnostics in Red Knot are collected as part of type inference and type inference is done at three different levels:

1. Per scope (calls into expressions and definition inference)
2. Per definition (class, function, variable), calls into expression inference
3. Per expression (only some expressions)

1. and 2. both have to "copy over" the diagnostics from the inner inferences operations. That means, a diagnostic created when infering an expression may be copied twice: Once to aggregate the diagnostics at the definition level and once when aggregating the diagnostics at a scope level. 

We later perform one last copy (clone) inside `check_types` 

This is why the old implementation used an `Arc` to make those clones as cheap as possible. Alternatives are to use `salsa::interned` (but that comes with extra bookkeeping but it has the advantage that it's arena allocated) or salsa accumulated (which now avoids cloning internally and only requires cloning at the boundaries)

Unless we say that cloning `Diagnostic`s isn't a performance concern relative to the cost of generating and printing them 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:5886 on 2025-04-02 07:06_

Nit
```suggestion
        self.types.diagnostics = self.context.finish();
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:327 on 2025-04-02 07:12_

I believe you mentioned in one of your reviews that it was a very intentional decision to implement `print` over `std::io::Write`. I don't remember the details but I noticed that it results in many places where it's necessary to use `Vec<u8>` in combination with a `String::from_utf8().unwrap` which is somewhat unfortunate. 

I don't really have a solution for this, it's just something I noticed (e.g. also in my tests).

---

_Review comment by @MichaReiser on `crates/red_knot_test/Cargo.toml`:16 on 2025-04-02 07:14_

It's not clear to me what changed that the `os` feature is now required or is this a feature we already missed declaring as dependency before? 

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:227 on 2025-04-02 07:16_

The `RefCell` might no longer be necessary, now that `Diagnostic::print` takes `&self`

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:280 on 2025-04-02 07:17_

You could also change the return type to `Result` without breaking the JS Api if you want to avoid the `unwrap` here

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:143 on 2025-04-02 07:18_


```suggestion
            .unwrap_or_default()
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:620 on 2025-04-02 07:19_

A use case for your `IntoDiagnostic` trait ;)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:909 on 2025-04-02 07:23_

Now that I'm through with my review: I see that you made `Diagnostic` use an `Arc` which addresses this concern. I liked that the "old" new diagnostic was salsa agnositc in the sense that it only used the `Arc` where necessary but I think this also makes sense. 

It does have the downside that Ruff's diagnostics get `Arc`ed where this isn't technically necessary. It will be interesting to see how this impacts performance.

---

_@MichaReiser approved on 2025-04-02 07:23_

Nice, thank you

---

_@BurntSushi reviewed on 2025-04-02 12:41_

---

_Review comment by @BurntSushi on `crates/red_knot_project/src/metadata/options.rs`:400 on 2025-04-02 12:41_

The `Diagnostic` API doesn't really support accepting ownership of things, so you have to clone anyway. For example:

```rust
    /// Attach a message to this annotation.
    ///
    /// An annotation without a message will still have a presence in
    /// rendering. In particular, it will highlight the span association with
    /// this annotation in some way.
    ///
    /// When a message is attached to an annotation, then it will be associated
    /// with the highlighted span in some way during rendering.
    pub fn message<'a>(self, message: impl std::fmt::Display + 'a) -> Annotation {
        let message = Some(message.to_string().into_boxed_str());
        Annotation { message, ..self }
    }
```

We may want to add alternative APIs to `Diagnostic` (and its constituent types) that accept ownership to avoid clones, but this isn't doing any more work AFAIK than the status quo:

https://github.com/astral-sh/ruff/blob/a15404a5c1f6319e511c7f2239c02a3ff8f4bf17/crates/red_knot_python_semantic/src/types/context.rs#L153-L160

I know we've been _talking_ about an `IntoDiagnostic` trait, but it might actually make more sense for it to be a `ToDiagnostic` trait. Or, alternatively, an `IntoDiagnostic` trait if we decide to add ownership consuming construction APIs to `Diagnostic`. I think I'd prefer that decision to be lead by profiling/benchmarking. (I guess this could matter in cases where Red Knot reports an avalanche of diagnostics. But even then, I'm skeptical that these sorts of copies are going to be anything but marginal compared to whatever other work Red Knot is doing in cases like that. But I'm mostly just guessing.)

---

_@BurntSushi reviewed on 2025-04-02 12:43_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/context.rs`:208 on 2025-04-02 12:43_

Oops! I thought I caught everything. This was actually scaffolding code I was using to aide the refactor. Nice catch. Removed.

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:909 on 2025-04-02 12:56_

Yeah I like sticking with `Arc` for now. I do this sort of thing all the time in my Rust libraries. It generally works very well.

You are of course correct that using `Arc` for these sorts of things isn't free. But I try to look at what the marginal cost of the `Arc` is. You're only getting a new `Arc` when you _create_ a diagnostic, and creating a diagnostic is already likely going to require allocs and formatting machinery and all that stuff.

I will be _very happy_ to eat crow and optimize if we can find use cases for which getting rid of these allocs helps perf. But I'll note that, at some point, you do have to actually _create_ strings somewhere in order to create a diagnostic, even if it had ownership accepting APIs. e.g., `Diagnostic::new(blah, blah, format_args!("some message about {foo}"))` has to do a `to_string()` on that `std::fmt::Arguments` _somewhere_. And unless we want a lifetime parameter infecting the `Diagnostic` types (which I would guess would be unworkable), it probably makes the most sense to do it at construction time.

I think even in alternative designs (e.g., a `Diagnostic` trait), there have to be clones and formatting happening somewhere, although precisely where might get shifted around a bit.

---

_@BurntSushi reviewed on 2025-04-02 12:56_

---

_@BurntSushi reviewed on 2025-04-02 12:59_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/lib.rs`:327 on 2025-04-02 12:59_

Yeah... right... good point. The reason was related to the linear type fakery. Namely, using `std::io::Write` as a destination makes it "more likely" that printing is actually going to stdout or similar, and not just some string in memory that could get accidentally ignored.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-02 13:13_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/lib.rs`:327 on 2025-04-02 13:15_

I don't think I have any strong feelings about reverting this back to just a `Display` impl or some such (the internal renderer already works that way, so it's an overall small change). One note though is that the `unwrap()` calls you're referencing I believe only occur in tests.

---

_@BurntSushi reviewed on 2025-04-02 13:15_

---

_@BurntSushi reviewed on 2025-04-02 13:16_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/lib.rs`:327 on 2025-04-02 13:16_

I'm leaning towards offering a `Display` impl. But might do that in a follow-up PR.

---

_@BurntSushi reviewed on 2025-04-02 13:17_

---

_Review comment by @BurntSushi on `crates/red_knot_test/Cargo.toml`:16 on 2025-04-02 13:17_

It was missed before. It came up when I was trying to run tests via `-p red_knot_test` I believe. Sorry, I meant to call this out in its own commit, but it slipped through.

---

_@BurntSushi reviewed on 2025-04-02 13:18_

---

_Review comment by @BurntSushi on `crates/red_knot_wasm/src/lib.rs`:227 on 2025-04-02 13:18_

Nice catch!

---

_@BurntSushi reviewed on 2025-04-02 13:19_

---

_Review comment by @BurntSushi on `crates/red_knot_wasm/src/lib.rs`:280 on 2025-04-02 13:19_

Ah, and this is a non-test instance of the `unwrap()` that I previously claimed was only in tests. Nice.

OK, that convinces me to switch back to a `Display` impl, kinda like how the old diagnostics work.

But I'll do that in a follow-up PR if that's okay with you. (Which will fix this too.)

---

_@BurntSushi reviewed on 2025-04-02 13:19_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:143 on 2025-04-02 13:19_

I'm surprised Clippy didn't catch this.

---

_@BurntSushi reviewed on 2025-04-02 13:21_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:620 on 2025-04-02 13:21_

Hmmm. I don't _think_ this on its own asks for generics. If it weren't for the dependency graph (which we briefly talked about fixing at the on-site), then this could "just" be a concrete method on `ParseError` and things would be fine.

---

_@BurntSushi reviewed on 2025-04-02 13:23_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/lib.rs`:327 on 2025-04-02 13:23_

> One note though is that the unwrap() calls you're referencing I believe only occur in tests.

This was wrong! See: https://github.com/astral-sh/ruff/pull/17130#discussion_r2024815794

---

_@MichaReiser reviewed on 2025-04-02 13:25_

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:280 on 2025-04-02 13:25_

Sure

---

_Merged by @BurntSushi on 2025-04-02 14:10_

---

_Closed by @BurntSushi on 2025-04-02 14:10_

---

_Branch deleted on 2025-04-02 14:10_

---
