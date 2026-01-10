```yaml
number: 15026
title: "Handle nested imports correctly in `from ... import`"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/relative-nested-imports
created_at: 2024-12-16T23:19:20Z
updated_at: 2024-12-17T19:23:37Z
url: https://github.com/astral-sh/ruff/pull/15026
synced_at: 2026-01-10T20:42:27Z
```

# Handle nested imports correctly in `from ... import`

---

_Pull request opened by @dcreager on 2024-12-16 23:19_

#14946 fixed our handling of nested imports with the `import` statement, but didn't touch `from...import` statements.  

cf https://github.com/astral-sh/ruff/issues/14826#issuecomment-2525344515

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2281 on 2024-12-16 23:29_

I took the liberty of reorganizing this logic to use early-return to tease apart the different cases.  I find it easier to reason about this way, but I'm happy to revert back to the previous style if that's house style.  Other than the part I call out below, most of this diff should just be a refactoring.

---

_Comment by @github-actions[bot] on 2024-12-16 23:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2332 on 2024-12-16 23:29_

This stanza is the part that's new

---

_@dcreager reviewed on 2024-12-16 23:31_

---

_Marked ready for review by @dcreager on 2024-12-16 23:31_

---

_Review requested from @carljm by @dcreager on 2024-12-16 23:31_

---

_Review requested from @MichaReiser by @dcreager on 2024-12-16 23:31_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-16 23:31_

---

_Review requested from @sharkdp by @dcreager on 2024-12-16 23:31_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-16 23:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2320 on 2024-12-17 00:05_

What if we did record each `from` import as a potential submodule import in semantic indexing? I think this might work, because when it isn't a submodule, we'd just get a little further into the submodule stanza in `Type::member` before finding we can't resolve the "submodule" and giving up and continuing to a regular module-namespace lookup. Then we wouldn't need to have this stanza both places, but I think also it would make code like this work, which doesn't in the current PR (this is the new code for `bar.py` in the test fixed by this PR):

```py
from . import foo
import pkg

reveal_type(pkg.foo.X)
```

That said: I'm not sure we need to make that code work. It's pretty subtle why it works at runtime, and (just as in the case without the `from . import foo` line) there's no reason you can't change `import pkg` to `import pkg.foo` to make it work explicitly.

And adding every `from` import as a speculative submodule import is going to increase the size of the data we have to store in semantic index, potentially a lot (especially for codebases that mostly use `from` imports of individual objects, which is definitely a style preferred some places).

So I think I'm happy going with this approach for now.

---

_@carljm approved on 2024-12-17 00:06_

Nice!

---

_@carljm reviewed on 2024-12-17 00:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2281 on 2024-12-17 00:08_

I think we (or at least I) have tended towards expression style rather than early return style, probably particularly in this case to avoid the repeat calls to `add_declaration_with_binding`. But I don't really have a strong preference for that, and I agree that in this case your version approves readability. In other words: feel free to improve our house style, that's what you're here for :)

---

_@MichaReiser reviewed on 2024-12-17 08:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2281 on 2024-12-17 08:37_

Just some additional data point: Early returns are used extensively in Ruff. 

---

_@MichaReiser reviewed on 2024-12-17 08:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2322 on 2024-12-17 08:42_

