```yaml
number: 17784
title: "Convert `Message::SyntaxError` to use `Diagnostic` internally"
type: pull_request
state: merged
author: ntBre
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: brent/diagnostics-file-enum
created_at: 2025-05-01T23:51:16Z
updated_at: 2025-05-08T16:45:53Z
url: https://github.com/astral-sh/ruff/pull/17784
synced_at: 2026-01-12T15:56:05Z
```

# Convert `Message::SyntaxError` to use `Diagnostic` internally

---

_@ntBre_

## Summary

This PR is a first step toward integration of the new `Diagnostic` type into ruff. There are two main changes:
- A new `UnifiedFile` enum wrapping `File` for red-knot and a `SourceFile` for ruff
- ruff's `Message::SyntaxError` variant is now a `Diagnostic` instead of a `SyntaxErrorMessage`

The second of these changes was mostly just a proof of concept for the first, and it went pretty smoothly. Converting `DiagnosticMessage`s will be most of the work in replacing `Message` entirely.

## Test Plan

Existing tests, which show no changes.


---

_Comment by @github-actions[bot] on 2025-05-01 23:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-02 00:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:222 on 2025-05-02 06:49_

Can we use more telling variable names here 😅 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:633 on 2025-05-02 06:54_

Reparsing notebooks here seems somewhat fragile and returning an empty notebook will return in a panic when trying to lookup locations (that now all point into nowhere)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:51 on 2025-05-02 06:55_

Can't these just be regular functions?

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:701 on 2025-05-02 13:27_

FWIW, I think the main intent with `FileResolver` is that it would become a trait, and this would be how we'd handle Ruff using its own interning for files. i.e., Red Knot would pass its own Salsa-backed resolver, but Ruff would use its non-Salsa thing.

I think if you aren't doing the interning, then yeah, this API is probably fine and perhaps even slightly over-engineered. If you do still plan to do interning---and I think that will let you definitively make `UnifiedFile` and therefore `Span` implement `Copy`---then I think this is a good stepping stone toward that.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/message/mod.rs`:66 on 2025-05-02 13:29_

Should these macros be helper functions?

---

_@BurntSushi reviewed on 2025-05-02 13:30_

---

_@ntBre reviewed on 2025-05-02 18:34_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/mod.rs`:66 on 2025-05-02 18:34_

Yeah that's a good idea. I need to sort out some lifetime issues I introduced, but functions would be preferable.

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-03 21:37_

---

_Comment by @ntBre on 2025-05-06 17:57_

Okay, I think this is ready for review. The two main changes since y'all last had a look are:
- Removing the macros 
  I added a somewhat overly-specific `Diagnostic::expect_ruff_span` method that just unwraps a `primary_span` call but with a ruff-centric message. I think this should eventually go away when we  actually use `Diagnostic` for output, so hopefully it's okay temporarily. Otherwise I just inlined the macros. They were mostly just to consolidate `expect` messages.
- Removing the repeated `Notebook` parsing
  I added a comment to this effect in the code, but Ruff already does its own notebook handling when `Message`s are emitted. Thanks @MichaReiser for the suggestion there!

---

_Marked ready for review by @ntBre on 2025-05-06 17:57_

---

_Review requested from @carljm by @ntBre on 2025-05-06 17:57_

---

_Review requested from @AlexWaygood by @ntBre on 2025-05-06 17:57_

---

_Review requested from @sharkdp by @ntBre on 2025-05-06 17:57_

---

_Review requested from @dcreager by @ntBre on 2025-05-06 17:57_

---

_Review requested from @MichaReiser by @ntBre on 2025-05-06 17:57_

---

_Renamed from "Prototype `Diagnostic` refactor" to "Convert `Message::SyntaxError` to use new `Diagnostic`s internally" by @ntBre on 2025-05-06 17:58_

---

_Renamed from "Convert `Message::SyntaxError` to use new `Diagnostic`s internally" to "Convert `Message::SyntaxError` to use `Diagnostic` internally" by @ntBre on 2025-05-06 17:59_

---

_@carljm reviewed on 2025-05-06 21:32_

---

_Review comment by @carljm on `crates/ruff_db/src/diagnostic/mod.rs`:655 on 2025-05-06 21:32_

```suggestion
            UnifiedFile::Ty(_) => panic!("Expected a ruff file, found a ty file"),
