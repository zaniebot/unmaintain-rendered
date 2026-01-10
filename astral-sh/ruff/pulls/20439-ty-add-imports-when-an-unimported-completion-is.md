```yaml
number: 20439
title: "[ty] Add imports when an unimported completion is selected"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/refactor-importer
created_at: 2025-09-16T17:44:05Z
updated_at: 2025-10-28T15:57:29Z
url: https://github.com/astral-sh/ruff/pull/20439
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Add imports when an unimported completion is selected

---

_Pull request opened by @BurntSushi on 2025-09-16 17:44_

This PR does the work necessary to make the "import"
part of "auto-import" work. That is, when an unimported
completion is found, it is now returned with an LSP text
edit for inserting an import that should bring that symbol
into scope.

The diffstat here is quite large, but a bulk of that are
tests. And this PR should be reviewed commit-by-commit.
Many of the commits are somewhat smaller refactorings.
The last commit is where the new `Importer` abstraction is
introduced, and it's where (arguably) most of the weeds are.

For now, I've not concerned myself with performance in order
to avoid getting distracted by it. In particular, every
unimported completion gets an edit generated for it. This
strikes me as sub-optimal, and indeed, it looks like the
LSP protocol has support for running "commands" in response
to a completion being selected. Future work might involve
taking advantage of that, but it's not clear to me yet how
expensive (relatively speaking) generating these edits
actually is.


---

_Review requested from @carljm by @BurntSushi on 2025-09-16 17:44_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-09-16 17:44_

---

_Review requested from @sharkdp by @BurntSushi on 2025-09-16 17:44_

---

_Review requested from @dcreager by @BurntSushi on 2025-09-16 17:44_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-09-16 17:44_

---

_Review request for @dcreager removed by @BurntSushi on 2025-09-16 17:44_

---

_Review request for @carljm removed by @BurntSushi on 2025-09-16 17:44_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-09-16 17:44_

---

_Comment by @BurntSushi on 2025-09-16 17:45_

Demo:

https://github.com/user-attachments/assets/e381082a-bc82-4f7e-b21b-e9f4a094a3cc



---

_Comment by @github-actions[bot] on 2025-09-16 17:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-16 17:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-16 17:58_

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

_Label `server` added by @BurntSushi on 2025-09-16 18:23_

---

_Label `ty` added by @BurntSushi on 2025-09-16 18:23_

---

_@BurntSushi reviewed on 2025-09-16 18:37_

---

_Review comment by @BurntSushi on `crates/ruff_python_importer/src/insertion.rs`:180 on 2025-09-16 18:37_

This is how I avoided using `libcst`. I believe this is the only use of libcst in ruff's existing importer abstraction: https://github.com/astral-sh/ruff/blob/aa63c24b8f4a82f5f54dc90c4e6b5adadbf3e1b2/crates/ruff_linter/src/importer/mod.rs#L470-L488

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/edit.rs`:69 on 2025-09-17 08:01_

Is this only so that you don't need to import `Ranged` to call `Range` because `Edit` implements `Ranged` (and, therefore, has a `edit.range` method)

---

_Review comment by @MichaReiser on `crates/ruff_source_file/src/locator.rs`:16 on 2025-09-17 08:08_

Hmm, I was hoping we could eventually remove `Locator`. What I dislike about the type is that its lazy computation of the index is a bit of a footgun (which is why all those methods are marked as deprecated). 

But `Locator` isn't much more than a `&str` if you remove all methods that don't use a `LineIndex` (`&str` also implements `LineRanges`). The only difference are the helper methods like `after`, `before` that work with `TextRanges`. 

I don't think it's worth spending too much time but is there maybe a way we could decouple `Importer` from `Locator` (e.g. by just using a `&str` and exposing some of those helper methods with a different trait on `&str` which I think could be useful in other places too)

---

_Review comment by @MichaReiser on `crates/ruff_python_importer/src/lib.rs`:15 on 2025-09-17 08:12_

The `convert_edit` calls confused me a fair bit because I didn't realize that there are now two `Edit` types. 

