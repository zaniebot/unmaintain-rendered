```yaml
number: 20721
title: "[ty] Rename `extra-paths` to `non-environment-paths`, and `--extra-search-path` to `--non-environment-search-path`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - breaking
  - cli
  - ty
assignees: []
base: main
head: alex/rename-extra-paths
created_at: 2025-10-06T14:11:52Z
updated_at: 2025-10-24T17:08:15Z
url: https://github.com/astral-sh/ruff/pull/20721
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Rename `extra-paths` to `non-environment-paths`, and `--extra-search-path` to `--non-environment-search-path`

---

_Pull request opened by @AlexWaygood on 2025-10-06 14:11_

## Summary

This PR renames ty's `--extra-search-path` CLI flag to `--non-environment-search-path`, and renames its `extra-paths` configuration option to `non-environment-paths`.

The motivation is to make the `extra-paths` option less "inviting" for users. It's really an advanced option that should only be used in situations where you have a directory of Python code that wouldn't normally be installed into a virtual environment at runtime in a conventional way, but which you do want ty to consider as a search path for module resolution. There are valid use cases for this option, but users shouldn't be reaching for it before making sure ty knows where their virtual environment is. At this point, I feel like I've seen several user issues that are caused by users thinking that they should be using this option when there are really better ways of achieving what they're trying to achieve.

Of the two, I think renaming the configuration option is more important here; I mainly renamed the CLI flag to keep it somewhat consistent with the configuration name.

Credit goes to @zsol for suggesting the name `non-environment-paths`!

Closes https://github.com/astral-sh/ty/issues/1197
Closes https://github.com/astral-sh/ty/issues/1289

## Test Plan

I grepped for the following phrases to try to make sure my renaming was comprehensive:
- `extra_paths`
- `extra_search`
- `extra-paths`
- `extra-search`
- `extra search`
- `Extra`
- `extra path`

---

_Review requested from @zsol by @AlexWaygood on 2025-10-06 14:11_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-06 14:11_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-06 14:11_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-06 14:11_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-06 14:11_

---

_Label `cli` added by @AlexWaygood on 2025-10-06 14:11_

---

_Label `ty` added by @AlexWaygood on 2025-10-06 14:11_

---

_Comment by @github-actions[bot] on 2025-10-06 14:14_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-06 14:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 53 diagnostics
+ Found 52 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `breaking` added by @MichaReiser on 2025-10-06 14:40_

---

_Review comment by @MichaReiser on `crates/ty/docs/configuration.md`:52 on 2025-10-06 14:41_

I do like the name in general, but I find it confusing in the configuration due to the `environment` configuration table. It's confusing because we use the same word but mean different `environments`.

---

_Comment by @carljm on 2025-10-06 14:43_

I would personally be inclined to not do this. The new name is significantly longer (and I think a less clear and concise description of the option), and I don't think we have clear evidence that the _name_ of the option is what attracts people to this option (vs, say, simply the fact of its existence and that it seems to work). I would rather keep the existing name and see what the impact is of updating documentation.

---

_@MichaReiser reviewed on 2025-10-06 14:45_

Thank you.

It's probably fine to make this breaking change, given that ty's still an alpha and it is hopefully not used often.

I do like the name but it seems problematic in the `environment` table. 

---

_Comment by @AlexWaygood on 2025-10-06 14:55_

> The new name is significantly longer

That was somewhat intentional! The idea is to make the option seem less attractive, after all. A loss of clarity is more problematic, though, for sure.

> I would rather keep the existing name and see what the impact is of updating documentation.

I definitely don't want to rush into something we don't have consensus on. But I do think we keep seeing these issues, and part of the reason for that is that it's very easy to understand what an `extra-paths` configuration option does whereas it doesn't seem nearly as intuitive to our users that specifying `python` may add multiple search paths for module resolution. And the disadvantage of waiting too long, as Micha says, is that it will become harder to become breaking changes once ty moves from alpha to beta, and from beta to general-release

---

_Comment by @carljm on 2025-10-06 14:56_

Pyright uses the name "extraPaths", and mypy uses simply "mypy-path" (which if anything is even more attractive, as it sounds like the primary way to configure search paths, it doesn't even include "extra"), so it doesn't seem likely to me that the name is the key factor here.

I don't think people are discovering this option from blind trial and error, they are finding it in `--help` or the docs, so changes there should be quite effective.

---

_Comment by @MichaReiser on 2025-10-06 14:59_

> I don't think people are discovering this option from blind trial and error, they are finding it in --help or the docs, so changes there should be quite effective.

That's a good point and @AlexWaygood made some great improvements. We could wait and see how it plays out?

> as Micha says, is that it will become harder to become breaking changes once ty moves from alpha to beta, and from beta to general-release

I think this option shouldn't be too hard. We can just add aliases on the CLI and the implementation at the configuration should look similar to what i've done for `src.root` (including a deprecation diagnostic)

---

_Comment by @AlexWaygood on 2025-10-06 15:00_

> Pyright uses the name "extraPaths", and mypy uses simply "mypy-path" (which if anything is even more attractive, as it sounds like the primary way to configure search paths, it doesn't even include "extra")

Right, though as we've discussed in the past, it's generally much less important to specify the Python environment for either of those, since mypy is usually invoked from inside your project's Python environment, and pyright is usually invoked from inside VSCode (which prompts you to select your project's Python interpreter when you open a project for the first time). So I would expect that users of those type checkers just don't need to look for an option to set extra search paths in the same way that ty users might.

Anyway, we can wait on this for now and see if we get any more issues about this in the coming months 👍

---

_Closed by @AlexWaygood on 2025-10-06 15:00_

---

_Comment by @AlexWaygood on 2025-10-24 15:49_

Another example of a user immediately reaching for `environment.extra-paths` when (from what I can see) `environment.roots` would be more appropriate: https://github.com/astral-sh/ty/issues/1413#issuecomment-3443606009

---

_Comment by @rgasper on 2025-10-24 17:08_

> Another example of a user immediately reaching for `environment.extra-paths` when (from what I can see) `environment.roots` would be more appropriate: [astral-sh/ty#1413 (comment)](https://github.com/astral-sh/ty/issues/1413#issuecomment-3443606009)

I did actually try `environment.roots` first, but it didn't seem to work - turns out that was because I was editing that in the wrong `pyproject.toml`, which was my IDE's (helix) fault, not `ty`

---
