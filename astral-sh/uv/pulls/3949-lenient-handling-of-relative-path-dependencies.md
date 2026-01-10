```yaml
number: 3949
title: Lenient handling of relative path dependencies
type: pull_request
state: closed
author: idlsoft
labels: []
assignees: []
base: main
head: relpath-dependencies
created_at: 2024-05-31T21:11:18Z
updated_at: 2024-07-18T20:50:59Z
url: https://github.com/astral-sh/uv/pull/3949
synced_at: 2026-01-10T13:42:52Z
```

# Lenient handling of relative path dependencies

---

_Pull request opened by @idlsoft on 2024-05-31 21:11_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

`uv` doesn't handle relative paths in dependencies very well.

This  came to my attention while using `rye`.
In my `pyproject.toml` files I would have dependencies like
```
"mylib @ ./../mylib"
```
But `uv` would complain about the path not being absolute.

I wrote a unittest to demostrate the problem.

I believe the reason for it failing is in couple of places, deserializing `LenientRequirement` either from a `pyproject.toml` or from wheel metadata. In both cases it ends up calling `Requirement::from_str()` and fails.

I'll try to see if I can write a fix, would welcome any feedback.


## Test Plan

The test currently fails, should turn green when a fix is implemented


---

_Comment by @codspeed-hq[bot] on 2024-05-31 21:17_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/idlsoft:relpath-dependencies)

### Merging #3949 will **not alter performance**

<sub>Comparing <code>idlsoft:relpath-dependencies</code> (5ed28ad) with <code>main</code> (ef43bcb)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @charliermarsh on 2024-05-31 21:41_

This is intentional though -- relative paths are not allowed under PEP 508, and requirements in `pyproject.toml` have to follow PEP 508. Allowing relative paths here would be a spec violation unfortunately.

---

_Comment by @idlsoft on 2024-05-31 21:56_

> This is intentional though -- relative paths are not allowed under PEP 508, and requirements in `pyproject.toml` have to follow PEP 508. Allowing relative paths here would be a spec violation unfortunately.

Yes I did notice comments on that subject, but also a bunch of `#[cfg(feature = "non-pep508-extensions")]` code.
This would ultimately defer to that code, just give it more context.

Are there any other recommended ways to support a monorepo-like project tree?
FWIW hatchling is perfectly happy with it. It also has its own `{root:uri}` and the like, but have `uv` understand all these variations seems hackier.

---

_Comment by @charliermarsh on 2024-06-01 00:34_

Can you say more about what you mean by "FWIW hatchling is perfectly happy with it"? I don't think you can `pip install` a package that uses Hatchling as a backend if that package has a relative path in its `project.dependencies`. Do you mean that Hatchling lets you _build_ a package with a relative path?

---

_Comment by @idlsoft on 2024-06-01 01:57_

Sorry, I guess that didn't make a lot of sense. Yes, it can build it. Which is kinda irrelevant in this context.
The reason I brought it up, is I'm trying to solve a problem that kinda involves both `hatchling` and `uv`.

I have a source tree with python projects, some are libraries, some are deployable apps.
Every python project has its own `pyproject.toml` managed by `rye` with dependencies, both binary and direct to other projects.

I want to be able to:
- create a venv for any project with all its transitive dependencies, preferably pointing directly to sources whenever possible.
  This would help coding, unittesting etc.
- build a publishable wheel/sourcedist for libraries.
  Publishable means that direct dependencies are automatically normalized from `mylib @ ../mylib` to something like `mylib == 1.0.9`.  I already have a hatchling build hook for this, which is probably why I mentioned it.
- collect all dependencies as wheel files sort of like `pip download` for packaging/dockerbuild etc

Ideally I'd like to have something closer to cargo's `mylib { workspace = true }` but then some other tool will complain. Leniency on relative paths seems like a fairly practical solution.

Btw I may be close to turning that unit-test green, hope you'll consider it.

---

_Renamed from "Unittest for relative path dependencies" to "Lenient handling of relative path dependencies" by @idlsoft on 2024-06-01 04:34_

---

_Marked ready for review by @idlsoft on 2024-06-01 04:35_

---

_Comment by @charliermarsh on 2024-06-01 12:30_

I'm undecided on supporting relative paths in `pyproject.toml` -- we plan to support the workflow you're describing with `tool.uv.sources` which allows you to enrich your dependencies in the way you've described above. It already exists today and supports Cargo-like workspaces but it's still in preview and not yet advertised or intended for public use. There are some basic docs around it in `docs/specifying_dependencies.md` but, again, it's under active development.

In the meantime, we _do_ respect `${PROJECT_ROOT}` which is currently an alias for `${PWD}`:

```
"mylib @ file://${PROJECT_ROOT}/../mylib"
```

---

_Comment by @idlsoft on 2024-06-01 15:35_

> I'm undecided on supporting relative paths in pyproject.toml

I would not suggest supporting it officially, more like letting it slide. There are still pitfalls there, especially if the structure is anything but sibling directories.

> It already exists today and supports Cargo-like workspaces but it's still in preview

I actually stumbled upon this later, saw all the `= { workspace = true }` mentions.
This looks great, and obviously a much, much better way of handling it.
There is a lot of potential here.
It would be cool to allow some workspace-related functionality like iterating over dependencies and dependents, for example.
Do you plan to have your own build backend eventually?
You may have to, if you wanted to write fully resolved workspace dependency versions into `METADATA`.
Or if you wanted to have shared default dependency versions in the workspace, shared default constraints, overrides.

> In the meantime, we _do_ respect `${PROJECT_ROOT}` which is currently an alias for `${PWD}`:

I tried that, but this gets `hatchling` upset. I tries to dereference everything that looks like`{...}` according to its own rules. There is also a potential issue of `rye lock` generating non portable lock files.

Btw it turns out my PR breaks `${PROJECT_ROOT}`, probably because there is a relative->absolute path conversion somewhere before it gets dereferenced.
I'll look into it.
... I used it without `file://`, maybe that's not meant to work, but it was still happier before my changes.
... looked into it, turns out `rye` removes the leading `/` to make `file:///${PROJECT_ROOT}` work, so no breakages.


---

_Comment by @noamraph on 2024-06-09 08:19_

@charliermarsh @idlsoft sorry for jumping into the PR, but perhaps it will be relevant. I'm looking for a solution for the exact same issue. I have a question: it seems to me that using relative paths is already supported when using `requirements.in` files, when using `-r ../path/to/other/requirements.in`. Am I correct? Does it mean that a reasonable way to achieve a Cargo-like 
 workspace using what's stable right now is to use `requirements.in` and `uv pip compile`?

Thanks!



---

_Comment by @idlsoft on 2024-07-18 20:50_

The use-case is supported using
```toml
[tool.uv.sources]
shared_lib = { path = "../mylib", editable = true}
```

---

_Closed by @idlsoft on 2024-07-18 20:50_

---
