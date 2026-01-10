```yaml
number: 19471
title: "[ty] Implement non-stdlib stub mapping for classes and functions"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/remod
created_at: 2025-07-21T18:28:05Z
updated_at: 2025-07-22T12:50:36Z
url: https://github.com/astral-sh/ruff/pull/19471
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Implement non-stdlib stub mapping for classes and functions

---

_Pull request opened by @Gankra on 2025-07-21 18:28_

This implements mapping of definitions in stubs to definitions in the "real" implementation using the approach described in https://github.com/astral-sh/ty/issues/788#issuecomment-3097000287

I've tested this with goto-definition in vscode with code that uses `colorama` and `types-colorama` (will put up a screen recording and hopefully tests after lunch).

Notably this implementation does not add support for stub-mapping stdlib modules, which can be done as an essentially orthogonal followup in the implementation of `resolve_real_module`.

Partially fixes https://github.com/astral-sh/ty/issues/788

---

_Review requested from @carljm by @Gankra on 2025-07-21 18:28_

---

_Review requested from @AlexWaygood by @Gankra on 2025-07-21 18:28_

---

_Review requested from @sharkdp by @Gankra on 2025-07-21 18:28_

---

_Review requested from @dcreager by @Gankra on 2025-07-21 18:28_

---

_Review requested from @MichaReiser by @Gankra on 2025-07-21 18:28_

---

_Label `server` added by @Gankra on 2025-07-21 18:28_

---

_Label `ty` added by @Gankra on 2025-07-21 18:28_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:89 on 2025-07-21 18:29_

I duplicated this in case I'd need to tweak the behaviour, but I never really did. I can/should just make resolve_module_query take two arguments.

---

_@Gankra reviewed on 2025-07-21 18:29_

---

_@Gankra reviewed on 2025-07-21 18:30_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1155 on 2025-07-21 18:30_

I simply haven't even tried to think about these.

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1197 on 2025-07-21 18:30_

Similarly, didn't even try to think about these.

---

_@Gankra reviewed on 2025-07-21 18:30_

---

_@Gankra reviewed on 2025-07-21 18:32_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1095 on 2025-07-21 18:32_

Weirdly this always seems to return two copies of the same result? It works out fine because the IDE deduplicates it.

---

_Comment by @github-actions[bot] on 2025-07-21 18:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@Gankra reviewed on 2025-07-21 18:33_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1079 on 2025-07-21 18:33_

I believe this path is currently only used for going to a function keyword argument or something like that?

---

_@AlexWaygood reviewed on 2025-07-21 18:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:136 on 2025-07-21 18:37_

this looks like it's still only used inside this crate, and I'm kinda leery about exposing it outside this crate

```suggestion
pub(crate) fn file_to_module(db: &dyn Db, file: File) -> Option<Module> {
```

---

_Comment by @github-actions[bot] on 2025-07-21 18:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Comment by @AlexWaygood on 2025-07-21 18:46_

It looks like for something like this, where there isn't a separate `*-stubs` package but instead the stub lives alongside the runtime files in the same package, we still jump to the stub file in go-to definition on this branch:

`main.py`:

```py
from module import Foo
```

`module.py`:

```py
class Foo: ...
```

`module.pyi`:

```py
class Foo: ...
```

E.g. I ran the playground locally with ^those files on this branch, and used go-to definition on the `Foo` symbol in `main.py`, and it jumped to `module.pyi` rather than `module.py`.

That said, it's something of a distinct task from the "separate `*-stubs` package" task, so I don't think it should block this PR being merged!

---

_@AlexWaygood reviewed on 2025-07-21 18:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:1015 on 2025-07-21 18:48_

I'm choosing to take the insinuation that stubs are somehow "fake" or "inauthentic" as a personal affront, since I'm a typeshed maintainer 😆

---

_@Gankra reviewed on 2025-07-21 19:29_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:136 on 2025-07-21 19:29_

good catch, gunk from initial prototyping.

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:89 on 2025-07-21 19:41_

Ah right, merging the two is annoying because our testing infra checks these queries and really can't deal with queries with >1 arguments.

---

_@Gankra reviewed on 2025-07-21 19:42_

---

_@Gankra reviewed on 2025-07-21 19:43_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1015 on 2025-07-21 19:43_

Only in the artifice can we find perfection

---

_@Gankra reviewed on 2025-07-21 20:40_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_definition.rs`:168 on 2025-07-21 20:40_

Disappointed and annoyed that the stub mapper logic doesn't seem to work on the cursor tests? Debugging...

---

_Comment by @Gankra on 2025-07-21 21:01_

Tests added

---

_Comment by @AlexWaygood on 2025-07-21 21:32_

Here's a slightly pathological situation where this branch still has... questionable? behaviour (but it's an edge case, so maybe not hugely important):