Given that `ruff_diagnostics` has very few dependencies, could we just reuse that type instead of duplicating its definition? 

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/stylist.rs`:44 on 2025-09-17 08:21_

Neat! 

But I'm not sure if this is actually needed, given that we only need `into_owned` in tests and only in tests that call `import` (or `import_from`) of which, I think, most tests only contain exactly one call. 

I'm leaning towards creating the `Stylist` ad-hoc on demand. The only downside is that we compute the `line_ending` a few more times than necessary but that should be neglectable, given that most test files have very short lines (it only searches for the first line ending)

---

_Review comment by @MichaReiser on `crates/ruff_python_importer/src/insertion.rs`:180 on 2025-09-17 08:25_

I'm only mid-through reviewing this PR and I'm going commit-by-commit as suggested so I don't know if you did this in a later commit.

I suggest adding extensive testing for this function, specifically in the presence of comments. 

```py
from collections import (
	Collector # comment
)

from collections import (
	Collector, # comment
)

from collections import (
	Collector # comment
    ,
)

from collections import (
	Collector
    # comment
    ,
)
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:62 on 2025-09-17 08:30_

@sharkdp do you see any issues with this?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:68 on 2025-09-17 08:35_

Given that this invariant exists, should we remove file and instead use `module.file(db)` to get the file or do we want to keep `file` because it is guaranteed that `Module` is never a `NamespacePackage`?

Another alternative would be to use `file_to_module(db, file)`, but I guess this has the same downside that it requires you to handle the `None` case

---

_Review comment by @MichaReiser on `crates/ty_wasm/src/lib.rs`:428 on 2025-09-17 08:41_

Would you mind adding the new fields here too (and wire them up in the playground?)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/importer.rs`:15 on 2025-09-17 08:43_

We should consider this very carefully, given that the `SemanticIndex` is constructed for every file but adding imports is only used very rarely. I think re-walking the tree here is perfectly fine (and should be very fast too).

An alternative is to introduce a `top_level_imports` query that caches the result but I don't think that's worth it because it needs to re-run on every AST change anyway

---

_Review comment by @MichaReiser on `crates/ty_ide/src/importer.rs`:67 on 2025-09-17 08:44_

Didn't you abstract away the only libcst usage (which would be great, because libcst is pretty large and chunky to build)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/importer.rs`:142 on 2025-09-17 08:47_

Should this documentation be moved to `members.at`?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/importer.rs`:232 on 2025-09-17 08:49_

```suggestion
            // sorted by their start location in the source.
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/importer.rs`:239 on 2025-09-17 08:49_

```suggestion
                if choice
                    .is_none_or(|c| !c.kind.is_prioritized_over(&response.kind))
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/importer.rs`:726 on 2025-09-17 09:00_

I think we'll want to support those for type-checking only imports. I think it's totally fine that this PR ignores those, but we should think about when we should add support for them. 

Unlike Rust, importing a module has a runtime cost. Because of that, Python supports type-checking only imports, to avoid importing modules that are only needed to type some API. 

The way type-checking only imports work in Python is that the imports are gated by an `if TYPE_CHECKING` guard (it's a well-known constant by type checkers or it can be imported from the typing module):

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import foo
```

Now, symbols imported in a `TYPE_CHECKING` block aren't available at runtime. That means, they should only be suggested when in a type position (and e.g. not in the condition of an `if` statement). 

It's also preferred to add the imports to an `if TYPE_CHECKING` block if the import is only used for type checking. 

I don't know how far existing LSPs go here but Ruff has a few rules to help with this: https://docs.astral.sh/ruff/rules/#flake8-type-checking-tc



---

_Review comment by @MichaReiser on `crates/ty_ide/src/importer.rs`:711 on 2025-09-17 09:02_

You could consider implementing `SourceOrderVisitor` instead to remove the need to sort the import statements after visiting because the ordering is already guaranteed by the visitor. 

The only drawback is that there's no statements only equivalent of it but it should be relatively easy to skip unnecessary nodes by overriding `enter_node`

---

_Review comment by @MichaReiser on `crates/ty_ide/src/importer.rs`:1356 on 2025-09-17 09:05_

I think there's a third option to alias the import. That's probably what I would expect but I also think it's fine to leave this as is for now

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_model.rs`:247 on 2025-09-17 09:07_

Can we remove the TODO here, considering that it's repeated in `scope`?

---

_@MichaReiser approved on 2025-09-17 09:09_

This is great! 

I think it would be nice to make use of the LSPs lazy resolution of completion metadata (e.g. lazily resolve the `Type` of a `Completion) but I agree with your scoping that we should leave this to a separate improvement. 

