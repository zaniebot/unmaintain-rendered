```yaml
number: 17804
title: "[syntax-errors] detect duplicate keyword arguments"
type: pull_request
state: open
author: eduardorittner
labels: []
assignees: []
base: main
head: dup-keyargs
created_at: 2025-05-03T01:28:58Z
updated_at: 2025-05-14T23:21:49Z
url: https://github.com/astral-sh/ruff/pull/17804
synced_at: 2026-01-10T18:51:01Z
```

# [syntax-errors] detect duplicate keyword arguments

---

_Pull request opened by @eduardorittner on 2025-05-03 01:28_

## Summary

Part of https://github.com/astral-sh/ruff/issues/17412

Detects duplicate keyword arguments in function call.

## Test Plan

Added inline tests to validate intended behavior


---

_Review requested from @MichaReiser by @eduardorittner on 2025-05-03 01:28_

---

_Review requested from @dhruvmanila by @eduardorittner on 2025-05-03 01:28_

---

_Review requested from @ntBre by @MichaReiser on 2025-05-03 14:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-03 14:26_

Nit: it would be nice if we can avoid allocating a hash set if we know that there are fewer than 2 keyword arguments. 

```rust
let mut keyword_arguments = args.keywords.iter().filter_map(|key| &key.arg).peekable();

let Some(first) = keyword_arguments.next() else {
	return;
};


if keyword_arguments.peek().is_none() {
	return;
}

let mut unique_args = FxHashSet::default();
unique_args.insert(first);

for arg in keyword_arguments {
	if !unique_keyword_args.insert(ident) {
	  let range = ident.range();
	  // test_err duplicate_keyword_args
	  // def foo(x): ...
	  // foo(x=1, x=2)
	  // def baz(x, y, z): ...
	  // baz(x, y=1, z=3, y=4)
	
	  // test_ok non_duplicate_keyword_args
	  // def foo(x): ...
	  // foo(x=1)
	  // def bar(x, y, z): ...
	  // foo(x="a", y=1, z=True)
	  Self::add_error(
	      ctx,
	      SemanticSyntaxErrorKind::DuplicateKeywordArgs(ident.to_string()),
	      range,
	  );
	}
}
```

