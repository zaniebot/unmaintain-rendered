```yaml
number: 21779
title: "[ty] Support re-exports in auto-import"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/auto-import-reexports
created_at: 2025-12-03T20:36:33Z
updated_at: 2025-12-04T18:21:29Z
url: https://github.com/astral-sh/ruff/pull/21779
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Support re-exports in auto-import

---

_Pull request opened by @BurntSushi on 2025-12-03 20:36_

This PR principally makes auto-import aware of re-exports in modules.
Here are some examples of re-exports that we previously ignored.

First is the "redundant import alias" idiom:

```
import X as X
from module import X as X
```

Second is a wildcard import:

```
from module import *
```

And third is a regular import with `__all__`:

```
from module import X
__all__ = ['X']
```

An import outside of the above is ignored for the purposes of
determining the exported interface of a module.
See: https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols

This PR also automatically skips over symbols in any module (or child
of a module) whose name starts with an underscore. (We don't skip such
modules in first party code though, since we want to be able to offer
auto-import completions for things internal to a project while working
in that project.)

Both of these improvements are very relevant for using libraries like `numpy`.
Namely, `numpy` crafts its API via exports and `__all__`. `numpy` also has many
symbols inside of project-internal modules that can otherwise flood the results
of auto-import.

Closes https://github.com/astral-sh/ty/issues/1555, Closes #21112, Closes https://github.com/astral-sh/ty/issues/895


---

_Review requested from @carljm by @BurntSushi on 2025-12-03 20:36_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-03 20:36_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-03 20:36_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-03 20:36_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-03 20:36_

---

_Label `ty` added by @AlexWaygood on 2025-12-03 20:36_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-03 20:37_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-03 20:37_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-03 20:37_

---

_Label `server` added by @AlexWaygood on 2025-12-03 20:37_

---

_Comment by @BurntSushi on 2025-12-03 20:38_

Demo for re-exports:

https://github.com/user-attachments/assets/072e141e-954e-4ded-9a2f-b3de7ce377a5

Demo that first party modules starting with a `_` are still included in auto-import:

https://github.com/user-attachments/assets/47eef4a5-31d0-442c-a9bc-68cd7feac4d1



---

_Comment by @astral-sh-bot[bot] on 2025-12-03 20:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-03 20:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review requested from @Gankra by @Gankra on 2025-12-03 22:32_

---

_Review comment by @carljm on `crates/ty_ide/src/completion.rs`:5819 on 2025-12-03 23:02_

What change is causing these suggestions to go away? They don't seem to be defined in an underscore-prefixed module. They are defined in a `sys.version_info` conditional -- do we also add support for that in this PR? Or change the Python version we run these tests with?

---

_Review comment by @carljm on `crates/ty_ide/src/completion.rs`:5886 on 2025-12-03 23:05_

Probably orthogonal to this PR, but the "Current module" bit caught me by surprise here. `ZQZQ` isn't defined in the current module, it's defined in `bar.py`. But maybe we use this text just to indicate "not going to require a new import"? As in, `foo` has been imported into your current module, so `foo.ZQZQ` is already available here.

---

_Review comment by @carljm on `crates/ty_ide/src/completion.rs`:5909 on 2025-12-03 23:06_

I guess the real test here is that (unlike in the case below with the explicit redundant-alias re-export convention), `"ZQZQ :: foo"` does _not_ show up here. This definitely confused me on my first read of this test -- "Why is `foo.py` even here? How is this a test of re-exports? What are we even testing for here?". I generally find that some comments regarding intent can help clarify such negative tests a lot. (Or really all tests -- but I'm not trying to dictate your test-writing style here, if you generally don't comment them.)

---

_Review comment by @carljm on `crates/ty_ide/src/completion.rs`:5886 on 2025-12-03 23:12_

I guess the noteworthy thing here is that when auto-completing an attribute (vs adding an auto-import), we don't care if the name is re-exported or not?

I think this is the right call for Python files, but I'm not sure about stubs / typeshed. I think in the case of stubs we maybe shouldn't auto-complete non-re-exported names at all? But not totally sure, and it's not a question that has to be answered in this PR.

---

_Review comment by @carljm on `crates/ty_ide/src/completion.rs`:5959 on 2025-12-03 23:14_

What if this were `bar.ZQ<CURSOR>` after `import bar`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/module_resolver/path.rs`:597 on 2025-12-03 23:14_

pub all the things! 😆 

