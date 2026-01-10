```yaml
number: 11436
title: "F401 - update documentation and deprecate `ignore_init_module_imports`"
type: pull_request
state: merged
author: plredmond
labels: []
assignees: []
merged: true
base: main
head: ruff.__all__
created_at: 2024-05-15T18:55:43Z
updated_at: 2024-05-21T16:23:46Z
url: https://github.com/astral-sh/ruff/pull/11436
synced_at: 2026-01-10T22:05:26Z
```

# F401 - update documentation and deprecate `ignore_init_module_imports`

---

_Pull request opened by @plredmond on 2024-05-15 18:55_

## Summary

* Update documentation for F401 following recent PRs
  * #11168
  * #11314
* Deprecate `ignore_init_module_imports`
  * Add a deprecation pragma to the option and a "warn user once" message when the option is used.
* Restore the old behavior for stable (non-preview) mode:
  * When `ignore_init_module_imports` is set to `true` (default) there are no `__init_.py` fixes (but we get nice fix titles!).
  * When `ignore_init_module_imports` is set to `false` there are unsafe `__init__.py` fixes to remove unused imports.
  * When preview mode is enabled, it overrides `ignore_init_module_imports`.
* Fixed a bug in fix titles where `import foo as bar` would recommend reexporting `bar as bar`. It now says to reexport `foo as foo`. (In this case we don't issue a fix, fwiw; it was just a fix title bug.)

## Test plan

Added new fixture tests that reuse the existing fixtures for `__init__.py` files. Each of the three situations listed above has fixture tests. The F401 "stable" tests cover:

> * When `ignore_init_module_imports` is set to `true` (default) there are no `__init_.py` fixes (but we get nice fix titles!).

The F401 "deprecated option" tests cover:

> * When `ignore_init_module_imports` is set to `false` there are unsafe `__init__.py` fixes to remove unused imports.

These complement existing "preview" tests that show the new behavior which recommends fixes in `__init__.py` according to whether the import is 1st party and other circumstances (for more on that behavior see: #11314).

---

_Comment by @codspeed-hq[bot] on 2024-05-15 19:00_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ruff.__all__)

### Merging #11436 will **not alter performance**

<sub>Comparing <code>ruff.__all__</code> (6028415) with <code>main</code> (403f0dc)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-05-15 19:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb reviewed on 2024-05-15 19:27_

---

_Review comment by @zanieb on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:38 on 2024-05-15 19:27_

I don't think you need to specify the `__init__.py` part, you can follow the above style.

> Alternatively, you can use `__all__` to declare a symbol as part of the module's public interface,
> as in:

---

_@zanieb reviewed on 2024-05-15 19:30_

---

_Review comment by @zanieb on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:51 on 2024-05-15 19:30_

I'd say something like 

> Applying fixes to `__init__.py` files is currently in preview.

Maybe you can just move this to the start of the next paragraph instead of saying "In preview mode..."?



---

_@zanieb reviewed on 2024-05-15 19:34_

---

_Review comment by @zanieb on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:57 on 2024-05-15 19:34_

I'd say something more like:

> Applying fixes to `__init__.py` files is currently in preview. The fix differs based on the type of the unused import. Ruff will suggest a safe fix to export first-party imports with either a redundant alias or, if already present in the file, an `__all__` entry. If multiple `__all_` declarations are present, Ruff will not offer a fix. Ruff will suggest an unsafe fix to remove third-party and standard library imports — the fix is unsafe because the module's interface changes.

---

_@zanieb reviewed on 2024-05-15 19:36_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:47 on 2024-05-15 19:36_

Just realized I was commenting on a test snapshot :) changes refer to this file.

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:47 on 2024-05-15 20:36_

No worries :) Thanks for your suggestions.

---

_@plredmond reviewed on 2024-05-15 20:36_

---

_Comment by @plredmond on 2024-05-15 20:36_

@zanieb thoughts on how to gracefully deprecate `lint.ignore_init_module_imports`? I can just grep for it and remove it, but that seems like it would break people who have the option in their config files.

---

_Comment by @zanieb on 2024-05-18 17:19_

Here's an example of a deprecated setting:

https://github.com/astral-sh/ruff/blob/a73b8c82a83336d76e69ee779d1f0648d1fdc667/crates/ruff_workspace/src/options.rs#L428-L432

which has a warning at

https://github.com/astral-sh/ruff/blob/6ed2482e27ec27fa9a6a812cff52efa1c360aded/crates/ruff_workspace/src/configuration.rs#L423-L430

I believe this one was deprecated in https://github.com/astral-sh/ruff/pull/8082 if that's helpful. I'm actually not entirely sure why we define a message for the deprecation _and_ manually include a separate warning. Perhaps because it was renamed? I'd have to play with this a bit to find the right usage.

I think we should still respect the setting on stable but you can ignore it in preview. In both cases, we should display a warning. In the future, we can make using the setting an error during configuration loading.

---

_@plredmond reviewed on 2024-05-20 23:18_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:271 on 2024-05-20 23:18_