Another alternative would be that we make the `FxHashSet` a field on the visitor and reuse it (we'd have to make sure that we always call `clear()`). This would ensure that we only allocate at most one hash set.  I think I'd prefer that (because we could use it for other checks too)



---

_@MichaReiser approved on 2025-05-03 14:26_

I've a small perf comment but this otherwise looks good to me. I'll leave it to @ntBre to approve

---

_Review requested from @MichaReiser by @MichaReiser on 2025-05-03 14:26_

---

_Comment by @github-actions[bot] on 2025-05-03 14:35_

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

_@eduardorittner reviewed on 2025-05-03 20:31_

---

_Review comment by @eduardorittner on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-03 20:31_

yeah that makes sense! By visitor do you mean `SemanticSyntaxChecker` or one of `InvalidExpressionVisitor`, `ReboundComprehensionVisitor` or `MatchPatternVisitor`?

---

_Comment by @eduardorittner on 2025-05-04 13:31_

It looks like there already is an error type for duplicate keyword arguments in red_knot. I ran into this when adding integration tests into `semantic_syntax_errors.md` following https://github.com/astral-sh/ruff/issues/17526. The tests trigger the `ParameterAlreadyAssigned` error instead of the new `DuplicateKeywordArgs`.

https://github.com/astral-sh/ruff/blob/fe4051b2e6bf74a33f67f60fdbd775a6568e96a3/crates/ty_python_semantic/src/types/call/bind.rs#L1480-L1484

How should we proceed?

---

_@ntBre reviewed on 2025-05-05 14:25_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-05 14:25_

I think Micha means `SemanticSyntaxChecker`, which would also require passing a `&mut self` parameter to this function.

---

_@ntBre approved on 2025-05-05 14:43_

Thanks, this looks good to me!

> It looks like there already is an error type for duplicate keyword arguments in red_knot. I ran into this when adding integration tests into semantic_syntax_errors.md following https://github.com/astral-sh/ruff/issues/17526. The tests trigger the ParameterAlreadyAssigned error instead of the new DuplicateKeywordArgs.

I think we may want to remove the other check and emit this as a syntax error. What do you think, @MichaReiser?

---

_Comment by @MichaReiser on 2025-05-05 16:10_

I don't think we can simply replace them because ty checks more. For example:

```py
# error: 13 [missing-argument] "No argument provided for required parameter `x` of function `f`"
# error: 18 [parameter-already-assigned] "Multiple values provided for parameter `x` of function `f`"
reveal_type(f(1, x=2))  # revealed: int
```

I think we have two options here:

* We skip this check for ty
* We change the check in ty to not emit a diagnostic for the simple case where `x` is repeated on the call site

@dcreager what's your take on this. You probably know the call checking in ty the best

---

_Comment by @ntBre on 2025-05-05 16:23_

Ah I see, and that case (`f(1, x=2)`) is not actually a syntax error, so we can't just extend the semantic error check either. I'm happy with either of those options, whichever the ty team prefers!

We could just add a filter here, if that's the approach we want to take:

https://github.com/astral-sh/ruff/blob/ea62fc9baf2178deb0e761da11d64e7dff9c7c72/crates/ty_python_semantic/src/types.rs#L97-L102

---

_Comment by @MichaReiser on 2025-05-05 16:43_

My preference (because of consistency) would be to change the check during type inference to skip over errors that are known syntax errors. Unless this is hard for some reason 

---

_@eduardorittner reviewed on 2025-05-05 22:56_

---

_Review comment by @eduardorittner on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-05 22:56_

Yeah that's what I was wondering, in this case if we wanted to avoid `clone`, we'd have a `HashSet<&Identifier>` which means adding a lifetime to `SemanticSyntaxChecker` and most of the places it's used. The borrow checker was giving me some trouble but I'll take another shot at it

---

_@eduardorittner reviewed on 2025-05-07 12:26_

---

_Review comment by @eduardorittner on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-07 12:26_

I got it to compile using unsafe like this

```rust
    fn duplicate_keyword_args<Ctx: SemanticSyntaxContext>(
        &'a mut self,
        args: &ast::Arguments,
        ctx: &Ctx,
    ) {
        let args: &'a ast::Arguments = unsafe { std::mem::transmute(args) };

        for key in &args.keywords {
            if let Some(ident) = &key.arg {
                if !self.keyword_args.insert(ident) {
                    let range = ident.range();

                    Self::add_error(
                        ctx,
                        SemanticSyntaxErrorKind::DuplicateKeywordArgs(ident.to_string()),
                        range,
                    );
                }
            }
        }

        self.keyword_args.clear();
    }
```

The problem is that the borrow checker doesn't understand that calling `clear()` at the end of the function invalidates the references to `&Arguments` stored in the HashSet, and since mutable references are invariant this leads it to believe that `&Arguments` must outlive `&mut self`. We know that isn't necessary since any reference to `&Arguments` will always be cleared at the end of the function, so I used unsafe to coerce the `&Arguments` lifetime into the same as `&mut self`.

The problem with this approach is that
1. We are using unsafe, even though it seems pretty "safe" here, I'm not sure what ruff's stance to using unsafe is.
2. This raises lifetime errors in `tests/fixtures.rs`, these can probably be solved but would still require some work.

The other options I can think of are:
1. Leaving it as is, allocating and deallocating a hashset on every function call
2. Reuse the hashset, but make it a `FxHashSet<ast::Identifier>` and call `clone()` on every argument

Both of these options allocate more memory than necessary, though I'm not sure which is costlier (though I think `clone`ing would most times be cheaper I haven't actually tested it).

Let me know what your thoughts are.

---

_@MichaReiser reviewed on 2025-05-07 12:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-07 12:36_

The idea is to use something along these lines

```rust
let mut hash_set = std::mem::take(&mut self.shared_hash_set);

for key in &args.keywords {
	...
}

hash_set.clear();
self.shared_hash_set = hash_set;
```

but I wouldn't try to hard if the borrow checker is giving you a hard time.

---

_@eduardorittner reviewed on 2025-05-09 01:12_

---

_Review comment by @eduardorittner on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-09 01:12_

Funny enough I did try something similar to this but couldn't get it to work, but now it worked, thanks! Now I should just need to add some lifetime annotations where `SemanticSyntaxChecker` is used elsewhere in the code base

---

_@eduardorittner reviewed on 2025-05-09 21:04_

---

_Review comment by @eduardorittner on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-09 21:04_

Yeah I mean I can't do it, changing this lifetime raised a bunch of lifetime errors (close to 80) on other parts of the codebase, mostly the `Visitor` implementations on `with_semantic_checker` calls. Maybe there's a way to fix them using some specific annotations, but I couldn't figure it out. So I'll leave it like this for now.

---

_@ntBre reviewed on 2025-05-12 20:22_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-12 20:22_

Thanks for trying. I think this is okay. [codspeed](https://codspeed.io/astral-sh/ruff/branches/eduardorittner%3Adup-keyargs) is only showing a few ~1% performance regressions, which I think is pretty typical for any new rule. I think we just need to resolve the question about the duplicate diagnostic (and the clippy errors) and then we can land this.

---

_@ntBre reviewed on 2025-05-12 20:26_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-12 20:26_

It may not overlap 100%, but there is an open issue in ty (https://github.com/astral-sh/ty/issues/119) to avoid emitting type inference diagnostics in the presence of syntax errors, which might be enough to ignore the duplication issue for the sake of this PR.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:563 on 2025-05-13 06:33_

I think this goes somewhat beyond #119. At least, what I had in mind for it. The goal for #119 is to avoid type checker errors for nodes that have non semantic or version related syntax errors. 

---

_@MichaReiser reviewed on 2025-05-13 06:33_

---