---

_Review comment by @carljm on `crates/ty_server/tests/e2e/snapshots/e2e__notebook__auto_import.snap`:9 on 2025-12-03 23:15_

Are these changes that a reviewer should validate somehow?

---

_Review comment by @carljm on `crates/ty_completion_eval/completion-evaluation-tasks.csv`:14 on 2025-12-03 23:17_

Are these changes that a reviewer should validate somehow?

---

_Review comment by @carljm on `crates/ty_ide/src/symbols.rs`:1024 on 2025-12-04 00:13_

We have to allocate a String here, we can't borrow a `&str` from the AST?

---

_Review comment by @carljm on `crates/ty_ide/src/symbols.rs`:763 on 2025-12-04 00:15_

This in particular seems like the place where the FIXME below about conditionals is most dangerous, because it could cause us to treat some symbols as private that should be public.

---

_Review comment by @carljm on `crates/ty_ide/src/symbols.rs`:1162 on 2025-12-04 00:21_

If you're wanting to test fixme cases, another "statically known condition" we should support here is `sys.version_info` checks.

---

_Review comment by @carljm on `crates/ty_ide/src/symbols.rs`:1451 on 2025-12-04 00:23_

I don't see any tests for the fact that a subsequent non-augmented assignment (or import) of `__all__` clears previous names. (Or any fixme test for the fact that we'll do this even if they are conditional, e.g. in

```py
if whatever:
    __all__ = [...]
else:
    __all__ = [...]
```

we will always take the `else` version of `__all__`.

---

_Review comment by @carljm on `crates/ty_ide/src/symbols.rs`:1378 on 2025-12-04 00:24_

Are we sure that we should consider double-underscore names as exports from a module? (In this case the question is whether it should be considered as exported from `foo`, since the spec is clear that if it's public in `foo`, the wildcard import makes it public in `test`.)

(I assume this was previously discussed and a decision was made for solid reasons, if so feel free to ignore.)

---

_@carljm approved on 2025-12-04 00:31_

Fantastic. What a test suite!

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:5819 on 2025-12-04 09:25_

I suspect it's either because `importlib.metadata` uses `__all__` OR because the definition is version dependet.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:121 on 2025-12-04 09:27_

Should this be an `FxIndexSet` so that we preserve the order in which the symbols are declared in all or do we never iterate over the symbols?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:387 on 2025-12-04 09:27_

It might be worth adding a constructor to `SymbolVisitor` that hides the "these are the default states" logic

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:800 on 2025-12-04 09:40_

Do we also need to consider modules starting with `_` as private or is this already handled elsewhere?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:786 on 2025-12-04 09:41_

Unrelated to this PR but I noticed that you're using it in a few places. It would probalby be good if we have a single definition of what's a constant. E.g. we have something similar (but different) in semantic tokens 

https://github.com/astral-sh/ruff/blob/8f104faa5fb9ef87e06aef822b31f342a177a9d8/crates/ty_ide/src/semantic_tokens.rs#L256-L259

I'm pretty sure we have something similar in Ruff but my search skills are failing me right now.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:905 on 2025-12-04 09:42_

Depending on your definition of `in_class`, you'll want to unset `in_class` when entering a new function and unset `in_function` when entering a class. You might also want to unset `in_class` and `in_function` when entering a lambda expression

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:1030 on 2025-12-04 09:43_

You might want to override `visit_expression` to an empty method to avoid unnecessary work (the default walks the entire expression)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:763 on 2025-12-04 09:45_

I could see us decide to union instead of overriding `__all__` in those cases. There's a risk that we suggest a few extra completions but it isn't strictly *more wrong* than always using the last branch (which might be for another python version). The benefit of unioning is that  users at least see all completions.

---

_@MichaReiser approved on 2025-12-04 09:46_

---

_@BurntSushi reviewed on 2025-12-04 14:05_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:5819 on 2025-12-04 14:05_

Yeah it's because this PR switches to a stricter semantic when `__all__` is present, and the `Deprecated` symbols aren't in `importlib.metadata.__all__`. (I think I mentioned this in a commit message. :P)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:5819 on 2025-12-04 14:07_

> (I think I mentioned this in a commit message. :P)

Should I tell you that...

---

_@MichaReiser reviewed on 2025-12-04 14:07_

---

_@BurntSushi reviewed on 2025-12-04 14:11_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:5819 on 2025-12-04 14:11_

For top-level conditionals, that behavior hasn't changed (or shouldn't have changed). At present, auto-import just assumes all such conditions are true. So you might get offered completions that don't actually exist.

