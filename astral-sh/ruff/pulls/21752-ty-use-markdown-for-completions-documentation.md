```yaml
number: 21752
title: "[ty] Use markdown for completions documentation"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: completion-docstrings
created_at: 2025-12-02T14:48:19Z
updated_at: 2025-12-27T00:22:10Z
url: https://github.com/astral-sh/ruff/pull/21752
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Use markdown for completions documentation

---

_Pull request opened by @MatthewMckee4 on 2025-12-02 14:48_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Make documentation markdown for completions, which, in zed, moves the documentation to the right of the completions panel, rather than inside it.

Also add documentation for class and function definitions

<img width="885" height="367" alt="image" src="https://github.com/user-attachments/assets/784c0f78-9cae-4a71-a27f-6e8101043b59" />

## Test Plan

e2e test.

---

_Review requested from @carljm by @MatthewMckee4 on 2025-12-02 14:48_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-12-02 14:48_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-12-02 14:48_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-12-02 14:48_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-12-02 14:48_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 14:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 14:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 496 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 44 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5814 diagnostics
+ Found 5815 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @Gankra on `crates/ty_server/src/server/api/requests/completion.rs`:116 on 2025-12-02 14:57_

Hmm don't we need to consult whether markdown is supported, like other places that do this?

---

_@Gankra had review dismissed on 2025-12-02 14:58_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:514 on 2025-12-02 15:04_

I am pretty worried about doing this eagerly for every single symbol in an environment. I did some very ad hoc testing to compare perf here. On `main` with this `pyproject.toml`:

```toml
[project]
name = "ex003-reexport-simple"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "matplotlib>=3.10.7",
    "numpy>=2.3.4",
    "pandas>=2.3.3",
    "scikit-learn>=1.7.2",
    "scipy>=1.16.3",
    "whenever>=0.9.3",
]
```

And this Python source file:

```python
array
```

With my cursor right after `array`.

I get this in the trace logs after requesting completions at my cursor position a few times:

```
2025-12-02 09:59:31.888839440 DEBUG request{id=13 method="textDocument/completion"}: Completions request returned 1582 suggestions in 177.106611ms
2025-12-02 09:59:34.297544175 DEBUG request{id=15 method="textDocument/completion"}: Completions request returned 1582 suggestions in 56.358747ms
2025-12-02 09:59:36.489098862 DEBUG request{id=17 method="textDocument/completion"}: Completions request returned 1582 suggestions in 57.155421ms
2025-12-02 09:59:38.932666244 DEBUG request{id=20 method="textDocument/completion"}: Completions request returned 1582 suggestions in 57.014757ms
2025-12-02 09:59:42.342361845 DEBUG request{id=22 method="textDocument/completion"}: Completions request returned 1582 suggestions in 58.887699ms
2025-12-02 09:59:45.683431792 DEBUG request{id=24 method="textDocument/completion"}: Completions request returned 1582 suggestions in 57.7364ms
```

Now with this PR, I do the same test and I get:

```
2025-12-02 10:02:13.280166325 DEBUG request{id=13 method="textDocument/completion"}: Completions request returned 1582 suggestions in 139.561616ms
2025-12-02 10:02:16.136079062 DEBUG request{id=15 method="textDocument/completion"}: Completions request returned 1582 suggestions in 73.513749ms
2025-12-02 10:02:18.525222384 DEBUG request{id=17 method="textDocument/completion"}: Completions request returned 1582 suggestions in 71.187676ms
2025-12-02 10:02:20.888572941 DEBUG request{id=19 method="textDocument/completion"}: Completions request returned 1582 suggestions in 71.481457ms
2025-12-02 10:02:23.408312653 DEBUG request{id=21 method="textDocument/completion"}: Completions request returned 1582 suggestions in 70.057387ms
2025-12-02 10:02:26.422923788 DEBUG request{id=23 method="textDocument/completion"}: Completions request returned 1582 suggestions in 72.955201ms
```

Which seems like a noticeable dip to me. And this probably isn't that big of a Python project either.

So I think I'd prefer to hold off on doing this eagerly with auto-import specifically. I think it looks like that should be done via a resolve request.

An alternative that still involves eagerly getting the docstring is to:

1. Only do it for symbols that match the query given.
2. Perhaps only do it for the first N such symbols.

---

_Label `server` added by @AlexWaygood on 2025-12-02 15:05_

---

_Label `ty` added by @AlexWaygood on 2025-12-02 15:05_

---

_@MichaReiser reviewed on 2025-12-02 15:07_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:116 on 2025-12-02 15:07_

Yes, we have to check the completion specific capability:

```
/**
		 * Client supports the follow content formats for the documentation
		 * property. The order describes the preferred format of the client.
		 */
		documentationFormat?: [MarkupKind](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#markupContent)[];
```

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/completion.rs`:113 on 2025-12-02 15:08_

cc @Gankra for this change.

---

_@BurntSushi requested changes on 2025-12-02 15:08_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-02 15:15_

---

_@Gankra reviewed on 2025-12-02 15:38_

---

_Review comment by @Gankra on `crates/ty_server/src/capabilities.rs`:288 on 2025-12-02 15:38_

`unwrap_or(false)` might be a bit more clear here but not a big deal

---

_@MatthewMckee4 reviewed on 2025-12-02 15:39_

---

_Review comment by @MatthewMckee4 on `crates/ty_server/src/capabilities.rs`:288 on 2025-12-02 15:39_

Most of these flag checks use `unwrap_or_default`

---

_@MatthewMckee4 reviewed on 2025-12-02 16:00_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/symbols.rs`:514 on 2025-12-02 16:00_

Yeah okay, resolve request sounds a lot better.

I can revert these changes and add them to another PR adding resolve completion. Do you think we should defer the auto import too?

 

---

_@BurntSushi reviewed on 2025-12-02 16:03_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:514 on 2025-12-02 16:03_

Thanks! What do you mean by defer the auto import too?

---

_@MatthewMckee4 reviewed on 2025-12-02 16:05_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/symbols.rs`:514 on 2025-12-02 16:05_

Sorry, I mean defer generating the edit for unimported symbols.

I'm not sure how expensive this is.

---

_@BurntSushi reviewed on 2025-12-02 16:13_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:514 on 2025-12-02 16:13_

I would leave that for now personally. It isn't obviously slow to me, and I wrote it to amortize most work so that the unit of work done per completion is pretty small (but still not fully optimal from a quick glance).

---

_Renamed from "Use markdown for completions docstring" to "[ty] Use markdown for completions documentation" by @MatthewMckee4 on 2025-12-02 16:31_

---

_Comment by @MatthewMckee4 on 2025-12-23 09:55_

bump!

---

_Review requested from @Gankra by @MatthewMckee4 on 2025-12-23 09:55_

---

_@MichaReiser approved on 2025-12-23 10:54_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-12-23 10:55_

---

_Comment by @MichaReiser on 2025-12-23 10:56_

@MatthewMckee4 is my understanding correct that you reverted the changes that introduced the performance regression (I just want to make sure that's the case before I by-pass our merge rules and overrule @BurntSushi's requested changes 😅)

---

_Comment by @MatthewMckee4 on 2025-12-23 13:46_

Yeah, this change doesn't change any semantics of the completions now.

Purely rendering change.

---

_Merged by @MichaReiser on 2025-12-23 16:07_

---

_Closed by @MichaReiser on 2025-12-23 16:07_

---

_Branch deleted on 2025-12-27 00:22_

---