```

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:209 on 2025-05-07 07:15_

This took me way too long to understand. I think we should simplify this to `.map(|span| span.file().expect_ty().path(db).as_str())` knowing that we are in ty code

We could simplify this further by moving `expect_ty` to `span` so that one can write

```rust
.map(|span| span.expect_ty_file().path(db).as_str())
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:243 on 2025-05-07 07:16_

I think this could just be `expect_span`. The name ruff span is confusing because there isn't a type that represents ar fuf span.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:701 on 2025-05-07 07:22_

I find having both `path` and `file_path` on `UnifiedFile` confusing because the only difference is whether they take a `db` or `FileResolver`, but that's not apparent from the method signature. 


One option is to make `FileResolver` a trait as @BurntSushi suggests and implement it for `Db` (and `Upcast`). We would then need to implement a similar struct for Ruff. 

Or we remove `file_path` (and all other methods that take `db`) and callers should instead call `expect_ty` and the `path` on the returned file. The renderer could `call resolver.path(file)` directly and we move this match into `resolver.path`. I think I sort of prefer this.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:631 on 2025-05-07 07:28_

```suggestion
    fn path<'a>(&'a self, resolver: &FileResolver<'a>) -> &'a str {
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:219 on 2025-05-07 07:29_

```suggestion
        resolver: &FileResolver<'a>,
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:185 on 2025-05-07 07:29_

```suggestion
        resolver: &FileResolver<'a>,
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:70 on 2025-05-07 07:33_

I'm a bit concerned about using `SourceText` here. The type is supposed to be the return type of the `source_text` query and most of the fields aren't relevant for you. I'd prefer a diagnostic specific type instead OR avoid constructing an intermediate representation. Can you point me to why we need to convert into `SourceText`?

I've a strong preference to not reuse `SourceText` for the diagnostic parts. I also don't think it's really needed because the renderer mainly uses `Input::as_source_code`. Could we store a `SourceCode` instead?

---

_@MichaReiser reviewed on 2025-05-07 07:33_

---

_Review request for @dcreager removed by @MichaReiser on 2025-05-07 07:33_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-05-07 07:33_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-05-07 07:33_

---

_@ntBre reviewed on 2025-05-07 13:21_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:655 on 2025-05-07 13:21_

Thank you! That should be the last one, I did a quick `rg -i 'red.knot'` to confirm.

---

_@ntBre reviewed on 2025-05-07 13:22_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:633 on 2025-05-07 13:22_

I think these should be resolved. (possibly left over from your first review of the draft?)

---

_@ntBre reviewed on 2025-05-07 13:29_

---

_Review comment by @ntBre on `crates/ruff_benchmark/benches/ty.rs`:209 on 2025-05-07 13:29_

Ah that's much better, thank you! `expect_ty_file` was more useful everywhere else `expect_ty` was currently used too.

---

_@ntBre reviewed on 2025-05-07 13:30_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:243 on 2025-05-07 13:30_

I wanted to put "ruff" in the `expect` message, but I agree. I'll rename it and update the message.

---

_@ntBre reviewed on 2025-05-07 13:45_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:631 on 2025-05-07 13:45_

I swear I tried this yesterday, and it wouldn't compile. Maybe that was one of the other cases because it worked now, thanks!

---

_@ntBre reviewed on 2025-05-07 13:49_

---

_Review comment by @ntBre on `crates/ruff_benchmark/benches/red_knot.rs`:222 on 2025-05-07 13:49_

Resolved by your other suggestion, fortunately!

---

_Comment by @ntBre on 2025-05-07 13:57_

I went ahead and pushed the minor changes, working on the `SourceText` and `FileResolver` stuff next.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/message/mod.rs`:80 on 2025-05-07 14:04_