These three lines look fishy because I'd originally replaced this
```rust
    let fix_init = !checker.settings.ignore_init_module_imports;
```
with
```rust
    let fix_init = checker.settings.preview.is_enabled();
```
which was perhaps lazy. In this PR I'm restoring the old behavior, and so gave `checker.settings.preview.is_enabled();` its own name.

---

_@plredmond reviewed on 2024-05-20 23:22_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:27 on 2024-05-20 23:22_

This field is required to make fix titles follow the "old" behavior in `__init__.py` files. When we finally remove the option `ignore_init_module_imports` then this field `ignore_init_module_imports` can be deleted too.

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:295 on 2024-05-20 23:23_

This section looks like a large change but isn't. I only added `preview_mode` to the disjunction. Turn on `-w` to see the diff w/o whitespace changes.

---

_@plredmond reviewed on 2024-05-20 23:23_

---

_@plredmond reviewed on 2024-05-20 23:25_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:42 on 2024-05-20 23:25_

This is the only change in to an existing snapshot in the whole diff. It was a bug where the fix title contained the binding, instead of the module.

---

_Marked ready for review by @plredmond on 2024-05-20 23:29_

---

_Review requested from @charliermarsh by @plredmond on 2024-05-20 23:32_

---

_@zanieb reviewed on 2024-05-20 23:57_

---

_Review comment by @zanieb on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:88 on 2024-05-20 23:57_

I think these options might get dynamically turned into links during documentation generation, not sure if this will work. I'd just leave it as-is probably.

---

_@zanieb reviewed on 2024-05-21 00:16_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:665 on 2024-05-21 00:16_

I think I'd phrase like...

> The `ignore-init-module-imports` option is deprecated and will be removed in a future release. Ruff's handling of imports in `__init__.py` files has been improved (in preview) and unused imports will always be flagged.

---

_@zanieb reviewed on 2024-05-21 00:22_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:664 on 2024-05-21 00:22_

I think we should warn regardless of the value if we're going to remove the setting, right? I guess it depends if preview is enabled but is that feasible to check here? i.e. if preview is disabled and this is false the suggestion is to try preview mode instead but not everyone can do that since it's a global toggle.

---

_@zanieb approved on 2024-05-21 00:24_

---

_@plredmond reviewed on 2024-05-21 03:48_

---

_Review comment by @plredmond on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:88 on 2024-05-21 03:48_

Willfix

---

_@plredmond reviewed on 2024-05-21 03:49_

---

_Review comment by @plredmond on `crates/ruff_workspace/src/configuration.rs`:665 on 2024-05-21 03:49_

Willfix

---

_@plredmond reviewed on 2024-05-21 03:49_

---

_Review comment by @plredmond on `crates/ruff_workspace/src/configuration.rs`:664 on 2024-05-21 03:49_

Oops! Yes :100: thank you for catching this.

---

_Comment by @charliermarsh on 2024-05-21 11:54_

Deferring to @zanieb, feel free to merge when approved!

---

_@Hnasar reviewed on 2024-05-21 13:56_

---

_Review comment by @Hnasar on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:56 on 2024-05-21 13:56_

I think it's good that ruff has different handling of first vs third party imports, but changing behavior based on the name of the module (containing the import) I find unintuitive and I argue is unpythonic.

By that, I mean: if I have `mod/__init__.py`, is the fix any more or less safe than if I instead named it `mod.py`? No — the `__init__.py` is just an implementation detail and in either case, the module's interface changes with _any_ import changes.

It's not a universally-accepted convention that `__init__.py` should store your entire public API.
For example, it's hinders import times for users of a large package, if the entire public API is placed in `__init__.py`, because then any import of a _submodule_ will trigger an import `__init__.py` and thus an import of _every_ module, if _everything_ is reexported.

At most, we have the typing docs ([Typing documentation: interface conventions](https://typing.readthedocs.io/en/latest/source/libraries.html#library-interface-public-and-private-symbols)) because there's nothing I can find on python.org that discusses this. But given the typing docs guidance, I would find it slightly easier to teach that:

* third party imports: _always_ safe to remove
* first party imports: _unsafe_ to remove if contained within the same package (similar to relative import convention from typing spec). Safe otherwise.

---

_@Hnasar reviewed on 2024-05-21 13:58_

---

_Review comment by @Hnasar on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:56 on 2024-05-21 13:58_

Also for mono-repos, there may be dozens or hundreds of _first party_ top-level packages. Therefore this rule should base its decisions on whether an import is intra-package (e.g. a relative import) rather than first vs third party packages.

---

_@zanieb reviewed on 2024-05-21 15:01_

---

_Review comment by @zanieb on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:56 on 2024-05-21 15:01_

Thanks for your feedback @Hnasar — would you mind opening a new issue to discuss this preview behavior? I don't think this pull request is a great place for it since it's just updating the docs.

---

_Merged by @plredmond on 2024-05-21 16:23_

---

_Closed by @plredmond on 2024-05-21 16:23_

---

_Branch deleted on 2024-05-21 16:23_

---