I filed https://github.com/astral-sh/ty/issues/1754 to track this.

---

_@BurntSushi reviewed on 2025-12-04 14:14_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:5886 on 2025-12-04 14:14_

Yeah this is just a test thing. But I agree the language could be clearer here. I changed this to `<no import required>`.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:447 on 2025-12-04 14:20_

nit: most entries in `__all__` are likely to be small -- could we use `ruff_python_ast::Name` here, for less memory allocation?

---

_@BurntSushi reviewed on 2025-12-04 14:20_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:5909 on 2025-12-04 14:20_

Yeah I can add a comment here. I sometimes comment tests. I can at least go through and add a quick note on tests expecting a negative result.

---

_Comment by @AlexWaygood on 2025-12-04 14:27_

I think this PR probably closes https://github.com/astral-sh/ty/issues/895. I.e., I don't think it would be desirable to have the `__all__` inference machinery you're adding to `SymbolVisitor` added to the `ExportFinder` visitor, because the `ExportVisitor` in `ty_python_semantic` cannot use type inference to evaluate statically known conditions such as `sys.version_info` checks, but our `__all__` machinery in `ty_python_semantic` _does_ use type inference to evaluate statically known conditions such as `sys.version_info` checks. So once this PR has landed, I think the conclusion is pretty clear that it would _not_ be a good idea to unify the two visitors 😄

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:566 on 2025-12-04 14:29_

note that for something like `from importlib import machinery`, `machinery` is actually a module. But that's obviously hard to detect without type inference

---

_@BurntSushi reviewed on 2025-12-04 14:29_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:5886 on 2025-12-04 14:29_

Yes, that's correct.

I don't think I have a strong opinion yet on whether auto-complete includes non-re-exported names based on whether it's a stub or not. What makes the stub special enough that we should be stricter when it's present? Is it just that it implies someone has thoughtfully crafted the public API and we should therefore be stricter about respecting it?

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:5886 on 2025-12-04 14:37_

You should assume that most names in a stub also exist at runtime.

Private names in a stub (starting with a single underscore) are often things that do not in fact exist at runtime. But you can't impose a blanket rule that private names in stubs should never be considered unexported, because there are things like `os._exit` that are very much public API. The underscore there is to warn users that it's a very dangerous API to use.

You can however have very high confidence that private names in stubs that define type aliases, protocols, type variables or typed dicts will not in fact exist at runtime. You can also have high confidence that most imported items will not exist at runtime: anything imported that is not included in `__all__`, does not refer to a submodule of the current package, and does not use the redundant alias convention. Lastly, anything decorated with `@type_check_only` in a stub file will not exist at runtime.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:800 on 2025-12-04 14:45_

Additionally, a symbol that starts with two underscores (but does not end with two underscores) should also probably be considered private

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:905 on 2025-12-04 14:46_

> You might also want to unset `in_class` and `in_function` when entering a lambda expression

I doubt lambdas are relevant to Andrew's work here, but if they are then we should do the same for comprehension scopes and generator expressions as we do for lambdas; these are also isolated scopes that can define their own symbols

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:933 on 2025-12-04 14:49_

do you also want to check `aug_assign.op` here? `__all__ *= 3` is a `StmtAugAssign` node -- I think if we see any `op` that's not an addition here and the l.h.s. is `__all__`, we should mark `self.all_invalid = true` and abort before calling `self.extend_all()`?

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:1507 on 2025-12-04 14:59_

this looks incorrect. Since `foo.py` doesn't have an `__all__` variable in it, the `from foo import *` import won't actually import `_ZQZQZQ` into `test.py` (because of the leading underscore). Not only should `_ZQZQ` not be considered re-exported from `test.py` -- it's actually undefined in `test.py`.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:1539 on 2025-12-04 15:01_

this could do with a comment added to it that says that the `from foo import *` will actually import the `collections` symbol into `test.py` at runtime, but we nonetheless do not consider it _publicly re-exported_ from either `foo` or `test`

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:1567 on 2025-12-04 15:03_

same here, I think this could do with a comment saying that this isn't testing the runtime semantics of which symbols are imported as a result of the `*` import; it's testing the semantics of which symbols we consider publicly exported according to the conventions in the typing spec

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:1833 on 2025-12-04 15:06_