It might be a little nicer to accept a `impl std::fmt::Display` here (which is what most of the `Diagnostic` APIs accept, at least, in spirit). Then you could avoid the various `err.to_string()` calls and just pass `err`.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:240 on 2025-05-07 14:06_

Can you add a comment documenting this? In particular, it would be good to state the panic conditions and the higher level motivation for why you might want to use this. (e.g., "When working with diagnostics in Ruff, the context requires that every diagnostic have a primary span.")

I think I'd also rename this to `expect_primary_span()` so that it's consistent with `primary_span()`.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:693 on 2025-05-07 14:07_

Can you add docs to this type? This might be a nice place to put some prose about why this exists and the high level strategy for how we're trying to share the diagnostics system between both ty and ruff.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:701 on 2025-05-07 14:15_

I don't feel too strongly here primarily because `path()` is an internal API, so I think the blast radius of confusion is somewhat more limited.

I'm happy with either of @MichaReiser's suggestions though. I kinda like just jumping straight to the trait approach, which was the original intent.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/source.rs`:70 on 2025-05-07 14:34_

I think using just `SourceCode` might be a little tricky here, because you need somewhere to anchor its lifetimes to.

I do agree that reusing `SourceText` here is a little wonky.

Maybe the way to go here is another enum:

```rust
enum DiagnosticSource {
    Ty(SourceText),
    Ruff(SourceFile),
)
```

And then you should be able to define a method to return a `SourceCode` I think.

---

_@BurntSushi reviewed on 2025-05-07 14:35_

Looking pretty good!

---

_@ntBre reviewed on 2025-05-07 15:52_

---

_Review comment by @ntBre on `crates/ruff_db/src/source.rs`:70 on 2025-05-07 15:52_

I went with 

```rust
enum DiagnosticSource {
    Ty(Input),
    Ruff(SourceFile),
}
```

since we need both the `SourceText` and the `LineIndex` from `Input` to implement `as_source_code`. I also tried using `SourceCode` directly and immediately ran into the tricky lifetimes Andrew mentioned!

I guess another option would be to use `Span::expect_ty_file` for now and revisit this when we actually start rendering diagnostics from ruff.

---

_@ntBre reviewed on 2025-05-07 18:05_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:701 on 2025-05-07 18:05_

Is this close to what y'all had in mind? [1faa5e0](https://github.com/astral-sh/ruff/pull/17784/commits/1faa5e0aec829af1bca4f176315b692b9a34676e)

I feel like I may have gone astray here in the details, but `FileResolver` is now a trait, the code compiles, and I got rid of `file_path`, at least.

The docs on `FileResolver` are probably slightly outdated now, but they still sounded relevant enough to me to leave alone since we're not really fully decoupled from salsa yet.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:701 on 2025-05-07 18:39_

Yeah that looks about right to me! Thanks!

I'd probably go for `dyn Trait` here though since I don't think the virtual call is going to matter perf-wise, and it will hopefully save from duplicating the rendering code.

---

_@BurntSushi reviewed on 2025-05-07 18:39_

---

_Comment by @MichaReiser on 2025-05-08 07:53_

@ntBre can you ping me when this is ready for re-review (it is already)?

---

_Comment by @ntBre on 2025-05-08 12:43_

It should be ready for another review!

---

_Review requested from @MichaReiser by @ntBre on 2025-05-08 12:43_

---

_Review requested from @BurntSushi by @ntBre on 2025-05-08 12:43_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:693 on 2025-05-08 13:19_

Nice, much better!

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:724 on 2025-05-08 13:19_

Nice docs. And I like this type. Much clearer.

---

_@BurntSushi approved on 2025-05-08 13:20_

LGTM!

---

_@MichaReiser approved on 2025-05-08 16:43_

This is excellent. Thank you

---

_Comment by @ntBre on 2025-05-08 16:45_

Thank you both!

---

_Merged by @ntBre on 2025-05-08 16:45_

---

_Closed by @ntBre on 2025-05-08 16:45_

---

_Branch deleted on 2025-05-08 16:45_

---
