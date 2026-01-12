```yaml
number: 19620
title: "[ty] Add a flow diagram for Python's import resolution algorithm"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ag/module-resolver-touchups
created_at: 2025-07-29T17:26:10Z
updated_at: 2025-07-30T13:57:07Z
url: https://github.com/astral-sh/ruff/pull/19620
synced_at: 2026-01-12T15:56:43Z
```

# [ty] Add a flow diagram for Python's import resolution algorithm

---

_@BurntSushi_

This PR includes a few other touch-ups that I split out into separate
commits. While the changes are pretty small, it probably makes sense
for reviewers to read this PR commit-by-commit.

The motivation for the flow diagram is to provide a source of truth
about our intended implementation for module resolution. In particular,
in addition to "resolve this specific module name," we also want to add
support for "list module names" in service of completions. Code reuse
between these two is likely difficult, so I wanted to 1) carefully
understand how the former worked and 2) provide a source of truth that
both implementations can target.


---

_Review requested from @carljm by @BurntSushi on 2025-07-29 17:26_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-07-29 17:26_

---

_Review requested from @sharkdp by @BurntSushi on 2025-07-29 17:26_

---

_Review requested from @dcreager by @BurntSushi on 2025-07-29 17:26_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-07-29 17:26_

---

_Review request for @dcreager removed by @BurntSushi on 2025-07-29 17:26_

---

_Review request for @carljm removed by @BurntSushi on 2025-07-29 17:26_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-07-29 17:26_

---

_Comment by @BurntSushi on 2025-07-29 17:27_

An SVG of the flow diagram should show up in the diff review, but here's where you can see it directly:

https://github.com/astral-sh/ruff/blob/89ca1cc83b659eb6b6e43e944587954b4f7d0475/crates/ty_python_semantic/src/module_resolver/import-resolution-diagram.svg

---

_Comment by @github-actions[bot] on 2025-07-29 17:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-29 17:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/import-resolution-diagram.dot`:85 on 2025-07-29 18:00_

one detail that the flowgraph doesn't capture currently is that if it's the standard-library search path then at every stage _only_ `.pyi` files are considered (because something very strange is going on if typeshed has `.py` files in it!!). So here, if this was the standard-library search path, we'd only check to see if every parent package of `module_name` corresponds to a directory that contains an `__init__.pyi` file; we wouldn't do the check for `__init__.py` files.

I'm not totally sure if it's worth trying to add that detail to the flowgraph, because the flowgraph is already pretty complicated, but I thought I'd mention it!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/import-resolution-diagram.dot`:1 on 2025-07-29 18:11_

This flowgraph is very nice, but I have some concerns about maintainability. There are significant features of the module resolver that we know we need to implement but have not yet:
- Currently we have no knowledge of C extensions, but ideally if a C extension was installed without any type stubs we wouldn't say "the module doesn't exist", we'd say "please go and install a stubs package so we can infer proper types when you import from this module". That'll require refactoring `resolve_module` to return a `Result` rather than an `Option` so that we can distinguish between the different error cases
- We don't implement `py.typed` files with `partial` in them yet (if a stubs package is marked as `partial`, you're meant to fallback to the runtime package if you can't find a module in the stubs package)
- @Gankra is adding another search path for the "real" (non-typeshed) standard library in https://github.com/astral-sh/ruff/pull/19529, so that we can implement go-to-definition jumping to the actual definition of a stdlib API rather than the type declaration in typeshed. That's sort-of going to have the opposite semantics to the typeshed search path: whereas the typeshed search path is the one search path where we can guarantee that only `.pyi` files will ever exist, the "real stdlib" is the one search path where we can guarantee that only `.py` files will ever exist.

This diagram could definitely help people adding these new features, but it also means they'll have another task to complete as part of any one of those features: they'll need to update the diagram! And I'm mostly worried that we'll forget to keep it updated, leading to it growing stale

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:5 on 2025-07-29 18:13_

