```yaml
number: 15204
title: "[`flake8-type-checking`] Improve flexibility of `runtime-evaluated-decorators`"
type: pull_request
state: merged
author: Daverball
labels:
  - configuration
assignees: []
merged: true
base: main
head: feat/runtime-evaluated-decorators-glob
created_at: 2024-12-30T16:28:12Z
updated_at: 2024-12-31T16:31:57Z
url: https://github.com/astral-sh/ruff/pull/15204
synced_at: 2026-01-10T20:42:27Z
```

# [`flake8-type-checking`] Improve flexibility of `runtime-evaluated-decorators`

---

_Pull request opened by @Daverball on 2024-12-30 16:28_

This is an alternative proposal towards accomplishing the use-case #15060 is trying to provide, which doesn't require adding a new setting.

## Summary

This slightly changes semantics of the setting to support a common pattern used by e.g. `FastAPI`. We will resolve assignments so you can do things like this:

```python
from __future__ import annotations

import fastapi

from mymodule import Foo

app = fastapi.FastAPI("My App")

@app.get("/home")
def home() -> Foo: ...
```

And add `fastapi.FastAPI.get` to `runtime-evaluated-decorators` in order to prevent a `TC001` violation for `from mymodule import Foo`.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-12-30 16:35_

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

_Comment by @MichaReiser on 2024-12-30 17:03_

It seems I was a few minutes too slow. I replied on #15060 

---

_Comment by @Daverball on 2024-12-30 17:12_

@MichaReiser No worries, the glob part is honestly the smaller part of this proposal. You can make it work without the glob support. It just seemed really annoying to me having to manually list all the decorators. With some frameworks you may very well be looking at multiple dozens of methods. The decorator as application configuration hook style is quite popular.

So I can take it back out if we're sure we really don't want it.

---

_Comment by @MichaReiser on 2024-12-30 17:15_

Thanks @Daverball My preference would be to start without it and add it if users run into it. We then also have the option to a) allow regex or b) do a prefix match 

---

_Renamed from "[`flake8-type-checking`] Allow globs in `runtime-evaluated-decorators`" to "[`flake8-type-checking`] Improve flexibility of `runtime-evaluated-decorators`" by @Daverball on 2024-12-30 17:23_

---

_@MichaReiser reviewed on 2024-12-30 17:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-30 17:31_

Could we use `semantic.lookup_attribute`?

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-30 17:41_

I would've liked to, but it only gives us the fully qualified name, but we also need the name node at the bottom, so we can `resolve_assignment` on it.

I also considered shifting this complexity into `resolve_assignment`, or adding a new function to `analyze::typing`, but that wouldn't really help us reuse `lookup_attribute` either.

---

_@Daverball reviewed on 2024-12-30 17:41_

---

_Comment by @Daverball on 2024-12-30 18:38_

The only thing I dislike about this approach, is that it forces you to write two separate entries for the same module and separate module case.

I also considered adding an option to `resolve_qualified_name` that would return a `Some(QualifiedName)` for `Assignment` and `AnnAssignment` as a band-aid for not having full cross-module type inference. That would bring it back down to one entry, but each instance would need to be ignored separately.

---

_@MichaReiser reviewed on 2024-12-31 10:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-31 10:05_

Yeah, our semantic model API is a bit annoying here. 

So, the "cheap" solution here would be to do the same as `is_logger_candidate` where we match on the `UnqualifiedName::from_expr` directly. 

Or we keep the "clever" solution that tries to follow at least one alias. This makes me wonder if this logic should be moved into `resolve_assignment`. I think it would make the API more intuitive overall because it isn't very intuitive that the first argument must be a name and we have multiple rules now that manually unroll one level of attributes. 

I think it could help performance to use a `SmallVec` with at least a size of 3 to avoid allocating in the common case.

---

_Comment by @MichaReiser on 2024-12-31 10:06_

> The only thing I dislike about this approach, is that it forces you to write two separate entries for the same module and separate module case.

Can you expand on what you mean by this

---

_Comment by @Daverball on 2024-12-31 10:49_

> Can you expand on what you mean by this

Sure, if you have a look at the new tests, you'll notice that I'm adding an entry for `fastapi.FastAPI.get` for the case that uses `resolve_assignment` but also `module.app.app.get` for the regular import case from another module, which assumes that whatever you're importing is something that would either be a class or function defined in the module you're importing from, even though it could just be regular assignment as well.

So `fastapi.FastAPI.get` works for decorators in the same file. but if you import `app` into another file you need a second entry in order for ruff to do the correct thing.

---

_@Daverball reviewed on 2024-12-31 10:58_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-31 10:58_

We could generate a faux-`QualifiedName` when `resolve_qualified_name` fails from `UnqualifiedName` prefixed with the current module (using the same logic as `resolve_qualified_name` uses for class/function definitions)

That would get rid of the need of having to add two entries to support both the same/foreign module use-case, i.e. in the test you'd only need `module.app.app.get` and not `fastapi.FastAPI.get` as well. The only drawback would be that you'd need an entry for each app instance. But you would need that anyways if you share the instances across module boundaries.

The only part of that, which feels a bit icky is, that red knot will be able to infer the fully qualified name even across module boundaries. So eventually the correct entry will be `fastapi.FastAPI.get` rather than `module.app.app.get`.

