```yaml
number: 16106
title: "[syntax-errors] Start detecting compile-time syntax errors"
type: pull_request
state: merged
author: ntBre
labels:
  - core
assignees: []
merged: true
base: main
head: brent/syntax-error-source-order
created_at: 2025-02-11T21:33:48Z
updated_at: 2025-03-21T18:45:28Z
url: https://github.com/astral-sh/ruff/pull/16106
synced_at: 2026-01-12T15:55:53Z
```

# [syntax-errors] Start detecting compile-time syntax errors

---

_@ntBre_

## Summary

This PR implements the "greeter" approach for checking the AST for syntax errors emitted by the CPython compiler. It introduces two main infrastructural changes to support all of the compile-time errors:
1. Adds a new `semantic_errors` module to the parser crate with public `SemanticSyntaxChecker` and `SemanticSyntaxError` types
2. Embeds a `SemanticSyntaxChecker` in the `ruff_linter::Checker` for checking these errors in ruff

As a proof of concept, it also implements detection of two syntax errors:
1. A reimplementation of [`late-future-import`](https://docs.astral.sh/ruff/rules/late-future-import/) (`F404`)
2. Detection of rebound comprehension iteration variables (https://github.com/astral-sh/ruff/issues/14395)

## Test plan
Existing F404 tests, new inline tests in the `ruff_python_parser` crate, and a linter CLI test showing an example of the `Message` output.

I also tested in VS Code, where `preview = false` and turning off syntax errors both disable the new errors:

![image](https://github.com/user-attachments/assets/cf453d95-04f7-484b-8440-cb812f29d45e)

And on the playground, where `preview = false` also disables the errors:

![image](https://github.com/user-attachments/assets/a97570c4-1efa-439f-9d99-a54487dd6064)


Fixes #14395

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:76 on 2025-02-11 21:36_

I don't think we should use another collection here. We should either push directly into `TypeCheckDiagnostics` or, what I'd prefer, move this to the semantic index phase. 

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:96 on 2025-02-11 21:39_

I don't think I'd implement `SourceOrderVisitor` here because you never call into `walk_stmt` or similar, so it sort of feels unnecessary. Instead, I'd only implement the methods that you plan on calling from `Checker` / Red Knot's visiting logic. For now, all you'd need is to have a `enter_stmt(&mut self, stmt: &Stmt)` function. 

The reason I recommend against implementing `SourceOrderVisitor` is because both the `Checker` and Red Knot's semantic index builder visit the tree in semantic and not source ordering. Using the `SourceOrderVisitor` trait here would give the wrong impression that the visiting happens in source order instead.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:68 on 2025-02-11 21:40_

Oh no, more `PythonVersion` types :( I'm somewhat inclined to move red knot's `PythonVersion` to `ruff_db` and reuse that instead (Ruff already depends on the `ruff_db` crate so adding that dependency won't hurt.

---

_@MichaReiser reviewed on 2025-02-11 21:43_

I haven't done an in depth review (as this is also a draft PR) but I left a few comments that hopefully help clarify things. 

Is the idea to emit version specific syntax errors (not semantic syntax errors) as part of the `SyntaxChecker` or do you still plan on emitting those as part of the parser?

I think we have to explore some alternative designs to decide on whether we want a rule code but it should definetely not block you from prototyping.

---

_Comment by @ntBre on 2025-02-11 22:20_

> I haven't done an in depth review (as this is also a draft PR) but I left a few comments that hopefully help clarify things.

Thanks Micha! Our conversation earlier was very helpful, and I think these comments clear up some places where I was still confused.

> Is the idea to emit version specific syntax errors (not semantic syntax errors) as part of the `SyntaxChecker` or do you still plan on emitting those as part of the parser?

In this draft, I tried emitting all of the errors, including version specific ones, in the `SyntaxChecker`, but I think your suggestion of keeping those in the parser still makes sense. Was your idea to keep the version-specific errors in the parser and use this `enter_stmt` approach just for the semantic errors? 

Detecting `LateFutureImport` felt a bit easier here than in the parser version, so I can see a place for both. On the other hand, I thought it might be nice to isolate all of these checks in the `SyntaxChecker` instead of mixing some into the parser, but some of the errors might only be detectable in the parser anyway.

> I think we have to explore some alternative designs to decide on whether we want a rule code but it should definetely not block you from prototyping.

I partially updated the design doc with your rule code suggestions from earlier, but I still need to add a more concrete proposal to help with this discussion. I'll do that tonight or first thing in the morning.


---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/types.rs`:76 on 2025-02-11 22:37_

Ahh okay, I knew something felt wrong about where I was putting this in red-knot. I will try moving it to the semantic index phase!

---

_@ntBre reviewed on 2025-02-11 22:37_

---

_@ntBre reviewed on 2025-02-11 22:40_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:96 on 2025-02-11 22:40_

That totally makes sense. I took the `SourceOrderVisitor` suggestion from the design doc too literally.

---

_Comment by @ntBre on 2025-02-11 22:43_

I haven't handled all of the review comments yet, but I rebased to get the new `Diagnostic` changes and hopefully fix the CI failures. I'm hoping this will be faster on codspeed than the other two prototypes too, or at least faster than the first one for sure.

---

_Comment by @github-actions[bot] on 2025-02-11 22:51_

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

_Comment by @MichaReiser on 2025-02-12 08:14_

> In this draft, I tried emitting all of the errors, including version specific ones, in the SyntaxChecker, but I think your suggestion of keeping those in the parser still makes sense. Was your idea to keep the version-specific errors in the parser and use this enter_stmt approach just for the semantic errors?

Exactly. My motivation for this is that:

* The parser already emits syntax errors today. So it feels seems like a good fit to emit the version specific syntax (not compile) errors in the parser too. 
* Our parser is used by other projects and I think it would be useful for them too if they can enforce a specific python version during parser without having to depend on the syntax-checker crate (which we'll likely remove if we unify Ruff/Red Knot)
* I would love if the formatter tests could assert that there are no syntax errors in the source and formatted file. Doing so is much easier if this is a capability of the formatter than compared to calling into some visitor. 
* I'm sort of okay with not documenting version specific syntax errors. I do think we may want to have some documentation for compile errors, at least long term. 



---

_Comment by @AlexWaygood on 2025-02-12 11:59_

> I'm sort of okay with not documenting version specific syntax errors. I do think we may want to have some documentation for compile errors, at least long term.

For most of these, I think we could get away without documentation. There's not that much to say about the `match` statement not being valid before Python 3.10, for example.

For some of them, though, I think docs would be really useful for our users. For example, the [syntax differences between Python 3.8 and 3.9 due to PEP 614](https://peps.python.org/pep-0614/) are pretty subtle. And the details on when exactly you're able to parenthesize context managers in `with` statements on Python 3.8 are also pretty complicated -- I can never remember what they are!

This is invalid syntax on Python 3.8:

```py
with (
    foo() as x,
    bar() as y,
):
    pass
```

But these are all valid syntax:

```pycon
Python 3.8.18 (default, Feb 15 2024, 19:36:58) 
[Clang 15.0.0 (clang-1500.1.0.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> with (x, y) as foo:
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
>>> with (x,
...     y) as foo:
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
>>> with (x,
... y):
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
>>> with (
...     x,
... ):
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: name 'x' is not defined
>>> with (
...     x,
...     y
... ) as foo:
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: name 'x' is not defined
>>> with x, (
...     y
... ): pass
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
>>> with x as foo, (
... y
... ) as bar:
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
>>> with x() as foo, (
...     y()
... ) as bar:
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
```



---

_Comment by @MichaReiser on 2025-02-12 12:11_

> For some of them, though, I think docs would be really useful for our users. For example, the [syntax differences between Python 3.8 and 3.9 due to PEP 614](https://peps.python.org/pep-0614/) are pretty subtle. And the details on when exactly you're able to parenthesize context managers in with statements on Python 3.8 are also pretty complicated -- I can never remember what they are!

That's fair. But how is it different from any other syntax error. E.g what if you get a comprehension wrong?

Either way. Having them in the parser doesn't mean that we can never have unique error codes. Red Knot would allow for it. It might just not be possible today and I think that's fine.

---

_Comment by @AlexWaygood on 2025-02-12 12:19_

> That's fair. But how is it different from any other syntax error.

Well, as we discussed earlier, we also probably want version-dependent syntax errors to be suppressible. If an error is suppressible and has documentation, it ends up looking _very much_ like a lint rule, no? ;)

And that's not to say that I want them detected using the same infrastructure as a lint rule. I think you're right that it'll be more performant to detect them in the parser where possible, and I think you make a great point when you say that it would be great for the formatter tests to be able to assert that they don't introduce any new version-dependent syntax errors. But I think we should be _aware_ that these in quite a few ways are going to end up looking a lot more like our existing linter rules than our existing syntax rules.

> E.g what if you get a comprehension wrong?

Sure, it might be nice to have better docs for our existing syntax errors too ;-) but I do think it's especially confusing and subtle for users if whether or not they get a syntax error depends on the target Python version we've inferred for their project. And documenting clearly which Python version the new syntax was added in could really help users.

> Either way. Having them in the parser doesn't mean that we can never have unique error codes. Red Knot would allow for it. It might just not be possible today and I think that's fine.

I don't have a strong opinion on whether we should detect these errors in the parser or elsewhere. But if we are going to detect them in the parser, I do think we should have a _plan_ for how we'll provide docs for these errors. It doesn't have to be part of the initial PR adding these syntax diagnostics, but it _is_ important to me.

---

_Comment by @MichaReiser on 2025-02-12 12:31_

> Well, as we discussed earlier, we also probably want version-dependent syntax errors to be suppressible. If an error is suppressible and has documentation, it ends up looking very much like a lint rule, no? ;)

I don't think this is something we have an agreement on.

---

_Comment by @AlexWaygood on 2025-02-12 12:33_

> I don't think this is something we have an agreement on.

Oh, sorry about that. I think I misunderstood our earlier conversation on Monday in that case -- that's on me :-)

---

_Renamed from "Syntax errors prototype v3" to "[syntax-errors] Start detecting compile-time syntax errors" by @ntBre on 2025-03-17 19:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:29 on 2025-03-19 17:40_

I think these all need better names. I called many of the variable names below `semantic_syntax_error(s)`. What do you think about `SemanticSyntaxChecker`, `SemanticSyntaxError`, and `SemanticSyntaxErrorKind`?

---

_@ntBre reviewed on 2025-03-19 17:40_

---

_@ntBre reviewed on 2025-03-19 17:57_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:20 on 2025-03-19 17:57_

This is not really used currently, but some of the errors in #11934 are both version-related and raised at compile time, so I expect to use it eventually.

---

_@ntBre reviewed on 2025-03-19 18:00_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:194 on 2025-03-19 18:00_

`rebound_variable` could also be a `Vec<TextRange>` and we could avoid returning here, if we want to look for additional violations.

---

_Marked ready for review by @ntBre on 2025-03-19 18:23_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-19 18:24_

---

_Comment by @ntBre on 2025-03-20 03:32_

I did notice one slightly surprising thing when testing the server integration. Because the greeter is tied to the AST visitor, if you have no AST-based rules enabled (e.g. `select = []`, which I had in my test config for some reason), you won't get any of these errors either.

---

_Comment by @MichaReiser on 2025-03-20 07:34_

> I did notice one slightly surprising thing when testing the server integration. Because the greeter is tied to the AST visitor, if you have no AST-based rules enabled (e.g. `select = []`, which I had in my test config for some reason), you won't get any of these errors either.

We have to remove this check

https://github.com/astral-sh/ruff/blob/37fbe58b13b905bfafac326a74c922f72f3b54a1/crates/ruff_linter/src/linter.rs#L139-L142

---

_Label `core` added by @MichaReiser on 2025-03-20 07:34_

---

_Review comment by @MichaReiser on `Cargo.toml`:33 on 2025-03-20 07:35_

Maybe:

```suggestion
ruff_python_syntax_errors = { path = "crates/ruff_python_semantic_errors" }
```

To clarify that it doesn't implement other syntax errors? We could also use `semantic_syntax_errors` but it feels very long. 

Another alternative is to implement the checks inside the parser crate. Or does the crate have dependencies that would prevent us from doing that?

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:5496 on 2025-03-20 07:36_

Let's add a test verifying that the semantic syntax errors are reported even if a user disabled all ast rules.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:856 on 2025-03-20 07:37_

Can you tell me more about this change? Did you remove the rule because it is now covered by the new semantic syntax checks? If so, that would be a breaking change and would have to be gated behind preview mode.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:29 on 2025-03-20 07:38_

I like the suggested names. It aligns with what we use in the parser

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:525 on 2025-03-20 07:40_

Nit: I would rename the methods to `visit_stmt` because the `enter_` suggests that there's an `exit_` pendant for every enter method. Unless you plan on adding `exit_*` methods to the visitor. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2772 on 2025-03-20 07:43_

Would you mind double-checking if/how Ruff sorts diagnostics? Does it sort diagnosics by file and their source location or only file? It's important that we sort by both, or late future import errors will now always appear last, even if their source location is earlier in a file.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:340 on 2025-03-20 07:45_

Let's move this check into `Checker` so that we avoid collecting the semantic syntax errors if the preview mode is off (Ideally, we'd skip the entire `SemanticSyntaxVisitor` but I think that would make the code unnecessarily awkward. Although it might be worth it if you're concerned about panics (it then gives users a way to opt out of the new semantic syntax visitor by disabling preview).

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:21 on 2025-03-20 07:49_

```suggestion
    python_version: PythonVersion,
```

`target_version` is the "legacy" ruff term. 

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-20 07:53_

It's unfortunate that it requires us to re-implement the `seen_docstring_boundary`. 

It makes me wonder if we should pass a `context` variable to each `visit` method that allows to pass some state to the visitor. 


For example:

```rust
fn visit_stmt(&mut self, contex: &SemanticSyntaxContext) {...}

#[derive(Debug)]
struct SemanticSyntaxContext {
	seen_docstring_boundary: bool
}
```

Or we can add a `set_seen_docstring_boundary` method to the `SyntaxChecker` and require the `Checker` to call it when appropriate (this seems a bit more error-prone to me).


Another alternative is to make `Context` a trait with:

```rust
trait SemanticSyntaxContext {
	fn python_version(&self) -> PythonVersion;

	fn seen_docstring_boundary(&self) -> bool;

	fn push_semantic_error(&mut self, error ...);
}
```

Where `Checker would implement `SemanticSyntaxContext`. The `SyntaxChecker can either be generic over `SemanticSyntaxContext` (results in a lot of monomorphization but that should be fine because we only use it once in each top-level project) or use `&mut dyn SemanticSyntaxContext)

I think I like the context most because it allows the `Checker` to push the syntax errors directly into the correct diagnostic vec.

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:167 on 2025-03-20 07:59_

Always collecting the names here seems unfortunate because most generators never contain an `ExprNamed`. I would store the generators on the `ReboundComprehensionVisitor` and use an `Option<FxHashSet<&'a Name>>` to lazily collect the target names (or a `OnceCell`)

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:194 on 2025-03-20 08:00_

I think it would be great to report all violations because it's annoying if you fix one violation only for the next to appear.

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:259 on 2025-03-20 08:02_

Moving the checker into the parser crate would have the added benefit that you could reuse its diagnostic rendering infrastructure. 

---

_@MichaReiser reviewed on 2025-03-20 08:03_

This looks good. I mainly have a few minor comments besides:

* I'd be open to move the checker into `ruff_python_parser`. Unless you forsee that the checks need to depend on additional crates
* I'm a bit worried about having to reimplement state that's currently tracked on `Checker` (e.g. seen_docstring). That's why I'm leaning towards introducing a `SemanticSyntaxContext` trait that `Checker` implements and allows providing "external" state to the checker. See inline comment

---

_@dhruvmanila reviewed on 2025-03-20 11:14_

---

_Review comment by @dhruvmanila on `crates/ruff_python_syntax_errors/src/lib.rs`:194 on 2025-03-20 11:14_

+1, this is one of the main benefit of having an error resilient parser so that we can report all of the syntax errors compared to before where we would only report the first syntax error. We should continue doing that and not regress in that area if possible.

---

_Comment by @dhruvmanila on 2025-03-20 11:21_

Feel free to request my review if it's needed otherwise I agree with all of Micha's review comments.

I'd suggest doing some testing in an editor context considering the `ruff.configuration` and `ruff.showSyntaxErrors` setting.

One thing that I'm reminded of is to check if `--statistics` includes the count of these new syntax errors or not. I think it should, we might want to update the CLI test case for `--statistics` to include these new errors.

---

_@dhruvmanila reviewed on 2025-03-20 11:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-20 11:24_

I'm not exactly sure which context are you referring to by "I like the context most" but my preference would be to have a context struct that's owned by the `Checker` which is then passed to the `SyntaxChecker`'s `visit_stmt` and `visit_expr` methods aka your first suggestion.

---

_@AlexWaygood reviewed on 2025-03-20 11:27_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:856 on 2025-03-20 11:27_

I believe the idea is:
- Move the detection logic to the new crate so that red-knot can detect it using the same backend logic, which will also want to detect this syntax error
- But from Ruff's side, make sure that this syntax error continues to be _reported_ using a linter diagnostic and with the same error code that it has always had. This ensures backwards compatibility for Ruff users while also ensuring that we do not duplicate code between the two tools or risking inconsistent behaviour between the two tools.

You can see that for Ruff, the diagnostic is still only reported if the lint rule is actually enabled. That logic has just been moved to line 2761 in `crates/ruff_linter/src/checkers/ast/mod.rs`

---

_@MichaReiser reviewed on 2025-03-20 11:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-20 11:46_

Good point. The context trait but I'm also fine with the context struct.

---

_@ntBre reviewed on 2025-03-20 12:45_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:856 on 2025-03-20 12:45_

Yes, Alex is right. This does not remove the rule and shouldn't change any user-facing behavior (I'll double check the sorting behavior you mentioned elsewhere), it just implements it with the new checker. The main motivation for this change was just to show that it's straightforward to emit these new errors as either true `Diagnostic`s or as `Message`s. That way we can migrate existing ruff rules that fit into this category if/when we want.

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-20 12:57_

The trait sounds a bit more natural to me because the `Checker` already has this state in its `SemanticModel`, so I think we'd have to repack it into a context struct for use by the `SyntaxChecker`.  Pushing the semantic errors directly is another nice benefit of the trait.

I'll play with these today and see how they feel.

---

_@ntBre reviewed on 2025-03-20 12:57_

---

_@ntBre reviewed on 2025-03-20 13:40_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:167 on 2025-03-20 13:40_

That's a good point. Is it even worth caching them like that, or should we just do a linear search each time? I think `generators` is likely to be small and most of them won't contain an `ExprNamed` anyway, like you said. That's the approach I've taken for now.

---

_@ntBre reviewed on 2025-03-20 14:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:2772 on 2025-03-20 14:03_

Empirically, I tested on this snippet:

```python
import late_future_import  # F401

from __future__ import annotations  # F404

another_violation = unbound  # F821
```

and the diagnostics come out in the correct order (F401, F404, F821), so it looks like we sort by file and location, but I will look for where in the code we actually perform the sort too.

---

_@ntBre reviewed on 2025-03-20 14:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:2772 on 2025-03-20 14:13_

It looks like they do get [sorted](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/commands/check.rs#L171) in the `check` command and rely on this `Ord` impl, which will first sort by file and then by the start of the message range.
https://github.com/astral-sh/ruff/blob/cdafd8e32b1c4d76d61d6d6578a8a7fa5a63dab2/crates/ruff_linter/src/message/mod.rs#L251-L255

---

_@MichaReiser reviewed on 2025-03-20 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:167 on 2025-03-20 15:08_

Either way seems fine to me.

---

_@MichaReiser reviewed on 2025-03-20 15:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2772 on 2025-03-20 15:09_

Thanks

---

_@ntBre reviewed on 2025-03-20 15:54_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-20 15:54_

One downside of the trait approach is that I couldn't borrow the `Checker` mutably and immutably at the same time:

```rust
    fn visit_stmt(&mut self, stmt: &'a Stmt) {
        self.syntax_checker.visit_stmt(stmt, self);
             ^^^^^^^^^^^^^^                  ^^^^
             mutable borrow                  immutable
        ...
    }
```

so I added some `RefCell`s in the `SemanticSyntaxChecker` to make them both immutable. This also means that it's not easy to have a `SemanticSyntaxContext::push_semantic_error` method; it would require another `RefCell` back on the implementer.

I still think it's okay, or maybe I'm missing something that would make it nicer too.

---

_@MichaReiser reviewed on 2025-03-20 15:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-20 15:58_

Ugh, that's annoying but makes sense. Yeah, you would have to `std::mem::take` `syntax_checker` before calling `visit_stmt`. 


Or we use the context struct, where you can copy over the relevant state.

---

_@ntBre reviewed on 2025-03-20 16:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:340 on 2025-03-20 16:04_

Ah I see what you mean about it being awkward. I thought I could just do something like

``` rust
if self.settings.preview.is_enabled() {
    self.syntax_checker.visit_stmt(stmt);
}
```

But this loses F404 detection when preview is disabled. For now I moved this into `check_ast` to avoid pushing onto the `semantic_syntax_errors` `Vec` when preview is disabled.

I don't think there's any panic-prone code in here, but I'll look back over it with that in mind. If we keep the context trait, the `RefCell` stuff is the most panicky, I think, but the encapsulated usage here should be about as safe as it gets.

---

_@ntBre reviewed on 2025-03-20 16:18_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-20 16:18_

I guess I'm slightly averse to the struct because I think we'd have to construct one on each call? I'm picturing

```rust
    fn visit_stmt(&mut self, stmt: &'a Stmt) {
        self.syntax_checker.visit_stmt(
            stmt,
            Context {
                python_version: self.target_version,
                seen_docstring_boundary: self.semantic.seen_docstring_boundary(),
            },
        );
        ....
    }
```

It's pretty minor either way for so few fields, but I'm not sure how big it will get over time. It would definitely require fewer lines of code changed in the current version, though.

`Checker` could also hold onto a `Context` of course, but then we have to keep it up to date when the underlying state changes, or move the underlying state to the `Context`, I guess.

---

_@ntBre reviewed on 2025-03-20 16:32_

---

_Review comment by @ntBre on `Cargo.toml`:33 on 2025-03-20 16:32_

Moving it to the parser makes a lot of sense to me. I moved it to a public `ruff_python_parser::semantic_errors` module. Another possible location would be a private submodule of the `error` module (`ruff_python_parser::error::semantic_errors` or maybe just `semantic` at the end?) and then we could re-export the new related types like we do for `ParseError` and `UnsupportedSyntaxError`. Maybe `UnsupportedSyntaxError` should have had its own `error` submodule too.

---

_@ntBre reviewed on 2025-03-20 16:56_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:259 on 2025-03-20 16:56_

Ooh that would be much nicer than the current snapshots. It looks like the rendering code is in the integration tests. Could I extend the inline tests to pick up the new module? I was definitely missing them after the unsupported syntax errors.

https://github.com/astral-sh/ruff/blob/c1971fdde24b37e64baa70386d933715aeec13c6/crates/ruff_python_parser/tests/generate_inline_tests.rs#L24-L26

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:259 on 2025-03-20 17:45_

Extending them might be fine. An alternative is to write some standalone tests and add them to the `valid` / `invalid` directories (but you would have to extend the testing to also run semantic checks)

---

_@MichaReiser reviewed on 2025-03-20 17:45_

---

_@MichaReiser reviewed on 2025-03-20 17:47_

---

_Review comment by @MichaReiser on `Cargo.toml`:33 on 2025-03-20 17:47_

I'm fine with any of it but I myself would go with `semantic_errors` because it's short and simple.

---

_Comment by @ntBre on 2025-03-20 22:14_

Thanks @dhruvmanila! I did test `ruff.showSyntaxErrors` initially, but I have now also tried `ruff.configuration`. It properly seems to override the local `ruff.toml` and setting `"preview": false` there disables the new errors.

I also added a test for `--statistics`, which seems to be working properly for all of the syntax error kinds.

Otherwise, I think I've handled the other comments too. Thank you and @MichaReiser  for the reviews! This push should get the updated ecosystem check as well, just in case.

---

_@dhruvmanila reviewed on 2025-03-21 04:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-21 04:41_

> `Checker` could also hold onto a `Context` of course, but then we have to keep it up to date when the underlying state changes, or move the underlying state to the `Context`, I guess.

This is what I actually envisioned with my comment. The context would all be grouped under a single struct which the `Checker` owns and that context itself would be passed to the `SyntaxChecker` whenever required. So, it would be the `Checker`'s responsibility to keep the context up to date while the `SyntaxError` should only have a read-only copy / reference to the context.

This does has the disadvantage that the diff of this PR might get large because it'll require updating multiple fields so I'd be fine moving with your approach of using either trait or creating a context specific to `SyntaxChecker`.

> I guess I'm slightly averse to the struct because I think we'd have to construct one on each call? I'm picturing

We could have a method on the `Checker` to create the context relevant to the `SyntaxChecker`.

---

_@dhruvmanila reviewed on 2025-03-21 04:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_syntax_errors/src/lib.rs`:259 on 2025-03-21 04:43_

Yeah, extending the path to pickup comments from non-parser crates should be fine

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/generate_inline_tests.rs`:42 on 2025-03-21 04:57_

Is there a specific reason that semantic errors are separate from other parser tests?

I think I'd prefer to expand the current test infrastructure to include them but I might be missing something that's obvious to you. What I'd do is:
* Include the `semantic_error` crate in `generate_inline_tests`. This will also remove the need of `resources/inline/semantic` directory as I think the generated test files are just an abstraction so it doesn't matter where those files are located as long as the test picks them up
* Expand `test_valid_syntax` and `test_invalid_syntax` to check the semantic errors using the `TestVisitor`

Curious to hear your thoughts on this.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors/mod.rs`:195 on 2025-03-21 04:59_

nit: can we move the test closer to where it's being added? So, above the `add_error` call below on line 220

---

_@dhruvmanila reviewed on 2025-03-21 05:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors/mod.rs`:1 on 2025-03-21 07:56_

I don't think this module has to have its own directory. Its only a single file. I'd move it to `src/semantic_errors.rs`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors/mod.rs`:128 on 2025-03-21 07:57_

Nit: The fact that all this code is between `SemanticSyntaxChecker` blocks makes the file a bit harder to navigate (I had to jump up and down to make changes)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:48 on 2025-03-21 08:27_

Should this be `SemanticSyntaxError` instead of `SyntaxError`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors/mod.rs`:25 on 2025-03-21 08:32_

I'd prefer not to use a `RefCell` here because they are only needed because of how `Checker` is organized. I quickly looked at Red Knot and I don't think ref cells will be necessary in Red Knot because it also has a `context` type that is independent. That's why I think the mutability should be worked around in `Checker` and not here. 

I had to play with the code to come up with a recommendation. That's why I'll share a patch here. 

```patch
Subject: [PATCH] Align server indexing with CLI behavior
---
Index: crates/ruff_python_parser/tests/fixtures.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_parser/tests/fixtures.rs b/crates/ruff_python_parser/tests/fixtures.rs
--- a/crates/ruff_python_parser/tests/fixtures.rs	(revision 88cbb9ba403d904f7e66025e9625adf076b79869)
+++ b/crates/ruff_python_parser/tests/fixtures.rs	(date 1742545597020)
@@ -1,3 +1,4 @@
+use std::cell::RefCell;
 use std::cmp::Ordering;
 use std::fmt::{Formatter, Write};
 use std::fs;
@@ -7,7 +8,9 @@
 use ruff_python_ast::visitor::source_order::{walk_module, SourceOrderVisitor, TraversalSignal};
 use ruff_python_ast::visitor::Visitor;
 use ruff_python_ast::{AnyNodeRef, Mod, PythonVersion};
-use ruff_python_parser::semantic_errors::{SemanticSyntaxChecker, SemanticSyntaxContext};
+use ruff_python_parser::semantic_errors::{
+    SemanticSyntaxCheckerVisitor, SemanticSyntaxContext, SemanticSyntaxError,
+};
 use ruff_python_parser::{parse_unchecked, Mode, ParseErrorType, ParseOptions, Token};
 use ruff_source_file::{LineIndex, OneIndexed, SourceCode};
 use ruff_text_size::{Ranged, TextLen, TextRange, TextSize};
@@ -194,15 +197,13 @@
 
     let parsed = parsed.try_into_module().expect("Parsed with Mode::Module");
 
-    let mut visitor = TestVisitor {
-        checker: SemanticSyntaxChecker::new(),
-    };
+    let mut visitor = SemanticSyntaxCheckerVisitor::new(TestContext::default());
 
     for stmt in parsed.suite() {
         visitor.visit_stmt(stmt);
     }
 
-    let semantic_syntax_errors = visitor.checker.finish();
+    let semantic_syntax_errors = visitor.into_context().diagnostics.into_inner();
 
     if !semantic_syntax_errors.is_empty() {
         let mut message = "Expected no semantic syntax errors for a valid program:\n".to_string();
@@ -252,15 +253,13 @@
 
     let parsed = parsed.try_into_module().expect("Parsed with Mode::Module");
 
-    let mut visitor = TestVisitor {
-        checker: SemanticSyntaxChecker::new(),
-    };
+    let mut visitor = SemanticSyntaxCheckerVisitor::new(TestContext::default());
 
     for stmt in parsed.suite() {
         visitor.visit_stmt(stmt);
     }
 
-    let semantic_syntax_errors = visitor.checker.finish();
+    let semantic_syntax_errors = visitor.into_context().diagnostics.into_inner();
 
     assert!(
         !semantic_syntax_errors.is_empty(),
@@ -531,11 +530,12 @@
     }
 }
 
-struct TestVisitor {
-    checker: SemanticSyntaxChecker,
+#[derive(Debug, Default)]
+struct TestContext {
+    diagnostics: RefCell<Vec<SemanticSyntaxError>>,
 }
 
-impl SemanticSyntaxContext for TestVisitor {
+impl SemanticSyntaxContext for TestContext {
     fn seen_docstring_boundary(&self) -> bool {
         false
     }
@@ -543,16 +543,8 @@
     fn python_version(&self) -> PythonVersion {
         PythonVersion::default()
     }
-}
 
-impl Visitor<'_> for TestVisitor {
-    fn visit_stmt(&mut self, stmt: &ruff_python_ast::Stmt) {
-        self.checker.visit_stmt(stmt, self);
-        ruff_python_ast::visitor::walk_stmt(self, stmt);
-    }
-
-    fn visit_expr(&mut self, expr: &ruff_python_ast::Expr) {
-        self.checker.visit_expr(expr, self);
-        ruff_python_ast::visitor::walk_expr(self, expr);
+    fn report_semantic_error(&self, error: SemanticSyntaxError) {
+        self.diagnostics.borrow_mut().push(error);
     }
 }
Index: crates/ruff_python_parser/src/semantic_errors/mod.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_parser/src/semantic_errors/mod.rs b/crates/ruff_python_parser/src/semantic_errors/mod.rs
--- a/crates/ruff_python_parser/src/semantic_errors/mod.rs	(revision 88cbb9ba403d904f7e66025e9625adf076b79869)
+++ b/crates/ruff_python_parser/src/semantic_errors/mod.rs	(date 1742545597028)
@@ -4,7 +4,7 @@
 //! [`SemanticSyntaxChecker::visit_stmt`] and [`SemanticSyntaxChecker::visit_expr`] methods should
 //! be called in a parent `Visitor`'s `visit_stmt` and `visit_expr` methods, respectively.
 
-use std::{cell::RefCell, fmt::Display};
+use std::fmt::Display;
 
 use ruff_python_ast::{
     self as ast,
@@ -13,51 +13,25 @@
 };
 use ruff_text_size::TextRange;
 
-struct CheckerState {
+#[derive(Debug)]
+pub struct SemanticSyntaxChecker {
     /// these could be grouped into a bitflags struct like `SemanticModel`
     seen_futures_boundary: bool,
 }
 
-pub struct SemanticSyntaxChecker {
-    /// The cumulative set of syntax errors found when visiting the source AST.
-    errors: RefCell<Vec<SemanticSyntaxError>>,
-
-    state: RefCell<CheckerState>,
-}
-
 impl SemanticSyntaxChecker {
     pub fn new() -> Self {
         Self {
-            errors: RefCell::new(Vec::new()),
-            state: RefCell::new(CheckerState {
-                seen_futures_boundary: false,
-            }),
+            seen_futures_boundary: false,
         }
     }
-
-    pub fn finish(self) -> Vec<SemanticSyntaxError> {
-        self.errors.into_inner()
-    }
 
     fn seen_futures_boundary(&self) -> bool {
-        self.state.borrow().seen_futures_boundary
-    }
-
-    fn set_seen_futures_boundary(&self, seen_futures_boundary: bool) {
-        self.state.borrow_mut().seen_futures_boundary = seen_futures_boundary;
+        self.seen_futures_boundary
     }
 
-    fn add_error(
-        &self,
-        kind: SemanticSyntaxErrorKind,
-        range: TextRange,
-        python_version: PythonVersion,
-    ) {
-        self.errors.borrow_mut().push(SemanticSyntaxError {
-            kind,
-            range,
-            python_version,
-        });
+    fn set_seen_futures_boundary(&mut self, seen_futures_boundary: bool) {
+        self.seen_futures_boundary = seen_futures_boundary;
     }
 }
 
@@ -125,22 +99,32 @@
 
     /// The target Python version for detecting backwards-incompatible syntax changes.
     fn python_version(&self) -> PythonVersion;
+
+    fn report_semantic_error(&self, error: SemanticSyntaxError);
 }
 
 impl SemanticSyntaxChecker {
+    fn add_error<Ctx: SemanticSyntaxContext>(
+        context: &Ctx,
+        kind: SemanticSyntaxErrorKind,
+        range: TextRange,
+    ) {
+        context.report_semantic_error(SemanticSyntaxError {
+            kind,
+            range,
+            python_version: context.python_version(),
+        });
+    }
+
     fn check_stmt<Ctx: SemanticSyntaxContext>(&self, stmt: &ast::Stmt, ctx: &Ctx) {
         if let Stmt::ImportFrom(StmtImportFrom { range, module, .. }) = stmt {
             if self.seen_futures_boundary() && matches!(module.as_deref(), Some("__future__")) {
-                self.add_error(
-                    SemanticSyntaxErrorKind::LateFutureImport,
-                    *range,
-                    ctx.python_version(),
-                );
+                Self::add_error(ctx, SemanticSyntaxErrorKind::LateFutureImport, *range);
             }
         }
     }
 
-    pub fn visit_stmt<Ctx: SemanticSyntaxContext>(&self, stmt: &ast::Stmt, ctx: &Ctx) {
+    pub fn visit_stmt<Ctx: SemanticSyntaxContext>(&mut self, stmt: &ast::Stmt, ctx: &Ctx) {
         // update internal state
         match stmt {
             Stmt::Expr(StmtExpr { value, .. })
@@ -160,7 +144,7 @@
         self.check_stmt(stmt, ctx);
     }
 
-    pub fn visit_expr<Ctx: SemanticSyntaxContext>(&self, expr: &Expr, ctx: &Ctx) {
+    pub fn visit_expr<Ctx: SemanticSyntaxContext>(&mut self, expr: &Expr, ctx: &Ctx) {
         match expr {
             Expr::ListComp(ast::ExprListComp {
                 elt, generators, ..
@@ -217,10 +201,10 @@
         // TODO(brent) with multiple diagnostic ranges, we could mark both the named expr (current)
         // and the name expr being rebound
         for range in rebound_variables {
-            self.add_error(
+            Self::add_error(
+                ctx,
                 SemanticSyntaxErrorKind::ReboundComprehensionVariable,
                 range,
-                ctx.python_version(),
             );
         }
     }
@@ -249,3 +233,37 @@
         walk_expr(self, expr);
     }
 }
+
+#[derive(Default)]
+pub struct SemanticSyntaxCheckerVisitor<Ctx> {
+    checker: SemanticSyntaxChecker,
+    context: Ctx,
+}
+
+impl<Ctx> SemanticSyntaxCheckerVisitor<Ctx> {
+    pub fn new(context: Ctx) -> Self {
+        Self {
+            checker: SemanticSyntaxChecker::new(),
+            context,
+        }
+    }
+
+    pub fn into_context(self) -> Ctx {
+        self.context
+    }
+}
+
+impl<Ctx> Visitor<'_> for SemanticSyntaxCheckerVisitor<Ctx>
+where
+    Ctx: SemanticSyntaxContext,
+{
+    fn visit_stmt(&mut self, stmt: &'_ Stmt) {
+        self.checker.visit_stmt(stmt, &self.context);
+        ruff_python_ast::visitor::walk_stmt(self, stmt);
+    }
+
+    fn visit_expr(&mut self, expr: &'_ Expr) {
+        self.checker.visit_expr(expr, &self.context);
+        ruff_python_ast::visitor::walk_expr(self, expr);
+    }
+}
Index: crates/ruff_linter/src/checkers/ast/mod.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/checkers/ast/mod.rs b/crates/ruff_linter/src/checkers/ast/mod.rs
--- a/crates/ruff_linter/src/checkers/ast/mod.rs	(revision 88cbb9ba403d904f7e66025e9625adf076b79869)
+++ b/crates/ruff_linter/src/checkers/ast/mod.rs	(date 1742545597025)
@@ -232,6 +232,7 @@
 
     #[allow(clippy::struct_field_names)]
     syntax_checker: SemanticSyntaxChecker,
+    semantic_errors: RefCell<Vec<SemanticSyntaxError>>,
 }
 
 impl<'a> Checker<'a> {
@@ -280,6 +281,7 @@
             docstring_state: DocstringState::default(),
             target_version,
             syntax_checker: SemanticSyntaxChecker::new(),
+            semantic_errors: RefCell::default(),
         }
     }
 }
@@ -520,6 +522,12 @@
     pub(crate) const fn target_version(&self) -> PythonVersion {
         self.target_version
     }
+
+    fn with_semantic_checker(&mut self, f: impl FnOnce(&mut SemanticSyntaxChecker, &Checker)) {
+        let mut checker = std::mem::take(&mut self.syntax_checker);
+        f(&mut checker, self);
+        self.syntax_checker = checker;
+    }
 }
 
 impl SemanticSyntaxContext for Checker<'_> {
@@ -530,11 +538,27 @@
     fn python_version(&self) -> PythonVersion {
         self.target_version
     }
+
+    fn report_semantic_error(&self, error: SemanticSyntaxError) {
+        match error.kind {
+            SemanticSyntaxErrorKind::LateFutureImport => {
+                if self.settings.rules.enabled(Rule::LateFutureImport) {
+                    self.report_diagnostic(Diagnostic::new(LateFutureImport, error.range));
+                }
+            }
+            SemanticSyntaxErrorKind::ReboundComprehensionVariable
+                if self.settings.preview.is_enabled() =>
+            {
+                self.semantic_errors.borrow_mut().push(error);
+            }
+            SemanticSyntaxErrorKind::ReboundComprehensionVariable => {}
+        }
+    }
 }
 
 impl<'a> Visitor<'a> for Checker<'a> {
     fn visit_stmt(&mut self, stmt: &'a Stmt) {
-        self.syntax_checker.visit_stmt(stmt, self);
+        self.with_semantic_checker(|semantic, context| semantic.visit_stmt(stmt, context));
 
         // Step 0: Pre-processing
         self.semantic.push_node(stmt);
@@ -1147,7 +1171,7 @@
     }
 
     fn visit_expr(&mut self, expr: &'a Expr) {
-        self.syntax_checker.visit_expr(expr, self);
+        self.with_semantic_checker(|semantic, context| semantic.visit_expr(expr, context));
 
         // Step 0: Pre-processing
         if self.source_type.is_stub()
@@ -2759,32 +2783,11 @@
 
     let Checker {
         diagnostics,
-        syntax_checker,
-        settings,
+        semantic_errors,
         ..
     } = checker;
 
     // partition the semantic syntax errors into Diagnostics and regular errors
-    let mut diagnostics = diagnostics.take();
-    let mut semantic_syntax_errors = Vec::new();
-    for semantic_syntax_error in syntax_checker.finish() {
-        match semantic_syntax_error.kind {
-            SemanticSyntaxErrorKind::LateFutureImport => {
-                if settings.rules.enabled(Rule::LateFutureImport) {
-                    diagnostics.push(Diagnostic::new(
-                        LateFutureImport,
-                        semantic_syntax_error.range,
-                    ));
-                }
-            }
-            SemanticSyntaxErrorKind::ReboundComprehensionVariable
-                if settings.preview.is_enabled() =>
-            {
-                semantic_syntax_errors.push(semantic_syntax_error);
-            }
-            SemanticSyntaxErrorKind::ReboundComprehensionVariable => {}
-        }
-    }
 
-    (diagnostics, semantic_syntax_errors)
+    (diagnostics.into_inner(), semantic_errors.into_inner())
 }

```

The main changes are:

* Use `std::mem::take` in `Checker` to work around the mutability problem. But use a helper method to do this to avoid the situation where we forget to put it back. 
* Add a `report_semantic_error` method to context. This will work better with Red Knot's error reporting (where errors are immediately checked for suppressions)
* Introduce a `SemanticSyntaxCheckerVisitor` that can be used in tests. It should reduce the risk that we forget to update tests when adding new `visit_` methods.

---

_@MichaReiser approved on 2025-03-21 08:33_

This looks good. A few nits around how to work around the context mutability problem.

---

_@MichaReiser reviewed on 2025-03-21 09:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:27 on 2025-03-21 09:21_

Separating `Checker` and its context information would be nice and could help with some future work too, but I think it's a little out of scope for this PR. I commented with a solution that should allow us to work-around the mutability problem locally in `Checker`.



---

_@ntBre reviewed on 2025-03-21 11:09_

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/generate_inline_tests.rs`:42 on 2025-03-21 11:09_

I thought we might want to avoid the AST visitor in the existing tests, but if it's okay to check that for all of them, I can definitely combine these!

---

_@ntBre reviewed on 2025-03-21 11:10_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors/mod.rs`:1 on 2025-03-21 11:10_

That makes sense. I mentioned in the PR summary that I thought about splitting "rules" into their own modules, but one file makes sense with the current arrangement.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:48 on 2025-03-21 11:12_

Yes, thank you!

---

_@ntBre reviewed on 2025-03-21 11:12_

---

_@ntBre reviewed on 2025-03-21 12:06_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors/mod.rs`:25 on 2025-03-21 12:06_

Thanks for the patch, this looks much nicer. I fixed one clippy lint for an unused `self` variable in `check_generator_expr`, but otherwise I applied it verbatim.

---

_Merged by @ntBre on 2025-03-21 18:45_

---

_Closed by @ntBre on 2025-03-21 18:45_

---

_Branch deleted on 2025-03-21 18:45_

---