(note that I believe this actually causes the `*` import to fail at runtime with `AttributeError`, because it tries to grab `TRICKSY` from `foo` but fails!)

---

_@AlexWaygood requested changes on 2025-12-04 15:08_

Epic test suite!!

I noticed one correctness issue. At runtime, `from foo import *` will not import any names starting with an underscore from `foo` if `foo` doesn't define `__all__` (https://github.com/astral-sh/ruff/pull/21779#discussion_r2589422916). This is specced [here](https://docs.python.org/3/reference/lexical_analysis.html#reserved-classes-of-identifiers).

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:5819 on 2025-12-04 15:15_

yeah, this seems like a very good change to me! `importlib.metadata.DeprecatedList` definitely isn't part of that module's public API.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:5886 on 2025-12-04 15:18_

I do think Carl is correct here that we should not offer `ZQZQ` as an attribute completion here at all if it's `foo.pyi` instead of `foo.py`. In the type checker, if `foo` is defined as a `pyi` file then I don't think we consider `foo` to have a `ZQZQ` attribute at all, because it's been defined in the `foo` module using an import that wasn't an explicit re-export. Stubs often import many things in modules even though they don't exist in those modules at runtime; it's often necessary to do so in order to annotate the public interfaces of those modules.

---

_@AlexWaygood reviewed on 2025-12-04 15:19_

---

_Comment by @AlexWaygood on 2025-12-04 15:23_

What's our behaviour if `__all__` is assigned or updated in a non-global scope? E.g.

```py
__all__ = []

def foo():
    __all__.append("bar")

class X:
    def method(self):
        __all__.extend(["baz"])
```

I might have missed it, but I didn't spot any tests for that. Does our behaviour here align with what we do in `ty_python_semantic`'s inference for `__all__`?

---

_@BurntSushi reviewed on 2025-12-04 15:34_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:5886 on 2025-12-04 15:34_

It actually looks like if I change this test to use `foo.pyi`, then `ZQZQ` isn't offered today.

In any case, I filed an issue about this more generally so that we don't lose track of it: https://github.com/astral-sh/ty/issues/1756

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:5959 on 2025-12-04 15:43_

Then `ZQZQ2` will be included, unlike in the auto-import case.

This was "intended" on my part. In the sense that this PR doesn't change how we handle `__all__` for object-attr completions. And because I am uncertain what the right behavior is here. Maybe the right approach is for it to mimic auto-import and strictly adhere to `__all__` when it's present? (It's perhaps notable that pylance is similarly relaxed here. It's also relaxed when it comes to respecting `__all__` for auto-import too. That is, even when `__all__` is defined, pylance will include symbols that aren't part of `__all__`. So that's probably where this behavior came from originally.)

In any case, I added this as a test (confirming current behavior) and created an issue for this: https://github.com/astral-sh/ty/issues/1757

---

_@BurntSushi reviewed on 2025-12-04 15:43_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:121 on 2025-12-04 15:44_

No, we don't care about order here. Just set membership. :)

---

_@BurntSushi reviewed on 2025-12-04 15:44_

---

_@BurntSushi reviewed on 2025-12-04 15:45_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:387 on 2025-12-04 15:45_

Fair!

---

_@BurntSushi reviewed on 2025-12-04 15:52_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:566 on 2025-12-04 15:52_

Yeah I think there are other cases where this symbol kind isn't quite correct. Or at least, not as precise as it could be.

---

_@AlexWaygood reviewed on 2025-12-04 15:54_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:566 on 2025-12-04 15:54_

Maybe that's worth adding a comment here? (No strong opinion though)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:763 on 2025-12-04 15:56_

Good catch. I filed https://github.com/astral-sh/ty/issues/1758 for tracking this.

---

_@BurntSushi reviewed on 2025-12-04 15:56_

---

_@BurntSushi reviewed on 2025-12-04 15:58_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:786 on 2025-12-04 15:58_

Yeah hmmm. I'll keep this in mind. The additional `name.len() > 1` condition on that definition is interesting. I feel like that's probably not what we want for auto-import?

---

_@BurntSushi reviewed on 2025-12-04 16:03_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:800 on 2025-12-04 16:03_

I don't think modules need to be considered separately here? They'll just be like a regular symbol. And auto-import doesn't suggest modules yet.

Also note the comment here: we are still returning these as part of the public API.

---