---

_Comment by @MichaReiser on 2024-12-31 11:20_

> Sure, if you have a look at the new tests, you'll notice that I'm adding an entry for fastapi.FastAPI.get for the case that uses resolve_assignment but also module.app.app.get for the regular import case from another module, which assumes that whatever you're importing is something that would either be a class or function defined in the module you're importing from, even though it could just be regular assignment as well.

Thanks, that makes sense, considering that Ruff can't see what `module.app.App` resolves to. The problem is similar to why we have the [`typing-modules`](https://docs.astral.sh/ruff/settings/#lint_typing-modules), [`logger_objects`](https://docs.astral.sh/ruff/settings/#lint_logger-objects), and [`gettext.extend-function-names`](https://docs.astral.sh/ruff/settings/#lint_flake8-gettext_extend-function-names) settings. This makes me wonder if we need a more generic approach that allows mapping qualified names, e.g., `module.app.App -> `fastapi.FastAPI`. 

---

_@MichaReiser reviewed on 2024-12-31 11:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-31 11:26_

I could see us returning a `Result` from `resolve_qualified_name` where the `Err` returns an `UnqualifiedName`, but I don't think we should make up qualified names; this seems somewhat error-prone. But I think we should still try to resolve the `UnqualifiedName`, e.g if you have:

```
import fastapi

a = fastapi

a.FastAPI # the full qualified name should resolve to `fastapi.FastAPI`
```

Which is what `resolve_assignment` does today, at least if you call it with `a` (but not if called with `a.FastAPI`)

---

_@Daverball reviewed on 2024-12-31 11:50_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-31 11:50_

Yes, that's why it feels icky to do that, but it would solve the issue of having redundant entries.

Although your proposed generic solution for an additional setting to map qualified names seems more robust. Especially once we transition to red knot as the backend where we no longer need to do that mapping manually.

The only problem with the generic solution is that we either end up having to add the same translation call after all of the `resolve_qualified_name` calls that benefit from this translation or we build it into `resolve_qualified_name` itself and end up with a potentially fairly big overhead if the list of mappings is large.

---

_@MichaReiser reviewed on 2024-12-31 12:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-31 12:25_

We might be able to reduce the overhead by, e.g., using [`matchit`](https://docs.rs/matchit/latest/matchit/) similar to what we do when resolving configurations, but I haven't fully thought that through. I'd say the good thing is the list will be small for most cases, so it might not be as big of a perf problem.

But I think we can tackle the extra setting separately. What we need in this PR is to resolve to `module.app.App` so that it can later be rewritten to `fastapi.FastAPI`

---

_@Daverball reviewed on 2024-12-31 12:48_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-31 12:48_

Yup I agree. I will switch to `SmallVec`, move the attribute logic to `resolve_assignment` and clean up any call sites that do the manual 1-wide attribute extension. Unless you want to do that refactor in a separate PR as well.

---

_@MichaReiser reviewed on 2024-12-31 12:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-31 12:57_

I think doing it in this PR is fine. There are only five call-sites but it's up to you

---

_@Daverball reviewed on 2024-12-31 13:43_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:108 on 2024-12-31 13:43_

Turns out the other call sites don't really need to change, the only one that might slightly benefit is the airflow rule, although it would make the logic for the replacement more complicated, so I left it as is for now.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:105 on 2024-12-31 14:05_

Nit: It might help future readers to add a small example here
```suggestion
						// ```python
						// from fastapi import FastAPI
						// @FastAPI.get
						// def test(): ...
						//  ```
            .resolve_qualified_name(expression)
            // if we can't resolve the name, then try resolving the assignment
						// ```python
						// from fastapi import FastAPI
						// app = FastAPI()
						// 
						// @app.get
						// def test(): ...
						// ```
            .or_else(|| analyze::typing::resolve_assignment(expression, semantic))
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:1020 on 2024-12-31 14:07_

Nit: We could consider adding an `extend` method` to reduce the code

---

_@MichaReiser approved on 2024-12-31 14:10_

Nice, thank you. I think it would be great if we could extend the setting documentation to cover both using a framework decorator but also an instance method from a symbol exported from a user module

---

_Label `configuration` added by @MichaReiser on 2024-12-31 14:11_

---

_@Daverball reviewed on 2024-12-31 15:49_

---

_Review comment by @Daverball on `crates/ruff_python_semantic/src/analyze/typing.rs`:1020 on 2024-12-31 15:49_

I struggled a bit to get the lifetimes correct. If there's a more idiomatic way to write the function, feel free to go ahead and fix it. But I definitely like how it simplified `resolve_assignment`, I also noticed that the two match arms were redundant, so I consolidated them.

---

_@MichaReiser approved on 2024-12-31 16:23_

Thanks

---

_@MichaReiser reviewed on 2024-12-31 16:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:1020 on 2024-12-31 16:24_

The trick was to use `into_iterator` so that the items have the type `&'a str` and not `&&'a str`. The alternative would have been to use `iter().copied()`, I think, which dereferences `&&'a str` to `&'a str`

---

_Merged by @MichaReiser on 2024-12-31 16:28_

---

_Closed by @MichaReiser on 2024-12-31 16:28_

---

_Branch deleted on 2024-12-31 16:28_

---