---

`main.py`:

```py
from module import Foo
```

`module/__init__.py`:

```py
# empty file
```

`module/Foo.py`:

```py
# empty file
```

`module.pyi`:

```py
class Foo: ...
```

---

If you go-to-definition on `Foo` in `main.py`, we currently jump to the `class Foo: ...` definition in `module.pyi` on this branch. But arguably the "runtime definition" of `Foo` is the submodule `module/Foo.py` (that's what will actually be imported by the statement `from module import Foo` at runtime!).

---

_Review comment by @carljm on `crates/ty_ide/src/stub_mapping.rs`:26 on 2025-07-21 21:40_

There are several `#[allow(...)]` annotations above (on this method and on `new()`) that can be removed now.

I'm still not convinced we're getting much value from having `StubMapper` as a struct (rather than just a pair of free functions), but I don't think it matters much either way.

---

_Comment by @Gankra on 2025-07-21 21:57_

Not sure how you were testing that -- in the cursor tests it actually yields "no goto (definition) target found". If you created this is an IDE and e.g. cmd+clicked, you may have just unknowingly triggered goto-declaration (if there's no results for goto-definition, goto-declaration is used by the IDE).

I've pushed up a commit adding a test for this.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:1079 on 2025-07-21 22:00_

Looks like it -- but those also have a `Definition`, so it should be possible to stop using `FileWithRange` for that case.

It seems like maybe `FileWithRange` should be more finely targeted to only the "entire module" case -- in every other case I'd think we should have a `Definition`.

---

_Comment by @Gankra on 2025-07-21 22:02_

> It looks like for something like this, where there isn't a separate *-stubs package but instead the stub lives alongside the runtime files in the same package, we still jump to the stub file in go-to definition on this branch:

I also just added a test for this case -- it's fixed now. In an earlier revision I just wasn't propagating ModuleResolveMode deep enough so simple cases still were grabbing .pyi results.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:1095 on 2025-07-21 22:04_

It's because it returns both bindings and declarations (and pushes them onto the returned vector separately), and many definitions (including the ones you support here, class and function definitions) are both bindings and declarations.

This is fixable, but maybe not worth it if the IDE deduplicates anyway.

---

_Review comment by @carljm on `crates/ty_ide/src/goto_definition.rs`:328 on 2025-07-21 22:10_

IMO we should not support the case as shown here, because the stub is kind of just lying. There is no `MyClass` in the namespace of `mymodule` (by default -- there would be if someone else happened to have done an `import mymodule.MyClass` somewhere, but this is an unreliable side effect we intentionally don't model.) In other words, I don't think we should eagerly go fetch submodules and treat them as attributes of the parent module for this purpose.

If, on the other hand, there were a `from . import MyClass` line right here, then I think it would make sense to find that as the definition of `MyClass`.

If we are going to include this test, I would suggest adding that import to it.

---

_Review comment by @carljm on `crates/ty_ide/src/goto_definition.rs`:350 on 2025-07-21 22:13_

I find this test (and the wording describing it here) a bit confusing. In an informal sense, I guess you could call `class MyClass: ...` in a `.py` file a "stub" (in that the class has no body, and it uses an ellipsis, like stubs do), but it's not really a "stub" in any sense described by the typing spec, so I wouldn't call it one here. And I'm not sure what value this test adds, because I don't think we would ever treat `class MyClass: ...` any differently from a more complex class definition, for the purposes of goto-definition?

---

_@carljm approved on 2025-07-21 22:14_

Fantastic! More to do here, but this is a great start and covers the cases people will care most about.

---

_Review comment by @Gankra on `crates/ty_ide/src/stub_mapping.rs`:26 on 2025-07-21 22:25_

I agree the StubMapper abstraction isn't really doing anything for us (and I had to put all the code outside of it to use all the features of `ty_python_semantic` that aren't `pub`). For now I'm leaving it here in case more advanced things (or perf concerns) give motivation to it.

---

_@Gankra reviewed on 2025-07-21 22:25_

---

_@Gankra reviewed on 2025-07-21 22:26_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1095 on 2025-07-21 22:26_

Yeah especially given it's only called on the leaf node and not for the internal nodes.

---

_@Gankra reviewed on 2025-07-21 22:27_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1079 on 2025-07-21 22:27_

It actually used to just be `Module(File)` but then i pulled main and it had turned into `FileWithRange` with that extra case. Not sure what's up with that.

---

_@Gankra reviewed on 2025-07-21 22:30_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_definition.rs`:350 on 2025-07-21 22:30_

I think perhaps the nuance is in the distinction between goto-definition and goto-declaration? But if so I still agree this case is dubious -- if we're enforcing that kind of distinction then the goto-definition should be empty here! (In fact it's possible it *should* be empty but my stub_mapping implementation is too naive to notice that).

---

_@carljm reviewed on 2025-07-21 22:37_

---

_Review comment by @carljm on `crates/ty_ide/src/goto_definition.rs`:350 on 2025-07-21 22:37_

I think my comment is kind of driving in the opposite direction here? That is, I think that `class MyClass: ...` in a `.py` file is not in any meaningful sense a "stub". It is a fully valid runtime implementation/definition of `MyClass` (just as much as `class MyClass:` followed by a three-hundred-line body would be), and we shouldn't treat it otherwise. Goto-definition should happily go to it (as it already does). So while I think the tested behavior here is fully correct, I don't really understand what is noteworthy or test-worthy about this case.

The only thing I see here that makes this test not totally redundant with `goto_definition_stub_map_class_ref` above, is that here the cursor is directly on the import name, not on a later reference. That does seem worth testing! But if that's the purpose of the test, then it should be named and commented differently. Name it eg `goto_definition_stub_map_class_import`, and just delete the comment about "both define a stub". And don't use a `...` body in the `.py` file, because that's just confusing.

---

_Review comment by @carljm on `crates/ty_ide/src/goto_definition.rs`:45 on 2025-07-21 22:48_

nit: our usual practice in ty is TODO comments, not FIXME comments (assuming you aren't planning to fix this before landing this PR)

---

_@Gankra reviewed on 2025-07-21 22:49_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_definition.rs`:53 on 2025-07-21 22:49_

I'm pretty sure this is a pre-existing bug earlier in the goto-def pipeline where we just handle import statements a little bit different (notably the next test where we click on a reference to an imported module resolves fine..). Checking in this "broken" case seems worthwhile.

---

_@Gankra reviewed on 2025-07-21 22:50_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_definition.rs`:185 on 2025-07-21 22:50_

Genuinely unsure what's correct here but I can definitely say for certain this behaviour naturally falls out of the fairly naive logic in my implementation.

---

_Review comment by @carljm on `crates/ty_ide/src/goto_definition.rs`:183 on 2025-07-21 22:52_

It might be worth testing how other LSPs (e.g. pylance in VSCode) handle this. I think our handling for this case with both `.py` and `.pyi` should be the same as our handling if there's only the `.py` and no stub-mapping is involved. I believe currently in the latter case we would highlight all definitions, not just the latest one (and that this was an intentional design choice by @UnboundVariable). In general the design choice was that it's confusing for users to highlight different definitions of a symbol depending on location in control flow, and it's better to just highlight all the definitions we can find.

So I think this is probably not a FIXME

---

_@carljm reviewed on 2025-07-21 22:52_

---

_@Gankra reviewed on 2025-07-21 22:52_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_definition.rs`:350 on 2025-07-21 22:52_

I'm gonna leave it in the PR for now so others have a chance to chime in here (I'm fine with whatever).

---

_@UnboundVariable reviewed on 2025-07-21 23:04_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:1079 on 2025-07-21 23:04_

I changed it to FileWithRange when I added support for keyword arguments. @carljm, are you sure those have definitions? I found definitions for keyword _parameters_ but not for keyword _arguments_.

---

_@UnboundVariable reviewed on 2025-07-21 23:06_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:1095 on 2025-07-21 23:06_

We should probably deduplicate internally. Most clients deduplicate, but some of them don't like to receive duplicates. This will become even more important for follow-on features like "find references".

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:1079 on 2025-07-21 23:11_

If I'm reading the code correctly in `definitions_for_keyword_argument`, the source is an argument, but the `ResolvedDefinition` resolves to the corresponding parameter in the callable's signature. That's what the doc comment says, and what the code appears to do.

It is correct that there's no Definition for the argument, but there is one for the parameter. Which seems like it should mean that we can return a `ResolvedDefinition::Definition` there.

---

_@carljm reviewed on 2025-07-21 23:11_

---

_@Gankra reviewed on 2025-07-21 23:24_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1095 on 2025-07-21 23:24_

Added an IndexSet to map_stub_definitions to do deduping at this point in the API.

---

_@carljm reviewed on 2025-07-21 23:36_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:1095 on 2025-07-21 23:36_

I think if we are going to dedupe, the deduping should happen at a different layer (perhaps inside `find_symbol_in_scope`?) so that it applies to both goto-declaration and stub-mapped goto-definition; the issue is not specific to stub-mapping.

---

_@carljm reviewed on 2025-07-21 23:39_

---

_Review comment by @carljm on `crates/ty_ide/src/goto_definition.rs`:350 on 2025-07-21 23:39_

That's fine, but looking into Alex's comment where I think this test came from, I'm pretty sure the fact that both definitions use `class Foo: ...` in that example is entirely incidental, not part of Alex's point at all. The point of that comment was simply that we were wrongly failing to prefer `module.py` over `module.pyi`. You already fixed that (by pushing `mode` deeper), and plenty of other tests here validate that fix already. So I'm pretty confident this test can be removed or reworked per my comment above, without contradicting anything Alex was suggesting.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:669 on 2025-07-22 06:23_

You could add `mode` to `ResolverContext` instead of funnelying it through all calls (we already do this for `context`)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:1012 on 2025-07-22 06:25_

This is something we discussed on Friday and I'm fine deferring this to another PR but `file_to_module` will fail for every resolved *real module*. That means that import resolution won't work for those files. We may want to change `file_to_module` to always return the module name if file matches the `real_module` (and not only the `resolved_module`)

---

_@MichaReiser reviewed on 2025-07-22 06:29_

---

_Comment by @AlexWaygood on 2025-07-22 11:17_

> Not sure how you were testing that -- in the cursor tests it actually yields "no goto (definition) target found". If you created this is an IDE and e.g. cmd+clicked, you may have just unknowingly triggered goto-declaration (if there's no results for goto-definition, goto-declaration is used by the IDE).
> 
> I've pushed up a commit adding a test for this.

Oh, yeah, I was interactively testing by running the playground locally using your branch. (You can do this by running `cd playground; npm start --workspace ty-playground` from the root of the Ruff repository, then opening http://localhost:5173/ in a browser window. It's a great way to experiment with IDE features!) So it might well have just fallen back to goto-declaration!

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_definition.rs`:328 on 2025-07-22 11:34_

> IMO we should not support the case as shown here, because the stub is kind of just lying.

I can't see the full test that you were commenting on here, but note that there are many places where the stub will be "kind of just lying", where it will be important for us to be able to figure out where the actual, truthful runtime definition of something is. For example, if you do "goto declaration" for `typing.Iterable` in Pylance, it takes you to https://github.com/python/typeshed/blob/ddd8434a42b83923b770450386ee12672ab3c604/stdlib/typing.pyi#L501-L504, but if you do "goto definition", it takes you to https://github.com/python/cpython/blob/c933a6bb329bb97bc7e448388dad1b74f7ca4baa/Lib/typing.py#L2714. The definition is a class in the stub but a variable assignment at runtime -- but being able to jump to the runtime definition is very useful if you're trying to figure out why the runtime behaviour of these aliases is different for some reason!

Similarly, typeshed will usually pretend that `types.SimpleNamespace` instances are instances of custom classes rather than instances of `SimpleNamespace`, since attributes on `SimpleNamespace` instances are untypable

---

_@AlexWaygood reviewed on 2025-07-22 11:34_

---

_@Gankra reviewed on 2025-07-22 12:36_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1095 on 2025-07-22 12:36_

Done in find_symbol_in_scope

---

_@Gankra reviewed on 2025-07-22 12:37_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_definition.rs`:328 on 2025-07-22 12:37_

I'm just gonna remove this test for now.

---

_Merged by @Gankra on 2025-07-22 12:42_

---

_Closed by @Gankra on 2025-07-22 12:42_

---

_Branch deleted on 2025-07-22 12:42_

---