_@BurntSushi reviewed on 2025-12-04 16:05_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:905 on 2025-12-04 16:05_

I don't think we need this level of precision here. I added some comments to clarify:

```rust
    /// Track if we're currently inside a function at any point.
    ///
    /// This is true even when we're inside a class definition
    /// that is inside a class.
    in_function: bool,
    /// Track if we're currently inside a class at any point.
    ///
    /// This is true even when we're inside a function definition
    /// that is inside a class.
    in_class: bool,
```

---

_@BurntSushi reviewed on 2025-12-04 16:14_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:933 on 2025-12-04 16:14_

Ooo nice catch! Done.

---

_@BurntSushi reviewed on 2025-12-04 16:16_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:1030 on 2025-12-04 16:16_

Nice! Done.

---

_@BurntSushi reviewed on 2025-12-04 16:22_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:1024 on 2025-12-04 16:22_

I did switch this to `Name`, so allocs should be avoided in many cases now.

Switching to a borrowed `&str` looks a little more tricky to me. I can always revisit this if it turns out to be a bottleneck.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:1451 on 2025-12-04 16:25_

Nice catch. Tests added!

---

_@BurntSushi reviewed on 2025-12-04 16:25_

---

_@BurntSushi reviewed on 2025-12-04 16:38_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:1507 on 2025-12-04 16:38_

Ah I was intentionally allowing sunder and dunder names (when `__all__` isn't defined) to be exported. But I guess it breaks down in the re-export case. Thank you for catching this! Fixed.

---

_@BurntSushi reviewed on 2025-12-04 16:41_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:1378 on 2025-12-04 16:41_

I think this was wrong. In this case, I don't think `__ZQZQZQ` is even available at runtime? I've fixed it so that this case no longer returns `__ZQZQZQ`.

But more generally:

> Are we sure that we should consider double-underscore names as exports from a module?

I don't think we are sure. But I think the last time we talked about it (I'm having trouble finding that discussion), we came down on the side of showing them but ranked down. That's the status quo today.

But I did file https://github.com/astral-sh/ty/issues/1757 where we can perhaps talk about this decision more.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:1539 on 2025-12-04 16:46_

Aye. I've added similarish comments to other tests with a negative result.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:1833 on 2025-12-04 16:54_

Ah yup. I added a comment noting this (and to a couple other tests where this is true too).

Interestingly, pylance behaves the same as ty (now does with this PR): an import is still offered, but it results in a runtime error.

I guess ideally we wouldn't offer completions for such things, but I opened an issue tracking that https://github.com/astral-sh/ty/issues/1760

---

_Review comment by @BurntSushi on `crates/ty_server/tests/e2e/snapshots/e2e__notebook__auto_import.snap`:9 on 2025-12-04 16:56_

This is just the ranking position of the completion. I don't think it's something that needs to be watched too carefully. In this case, it changed because some completions were added and some removed. So it changed the relative position.

I added a filter that redacts this part of the snapshot. I think this is okay especially given that we already have an evaluation running in CI that pays attention to ranking.

---

_@BurntSushi reviewed on 2025-12-04 17:07_

---

_Review request for @Gankra removed by @Gankra on 2025-12-04 17:17_

---