We can use the identifier directly without creating a new name
```suggestion
        if let Some(submodule_name) = ModuleName::new(&name) {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:124 on 2024-12-17 11:36_

I think this means that you can delete the last bullet point of the TODO here:

https://github.com/astral-sh/ruff/blob/7c2e7cf25e2b04f7b52052ed4c0710a593ebe4a1/crates/red_knot_python_semantic/src/types/infer.rs#L2244-L2252

---

_@AlexWaygood approved on 2024-12-17 11:38_

Nice! I like the use of early return

---

_@AlexWaygood reviewed on 2024-12-17 12:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:124 on 2024-12-17 12:57_

Oh, I think you can also get rid of the TODO comment here:

https://github.com/astral-sh/ruff/blob/dcb99cc8171aa579256f53ca3277a6ac524f5224/crates/red_knot_python_semantic/resources/mdtest/import/relative.md?plain=1#L133-L143

I don't think there's actually anything to do there? It looks to me like that test is actually testing that we emit a diagnostic if you attempt a relative import of a nonexistent submodule. So previously it passed for the wrong reasons, but I'm guessing with this PR it now passes for the right reasons...?

It would be great to add a short description to that test that clarifies what it's actually testing 😆

---

_@AlexWaygood reviewed on 2024-12-17 14:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2322 on 2024-12-17 14:03_

Oh, this also doesn't handle invalid syntax like the following well:

```py
# experiment/foo.py
from importlib import metadata.diagnose
reveal_type(metadata)
```

Running red-knot with `--project experiment` gives this output:

```
error[invalid-syntax] /Users/alexw/dev/experiment/foo.py:1:31 Expected ',', found '.'
error[lint:unresolved-import] /Users/alexw/dev/experiment/foo.py:1:32 Module `importlib` has no member `diagnose`
warning[lint:undefined-reveal] /Users/alexw/dev/experiment/foo.py:2:1 `reveal_type` used without importing it; this is allowed for debugging convenience but will fail at runtime
info[revealed-type] /Users/alexw/dev/experiment/foo.py:2:1 Revealed type is `<module 'importlib.metadata'>`
```

This is because `metadata.diagnose` is a valid module name, but not a valid import alias (an import alias must be a valid identifier; it cannot have any `.`s in it).

I think the output we should aim for on this snippet are:

```
error[invalid-syntax] /Users/alexw/dev/experiment/foo.py:1:31 Expected ',', found '.'
warning[lint:undefined-reveal] /Users/alexw/dev/experiment/foo.py:2:1 `reveal_type` used without importing it; this is allowed for debugging convenience but will fail at runtime
info[revealed-type] /Users/alexw/dev/experiment/foo.py:2:1 Revealed type is Unknown
```



---

_@AlexWaygood reviewed on 2024-12-17 14:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2322 on 2024-12-17 14:08_

Though this is at least partly a pre-existing issue; here's the error message we give on `main` for this:

```
error[lint:unresolved-import] /Users/alexw/dev/experiment/foo.py:1:23 Module `importlib` has no member `metadata`
error[invalid-syntax] /Users/alexw/dev/experiment/foo.py:1:31 Expected ',', found '.'
error[lint:unresolved-import] /Users/alexw/dev/experiment/foo.py:1:32 Module `importlib` has no member `diagnose`
warning[lint:undefined-reveal] /Users/alexw/dev/experiment/foo.py:2:1 `reveal_type` used without importing it; this is allowed for debugging convenience but will fail at runtime
info[revealed-type] /Users/alexw/dev/experiment/foo.py:2:1 Revealed type is `Unknown`
```

I'm okay deferring this to a followup!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:33 on 2024-12-17 15:06_

This is no longer needed with https://github.com/astral-sh/ruff/pull/15030 merged.  Thanks @MichaReiser!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:124 on 2024-12-17 15:13_

> So previously it passed for the wrong reasons, but I'm guessing with this PR it now passes for the right reasons...?

Ha!  Yeah that looks right to me...  I was a bit surprised, and also a bit not surprised, to learn that that `from . import foo` syntax works to import a symbol from the current package.  Surprised in that it's a no-op, so why would you do that.  But not surprised in that it seems like the right behavior for that syntax.

Anyway, added a comment to hopefully clarify all of this.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2320 on 2024-12-17 15:28_

> And adding every `from` import as a speculative submodule import is going to increase the size of the data we have to store in semantic index

I was worried less about the size of the data, since there are compressible representations if that turns out to be a problem.  My concern was more about execution time, since we would attempt a submodule resolution every time we look up a member of a module.

Though as I talk through this, I realize it wouldn't be for _every_ module member lookup — just the ones that are involved in a `from...import`.  Which would be a weird pattern to follow:

```py
import unittest
from unittest import mock
reveal_type(unittest.mock)
```

So maybe it wouldn't be so bad?  I do like the idea of these imports being handled by the existing code from #14946, and not needing the additional check.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2322 on 2024-12-17 15:29_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2322 on 2024-12-17 15:37_

I added an mdtest with the current behavior, along with a TODO that we might consider cleaning up the diagnostics.

---

_@dcreager reviewed on 2024-12-17 15:38_

---

_@dcreager reviewed on 2024-12-17 15:51_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2320 on 2024-12-17 15:51_

Ah, but tracking this in the semantic index means that we'd need to resolve relative import paths, which means we'd need to know (when building the semantic index) whether the current file is a module or a package.

---

_@carljm reviewed on 2024-12-17 15:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2320 on 2024-12-17 15:52_

Makes sense! I don't have strong feelings about which way we go here, happy to go with whatever makes more sense to you. I think the most valuable thing is that we record in some form our conclusions from the time we've spent thinking about the two options here :) (The code example I gave above may be worth adding as a test either way.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:133 on 2024-12-17 15:55_

```suggestion
This test verifies that we emit an error when we try to import a symbol that is neither a submodule
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2320 on 2024-12-17 15:56_

> we'd need to resolve relative import paths, which means we'd need to know (when building the semantic index) whether the current file is a module or a package.

Ah! Yeah, that's a problem; we can't have dependencies on information external to the file in semantic indexing.

---

_@carljm approved on 2024-12-17 15:57_

---

_@AlexWaygood reviewed on 2024-12-17 15:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2322 on 2024-12-17 15:58_

Thanks!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:133 on 2024-12-17 18:10_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2320 on 2024-12-17 18:10_

Added that test case and some additional commentary

---

_@dcreager reviewed on 2024-12-17 18:10_

---

_@carljm approved on 2024-12-17 18:19_

Looks great!

EDIT: well, except for the failing test 😆 

---

_Comment by @dcreager on 2024-12-17 19:04_

> well, except for the failing test 😆

Details :smile: 

---

_@carljm approved on 2024-12-17 19:20_

---

_Merged by @dcreager on 2024-12-17 19:23_

---

_Closed by @dcreager on 2024-12-17 19:23_

---

_Branch deleted on 2024-12-17 19:23_

---