We probably also want some more advanced sorting of completion items to e.g. use the closest (and most high-level) re-export of a symbol over importing it from a very deeply nested submodule (or from a test!). But again, these are incremental improvements that we can make later. 

---

_Review comment by @AlexWaygood on `crates/ruff_python_importer/src/lib.rs`:2 on 2025-09-17 09:37_

```suggestion
Low-level helpers for manipulating Python import statements.
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_importer/src/lib.rs`:73 on 2025-09-17 09:38_

```suggestion
    /// Returns `None` if this edit is a deletion.
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:7 on 2025-09-17 09:42_

```suggestion
Both of them use the lower-level `ruff_python_importer::{Edit, Insertion}`
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:10 on 2025-09-17 09:42_

```suggestion
1. This works with ty's semantic model instead of ruff's.
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:15 on 2025-09-17 09:42_

```suggestion
   `Importer` into ty's `SemanticIndex`.
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:150 on 2025-09-17 09:49_

```suggestion
    /// request's `member`, it can be different. For example, there
    /// might be an alias, or the corresponding module might already
    /// be imported in a qualified way.
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:172 on 2025-09-17 09:49_

```suggestion
                // As long as it's not a wildcard import, we use whatever name
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:260 on 2025-09-17 09:51_

```suggestion
/// [`Importer::members_in_scope_at`] in order to use the [`Importer::import`] API.
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:526 on 2025-09-17 09:54_

```suggestion
                // ...unless the symbol we want is already
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:530 on 2025-09-17 09:55_

```suggestion
            // ...unless the module symbol we found here is
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:547 on 2025-09-17 09:55_

```suggestion
            // symbol is definitely in scope, we attempt a
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:598 on 2025-09-17 10:02_

```suggestion
/// statement satisfy an [`ImportRequest`]? This encodes the different
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:632 on 2025-09-17 10:08_

```suggestion
    /// It is guaranteed that this never contains a wildcard import
    /// (otherwise, this import wouldn't be partial).
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:1019 on 2025-09-17 10:33_

the motivation there is almost certainly so that a snapshot that includes paths in some ways will pass on both Unix and Windows systems. But it feels like we probably shouldn't have that filter enabled on this snapshot assertion?

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:1240 on 2025-09-17 10:34_

```suggestion
        // we add another import at the top level
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:1266 on 2025-09-17 10:35_

```suggestion
        // we add another import at the top level
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:1369 on 2025-09-17 10:36_

```suggestion
    /// prompt the user for what they want to do. But I think
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/lib.rs`:358 on 2025-09-17 10:37_

```suggestion
    /// The file, and the offset into that file where the `<CURSOR>` marker is located.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:277 on 2025-09-17 10:39_

As @carljm said in https://github.com/astral-sh/ruff/pull/18455#discussion_r2126833137, I think we should just remove this TODO. Using a non-panicking method here seems better in the long term; I don't think we should ever change this.

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_importer/src/insertion.rs`:154 on 2025-09-17 10:41_

```suggestion
    /// newline terminator. Callers can then call [`Insertion::into_edit`]
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_importer/src/insertion.rs`:166 on 2025-09-17 10:41_

```suggestion
        // ...however, unless we can be certain that
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_importer/src/lib.rs`:35 on 2025-09-17 10:42_

```suggestion
    /// [`Edit::deletion`] with the given range.
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_importer/src/lib.rs`:51 on 2025-09-17 10:42_

```suggestion
    /// [`Edit::deletion`] with the given range.
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_importer/src/lib.rs`:81 on 2025-09-17 10:43_

Or you could implement `Ranged` for `Edit`? Then you'd also get `start()` and `end()` methods for free

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:47 on 2025-09-17 10:44_

```suggestion
    /// may be cheaper to compute at scale (e.g., for
    /// unimported symbol completions).
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:49 on 2025-09-17 10:44_

```suggestion
    /// Callers should use [`Completion::kind`] to get the
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:66 on 2025-09-17 10:47_

Just to double-check: you're using "builtins" here to refer to the `builtins` module/namespace in the stdlib rather than "any stdlib module from our vendored typeshed", right?