_@BurntSushi reviewed on 2025-12-04 17:20_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/completion-evaluation-tasks.csv`:14 on 2025-12-04 17:20_

I do mention them in my commit message for this change. This particular change is an improvement, since the expected result now appears at rank position 2 instead of 4.

As for reviewers _validating_ them... if you wanted to do that, I think you'd have to run a `show-one` command to look at the completions returned. In this case (from the [eval README](https://github.com/astral-sh/ruff/blob/main/crates/ty_completion_eval/README.md#running-an-evaluation)), that'd be:

```
$ cargo r -q -p ty_completion_eval internal-typeshed-hidden --index 0
ALERT_DESCRIPTION_UNKNOWN_PSK_IDENTITY (module: ssl)
NoneType (module: types) (*, 2/4)
NotImplementedType (module: types)
REG_NOTIFY_CHANGE_SECURITY (module: winreg)
-----
found 4 completions
```

And you can look at the input like so (for example):

```
$ cat ./crates/ty_completion_eval/truth/internal-typeshed-hidden/main.py
# This is a case where a symbol from an internal module appears
# before the desired symbol from `typing`.
#
# We use a slightly different example than the one reported in
# the issue to capture the deficiency via ranking. That is, in
# astral-sh/ty#1274, the (current) top suggestion is the correct one.
#
# ref: https://github.com/astral-sh/ty/issues/1274#issuecomment-3345923575
NoneTy<CURSOR: types.NoneType>
```

I had considered being more verbose here with snapshots of the actual completion results, but I thought that might be a bit too much. There are a lot of tasks and some tasks generate lots of completions. So it is a bit laborious to validate these.

CI does check that the overall MRR averaged across all tasks doesn't dip below a certain threshold.

When the ranking change like this, I usually look at a few examples (as shown above) to confirm it's what I expect. With a special focus on regressions (when the ranking of the desired result increases).

---

_Comment by @BurntSushi on 2025-12-04 17:32_

> What's our behaviour if `__all__` is assigned or updated in a non-global scope? E.g.
> 
> ```python
> __all__ = []
> 
> def foo():
>     __all__.append("bar")
> 
> class X:
>     def method(self):
>         __all__.extend(["baz"])
> ```
> 
> I might have missed it, but I didn't spot any tests for that. Does our behaviour here align with what we do in `ty_python_semantic`'s inference for `__all__`?

I added a test for it. Auto-import sees `__all__` as empty here, which I think matches what `ty_python_semantic` does. In particular, the auto-import AST scanner will avoid walking into class/function bodies when looking for exported symbols. (It only walks into function/class bodies for document symbols, i.e., when the LSP wants to display a hierarchy of symbols.)

---

_Comment by @BurntSushi on 2025-12-04 17:35_

OK, I believe I've addressed all feedback (or created issues for them). Thanks for the very in depth reviews!!!

---

_Comment by @AlexWaygood on 2025-12-04 17:35_

> I added a test for it. Auto-import sees `__all__` as empty here, which I think matches what `ty_python_semantic` does. In particular, the auto-import AST scanner will avoid walking into class/function bodies when looking for exported symbols. (It only walks into function/class bodies for document symbols, i.e., when the LSP wants to display a hierarchy of symbols.)

yeah, that sounds correct. Though I guess class scopes in the global scope are eagerly executed, so we should probably recognise that `__all__` will be nonempty at the end of this module:

```py
__all__ = []

class Whatever:
    __all__.append("foo")
```

I also wouldn't worry about this too much if we get it wrong currently though:
1. It's a real edge case (why on earth would a person or LLM write code like this?)
2. We probably get this wrong in `ty_python_semantic` too now that I think about it
3. If it hurts performance to check in class scopes inside the global scope (or class scopes inside class scopes inside global scopes, etc.) then it's definitely not worth it

---

_Comment by @BurntSushi on 2025-12-04 17:37_

Yeah I think `ty_python_semantic` also avoids recursing into function/class bodies:

https://github.com/astral-sh/ruff/blob/cccb0bbaa41e95a1efbe997ebe8119454f9b93f9/crates/ty_python_semantic/src/dunder_all.rs#L407-L410

---

_@AlexWaygood reviewed on 2025-12-04 17:41_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:1034 on 2025-12-04 17:41_

strictly speaking `ast::Expr::Named` nodes ("walrus expressions") can also be used to define symbols, including the `__all__` symbol. I wouldn't worry about that here, but it could be worth a TODO comment. We handle this in `ExportFinder` in `ty_python_semantic`, I think

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:1499 on 2025-12-04 17:43_

```suggestion
        // Without `__all__` present, a wildcard import
        // won't include names starting with an underscore at runtime.
        // So `_ZQZQZQ` should not be present here.
        // See: https://docs.python.org/3/reference/lexical_analysis.html#reserved-classes-of-identifiers
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/symbols.rs`:1514 on 2025-12-04 17:43_

```suggestion
        // Without `__all__` present, a wildcard import
        // won't include names starting with an underscore at runtime.
        // So `__ZQZQZQ` should not be present here.
        // See https://docs.python.org/3/reference/lexical_analysis.html#reserved-classes-of-identifiers
```

---

_@AlexWaygood approved on 2025-12-04 17:44_

Thank you -- fantastic work!

---

_@BurntSushi reviewed on 2025-12-04 17:48_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:1034 on 2025-12-04 17:48_

Aye I see yeah. I've added a TODO comment for now.

---

_Merged by @BurntSushi on 2025-12-04 18:21_

---

_Closed by @BurntSushi on 2025-12-04 18:21_

---

_Branch deleted on 2025-12-04 18:21_

---