```suggestion
usually want the former, unless you're certain you want to forbid stubs,
in which case, use the latter.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:253 on 2025-07-29 18:15_

We've discussed this a few times since I originally added this TODO and I think we're still on the fence as a team about whether this is something we actually want to do or not. Pyright does it, so language-server users may be expecting it; but vendoring these third-party stubs would increase the size of our binary, and possibly make it harder to reproduce type-checking results across machines. Mypy, and other type checkers that are not primarily designed to be good language servers, generally do not vendor these stubs.

TL;DR: let's add some more doubt here 😄

```suggestion
        // TODO vendor typeshed's third-party stubs as well as the stdlib and
        // fallback to them as a final step?
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:694 on 2025-07-29 18:17_

"module path" here would imply `foo/bar/baz` rather than `foo.bar.baz` to me

```suggestion
/// `name` should be a complete non-empty module name e.g, `foo` or
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:699 on 2025-07-29 18:21_

Strictly speaking, all packages are modules (but not all modules are packages!)

```suggestion
/// module: its kind (single-file module or package), the search path in which it was found
```

---

_@AlexWaygood approved on 2025-07-29 18:25_

Thank you!

I'm a little bit concerned that we'll forget to update the flow diagram when making changes in the future. But I also might be overthinking it. I don't want to block this from being landed if you think it will help future implementors of module-resolver features -- that's definitely the most important aspect!

---

_@BurntSushi reviewed on 2025-07-29 18:30_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/import-resolution-diagram.dot`:85 on 2025-07-29 18:30_

Yeah I did actually notice that. But yeah it would make the flow graph much more complicated. _And_ I interpreted this as a implementation detail for perf reasons. i.e., If you did check to `.py` in std paths, would the correctness change? I tried to leave optimizations out of the flow graph. Another one I left out was the std check here:

https://github.com/astral-sh/ruff/blob/656273bf3d51125911991dc15903606705acf269/crates/ty_python_semantic/src/module_resolver/resolver.rs#L712-L737

---

_@BurntSushi reviewed on 2025-07-29 18:33_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/import-resolution-diagram.dot`:1 on 2025-07-29 18:33_

Yeah that's definitely valid. That's always the problem with docs. :-) I don't think there is a feasible means of doing a machine check here, so this is just something we humans will have to maintain. If it ends up going unmaintained, then we can delete it under the reasoning that it has outlived its usefulness. But I still think it's an important stepping stone to adding what is likely to be a distinct implementation of these semantics via a different access pattern.

---

_@BurntSushi reviewed on 2025-07-29 18:36_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:253 on 2025-07-29 18:36_

Haha fair enough, done. I also linked to this comment. :-)

---

_Comment by @BurntSushi on 2025-07-29 18:49_

@MichaReiser Happy to address any follow-ups if you have them!

---

_Merged by @BurntSushi on 2025-07-29 18:49_

---

_Closed by @BurntSushi on 2025-07-29 18:49_

---

_Branch deleted on 2025-07-29 18:49_

---

_Comment by @AlexWaygood on 2025-07-29 18:57_

Oh, one significant detail not covered in the flow diagram is the fact that for the standard library, we will return `None` from `resolve_module` _even if_ the module exists in the vendored zip file according to the normal rules if typeshed's `VERSIONS` file indicates that the module doesn't exist on the user's configured Python version

---

_Comment by @BurntSushi on 2025-07-29 19:04_

@AlexWaygood Ah I see, interesting. That's something I legitimately missed. It looks like it's primarily implemented in the `ModulePath` routines. I'll work it into the flow diagram. (I also think I should be able to reuse `ModulePath` in the "list modules" implementation.)

---

_Label `ty` added by @ntBre on 2025-07-29 20:14_

---

_Label `ty` removed by @ntBre on 2025-07-29 20:14_

---

_Comment by @BurntSushi on 2025-07-30 13:45_

I opened https://github.com/astral-sh/ruff/pull/19638 to account for `VERSIONS` in typeshed.

---

_Label `internal` added by @AlexWaygood on 2025-07-30 13:57_

---

_Label `ty` added by @AlexWaygood on 2025-07-30 13:57_

---