(If so, you're using the term correctly, but I've seen it used incorrectly in some places in ty, so I'm just double-checking ;) )

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:165 on 2025-09-17 10:48_

```suggestion
/// The idea here is that [`Completion::kind`] defines the mapping to this from
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:278 on 2025-09-17 10:49_

```suggestion
        // FIXME: `all_symbols` doesn't account for wildcard imports. Since
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:57 on 2025-09-17 10:50_

```suggestion
    /// The [`Stylist`] dictates the code formatting options of any code
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:63 on 2025-09-17 10:50_

```suggestion
    /// The [`Locator`] is used to get access to the original source
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:101 on 2025-09-17 10:50_

```suggestion
    /// [`ruff_text_size::Ranged`] trait). However, in some cases, identifying
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:376 on 2025-09-17 10:51_

```suggestion
    /// [`ImportRequest`], but this may sometimes be fully qualified based on
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_name.rs`:279 on 2025-09-17 10:53_

```suggestion
    /// `importing_file` must be the [`File`] that contains the import
```

---

_@AlexWaygood reviewed on 2025-09-17 10:55_

This looks very exciting!!

The documentation you've added is (as usual) fantastic. I did spot a few typos reading through the comments, so I ended up doing a docs-only review pass (sorry if some comments here are a bit nitpicky!)

---

_Review comment by @BurntSushi on `crates/ruff_diagnostics/src/edit.rs`:69 on 2025-09-17 14:01_

Nope, just completely missed the `Ranged` impl. :P 

---

_Review comment by @BurntSushi on `crates/ruff_source_file/src/locator.rs`:16 on 2025-09-17 14:06_

Yeah I noticed this. `Locator` struck me as odd for the reasons you cite. But I was trying to find a balance between "make progress" and "refactor." :P 

The only reason I'm using it is because `Insertion` uses it.

But I think you're right. It looks like `Insertion` isn't using any of the methods that use the `LineIndex`. If not, I can refactor that.

---

_Review comment by @BurntSushi on `crates/ruff_source_file/src/locator.rs`:16 on 2025-09-17 14:45_

Done! I also moved `Locator` back into `ruff_linter`. _And_ I removed `Locator` from `ruff_linter`'s `Importer`.

---

_Review comment by @BurntSushi on `crates/ruff_python_importer/src/lib.rs`:15 on 2025-09-17 14:50_

I guess that's fine with me. I was trying to avoid bringing `ruff_diagnostics` into the ty world. And the type is very simple, so I don't think duplicating it is terrible. Moreover, I feel like `ruff_python_importer` should probably own the types that it returns, but that's a vague API design nit. But this is done now.

---

_Review comment by @BurntSushi on `crates/ruff_python_codegen/src/stylist.rs`:44 on 2025-09-17 15:01_

It's a little more subtle than that. An `Importer` wants a `&Stylist`. So if we create a `Stylist` on-demand, it can't be tied to the stack frame that creates an `Importer` _and_ returns that importer.

There are a lot of different ways around this. A `Stylist` could always own its source for example. Or the tests could always be required to create an `Importer` in the same stack frame (or above) as where it's used.

But I found this to be simple and it follows a standard API pattern in Rust libraries for creating borrowed/owned versions of the same type. And I don't think the cost here is that big.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:68 on 2025-09-17 15:04_

Yeah I kept both to avoid the unwraps and because I perceived there to be little-to-no cost in doing so. And if and when we ever refactor this to use namespace packages, the types will help a bit more I think since it will force us to confront the fact that a `File` may be missing.

---

_Review comment by @BurntSushi on `crates/ty_wasm/src/lib.rs`:428 on 2025-09-17 15:16_

Yes! Done.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:15 on 2025-09-17 15:19_

Yeah that is actually the same instinct that led me to _not_ try to add it to the semantic index. Here, I was only talking about doing so _if_ for some reason this was needed more broadly. But yeah, I can't think of why we would need it more broadly.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:67 on 2025-09-17 15:19_

Yeah I wrote this comment when I thought I wouldn't be able to do that. Fixed now. :-)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:142 on 2025-09-17 15:20_

Yup, done. It was already moved (with some edits), but I forgot to delete it here.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:239 on 2025-09-17 15:22_

Blech. Done.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:726 on 2025-09-17 15:24_

Yeah good call out! I thought about this when working on the importer, but the "how do you know when it should go into a `TYPE_CHECKING` block" seemed like it was big enough to be worth punting on for now. I've added this to my notes for completions.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:711 on 2025-09-17 15:30_

All righty. Seems sensible. Done.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:1356 on 2025-09-17 15:31_

Yeah I was talking about that with Zanie. It's definitely not clear to me that aliasing would be better... But we can wait for user feedback on this one. Either way, I think if we did want to go the alias route, it wouldn't be hard to bolt that on to what I have here.

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:247 on 2025-09-17 15:32_

Sure, done!

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:526 on 2025-09-17 15:37_

Hmmm I prefer the space after the `...` here. :-)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:547 on 2025-09-17 15:38_

I meant "definitively" here.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:1019 on 2025-09-17 15:42_

Hmmm. The filter here is

```
insta_settings.add_filter(r#"\\(\w\w|\s|\.|")"#, "/$1");
```

That `\s` is what threw me off here, since I don't think that usually helps with file paths. I'd guess that was probably copied from elsewhere.

In any case, just removing the `\s` alternation fixed things and makes this snapshot read better.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:1240 on 2025-09-17 15:44_

I think if `low-level` is supposed to be hyphenated then `top-level` ought to be as well.

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:277 on 2025-09-17 15:47_

Fair. Done!

---

_Review comment by @BurntSushi on `crates/ruff_python_importer/src/lib.rs`:81 on 2025-09-17 15:48_

Yeah I ended up just deleting this type.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:66 on 2025-09-17 15:50_

I believe so, yes. Specifically, however ty uses "builtin." `builtin` here is `true` when `module.is_known(self.db, KnownModule::Builtins)` is `true`.

---

_Review comment by @BurntSushi on `crates/ruff_python_importer/src/insertion.rs`:180 on 2025-09-17 17:07_

Done! I figured I was forgetting something obvious. Perhaps surprising, my implementation passes all the tests. (In the sense that it still generates valid Python source, although I wouldn't call the formatting ideal. Although perhaps some of your examples don't have ideal formatting in the first place.)

---

_@BurntSushi reviewed on 2025-09-17 17:09_

---

_Comment by @BurntSushi on 2025-09-17 17:59_

Going to bring this in, but I'm happy to address additional feedback in follow-ups. :-)

---

_Merged by @BurntSushi on 2025-09-17 17:59_

---

_Closed by @BurntSushi on 2025-09-17 17:59_

---

_Branch deleted on 2025-09-17 17:59_

---

_@AlexWaygood reviewed on 2025-09-17 18:11_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:1240 on 2025-09-17 18:11_

Unfortunately whether either "top-level" or "low-level" should be hyphenated or not depends on the context of whether it's being used as a compound adjective (but this is definitely not worth worrying about :-)

---

_@sharkdp reviewed on 2025-09-18 07:08_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:62 on 2025-09-18 07:08_

I only added the `single_declaration` field in the return type of `place_from_declarations` recently. We could probably do something similar and add a `single_binding` or `single_definition` field in the return type of `place_from_bindings`, so as to avoid the additional traversal here?

A case where this (and the `single_declaration` / `single_binding` fields) would fail to return a single definition would be:
```py
if flag:
    x = 1
else:
    x = 2

# there are two live bindings of `x` at the end of the scope here, each with its own definition
```

I haven't thought through the implications here (I don't know what we use the definition field for, downstream), but it's maybe worth at least adding some tests for?

---

_@sharkdp reviewed on 2025-09-18 07:09_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:62 on 2025-09-18 07:09_

*Somewhat* related discussion that describes another shortcoming of our `place_from_{declarations,bindings}` API: https://github.com/astral-sh/ty/issues/1051

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/ide_support.rs`:62 on 2025-09-18 13:12_

Yeah https://github.com/astral-sh/ty/issues/1051 looks very wise here.

I explored this a bit by adding several test cases in this PR: https://github.com/astral-sh/ruff/pull/20467

---

_@BurntSushi reviewed on 2025-09-18 13:12_

---

_Comment by @rushter on 2025-09-19 20:15_

> Demo:
> 
>  even-slicker-import-demo.mp4

What lsp feature displays the import path (green text)?

In helix, I just see suggested names without imports. They are from different modules and there is no way to tell which is correct.

<img width="1419" height="354" alt="image" src="https://github.com/user-attachments/assets/6f15f27d-1e0a-4544-b1fc-8bd15568e6eb" />


---

_Comment by @BurntSushi on 2025-09-19 20:28_

We put it in [`CompletionItemLabelDetails.detail`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItemLabelDetails). It does require LSP 3.17 though.

---

_Comment by @rushter on 2025-09-19 20:40_

> We put it in [`CompletionItemLabelDetails.detail`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItemLabelDetails). It does require LSP 3.17 though.

Weird, I think they do support it. I tested ty on `0.0.1-alpha.21`, not from GitHub branch.

https://github.com/helix-editor/helix/blob/0ae37dc52ba715100893c327414bcb1a1924a4c3/helix-lsp-types/src/completion.rs#L559


I can see them in other LSPs.

<img width="1396" height="481" alt="image" src="https://github.com/user-attachments/assets/db795ccc-da43-4b85-b830-3b68b358d99b" />


---

_Comment by @rushter on 2025-09-19 20:48_

I'm getting the same behavior in Zed as well. Will test it more tomorrow.

<img width="853" height="492" alt="image" src="https://github.com/user-attachments/assets/54d2ae9e-374e-4b89-9252-aee6cb300f8d" />


---

_Comment by @BurntSushi on 2025-09-19 20:55_

> Weird, I think they do support it. I tested ty on `0.0.1-alpha.21`, not from GitHub branch.

Hmmm, [this](https://github.com/astral-sh/ruff/pull/20466) might be related and I don't think made it into `0.0.1-alpha.21`? Although that shouldn't prevent it from showing up at all.

> Will test it more tomorrow.

Thank you!!! <3

> I can see them in other LSPs.

Hmmm that's not showing the import path though as far as I can see. It's just showing the kind. Although ty should be including the kind too, and that doesn't seem to be showing up for you either.

---

_Comment by @rushter on 2025-09-19 21:44_

> > Weird, I think they do support it. I tested ty on `0.0.1-alpha.21`, not from GitHub branch.
> 
> Hmmm, [this](https://github.com/astral-sh/ruff/pull/20466) might be related and I don't think made it into `0.0.1-alpha.21`? Although that shouldn't prevent it from showing up at all.
> 
> > Will test it more tomorrow.
> 
> Thank you!!! <3
> 
> > I can see them in other LSPs.
> 
> Hmmm that's not showing the import path though as far as I can see. It's just showing the kind. Although ty should be including the kind too, and that doesn't seem to be showing up for you either.

Installed from the main branch. Here is what I see in LSP logs of helix:

```
 {
      "additionalTextEdits": [
        {
          "newText": "from selectolax.parser import HTMLParser\n",
          "range": { "end": { "character": 0, "line": 7 }, "start": { "character": 0, "line": 7 } }
        }
      ],
      "insertText": "HTMLParser",
      "kind": 7,
      "label": "HTMLParser",
      "labelDetails": { "detail": " (import selectolax.parser)" },
      "sortText": " 56"
    },
    {
      "additionalTextEdits": [
        {
          "newText": "from html.parser import HTMLParser\n",
          "range": { "end": { "character": 0, "line": 7 }, "start": { "character": 0, "line": 7 } }
        }
      ],
      "insertText": "HTMLParser",
      "kind": 7,
      "label": "HTMLParser",
      "labelDetails": { "detail": " (import html.parser)" },
      "sortText": " 57"
    },
```
    
   So I guess label details are not fully supported. Not sure why kind does not work though...

---

_Comment by @rushter on 2025-09-20 10:00_

Checked how it's done in other LSP.s 
Rust analyzer just appends it to label:
```
        "label": "PatternMatchAs(use ruff_python_ast::PatternMatchAs)",
```
gopls puts it in the description (if it's a module import).:

```
"label": "unsafeheader",
"kind": 9,
 "detail": "\"internal/unsafeheader\"",
```


It looks like even Zed does not support labelDetails yet, so perhaps it's better to put it somewhere else for now.

---

_Comment by @BurntSushi on 2025-09-20 13:37_

Oh nice find! I missed that in r-a. I think you're right that stuffing it somewhere else if the LSP doesn't support `labelDetails` would be wise.

---

_Comment by @BurntSushi on 2025-10-28 15:35_

@rushter I didn't forget about your bug! It should be fixed in https://github.com/astral-sh/ruff/pull/21109 :-)

---

_Comment by @rushter on 2025-10-28 15:57_

> @rushter I didn't forget about your bug! It should be fixed in #21109 :-)

Thanks! I was checking the recent ty changes yesterday for autocomplete improvements :) 

---
