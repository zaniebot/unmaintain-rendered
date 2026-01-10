```yaml
number: 18295
title: "[ty] Tell the user why we inferred a certain Python version when reporting version-specific syntax errors"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/syntax-error-hint
created_at: 2025-05-25T10:44:00Z
updated_at: 2025-05-26T20:44:44Z
url: https://github.com/astral-sh/ruff/pull/18295
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Tell the user why we inferred a certain Python version when reporting version-specific syntax errors

---

_Pull request opened by @AlexWaygood on 2025-05-25 10:44_

## Summary

This PR improves our reporting of version-specific syntax errors in ty so that we inform the user why we inferred a certain Python version when parsing Python syntax in their project.

<details>
<summary>Screenshot demo</summary>

![image](https://github.com/user-attachments/assets/09be22ac-907d-4d6c-a4a4-16a4048a181b)

</details>

~~## Implementation~~

~~I had to add a couple of `AsMut` implementations so that the function adding the subdiagnostic would be able to accept both a `mut LintDiagnosticGuard` (if called from within the `ty_python_semantic` crate) or a `mut Diagnostic` (if called from within the `ty` crate). The existing `DerefMut<Target=Diagnostic>` implementation that `LintDiagnosticGuard` has is insufficient, because `Diagnostic` does not (and cannot) implement `DerefMut<Target=Diagnostic>`. It felt odd adding `AsMut` implementations without also adding `AsRef` implementations, so I also added these for consistency, although they are not strictly needed.~~

## Test Plan

I added a snapshot test to the `ty` crate


---

_Review requested from @BurntSushi by @AlexWaygood on 2025-05-25 10:44_

---

_Review requested from @ntBre by @AlexWaygood on 2025-05-25 10:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-25 10:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-25 10:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-25 10:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-25 10:44_

---

_Label `ty` added by @AlexWaygood on 2025-05-25 10:44_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-25 10:44_

---

_Comment by @github-actions[bot] on 2025-05-25 10:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @ntBre on `crates/ty_python_semantic/src/types/diagnostic.rs`:1771 on 2025-05-25 13:32_

I _think_ you can get by without the new `AsRef` and `AsMut` stuff if you pass a `&mut Diagnostic` here, unless there's a reason I'm missing that it needs to be passed by value.

Auto-deref on the guard seems to be working locally for me with that change.

---

_@ntBre approved on 2025-05-25 13:34_

Nice, this makes sense to me!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1771 on 2025-05-25 13:53_

Ahh, thank you! That's indeed a fair bit simpler!

---

_@AlexWaygood reviewed on 2025-05-25 13:53_

---

_@AlexWaygood reviewed on 2025-05-25 13:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-05-25 13:54_

this function could plausibly be moved to the `ruff_db` crate now, since it's no longer specific to `ty_python_semantic` diagnostics. I'm happy to make that change, but it's a slightly bigger refactor -- the `PythonVersionSource` enum would also have to be moved to that crate

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-05-26 06:50_

I'd prefer to keep it in `ty_python_semantic`. While it's possible dependency-wise, it still more closely fits into a Python-specific crate. 

I do agree that it's no longer typing specific, which could be a reason to move it out of the `types` module.

```suggestion
/// Add a subdiagnostic to `diagnostic` that explains why a certain Python version was inferred.
///
/// ty can infer the Python version from various sources, such as command-line arguments,
/// configuration files, or defaults.
pub(crate) fn add_inferred_python_version_hint_to_diagnostic(
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:1772 on 2025-05-26 06:51_

To avoid monomorphization (this isn't perf sensitive and it involves a fair amount of code)

```suggestion
    action: &dyn std::fmt::Display,
```

---

_Review comment by @MichaReiser on `crates/ty/tests/cli.rs`:312 on 2025-05-26 06:54_

It would be nice if these tests could be mdtests instead. But I think our mdtest markdown parsing doesn't preserve the location

---

_@MichaReiser approved on 2025-05-26 06:55_

Nice

---

_@AlexWaygood reviewed on 2025-05-26 20:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1772 on 2025-05-26 20:03_

I'll just change this to `&str` for now; we can switch it to a more general type if we ever actually have a need for it!

---

_@AlexWaygood reviewed on 2025-05-26 20:06_

---

_Review comment by @AlexWaygood on `crates/ty/tests/cli.rs`:312 on 2025-05-26 20:06_

yeah, I don't think it's worth the added tomplexity to the mdtest framework to make it possible to write these as mdtests. (Not yet, anyway -- I guess that might change if we add lots more tests like this...)

It's also actually kinda nice writing these as integration tests because the snapshots are inline 😄

---

_@AlexWaygood reviewed on 2025-05-26 20:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-05-26 20:09_

Making it `pub(crate)` doesn't compile, because this PR adds a usage of this function from the `ty_project` crate. That's part of the reason why I thought it might be logical to move the function to `ruff_db` -- it's a utility function that can be used to enhance multiple kinds of diagnostics, not all of which are emitted or even processed by the `ty_python_semantic` crate.

---

_@MichaReiser reviewed on 2025-05-26 20:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-05-26 20:13_

I still think it's a good fit given that ProgramSettings are defined in the semantic crate

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-05-26 20:16_

Makes sense. I like your idea of moving it out of the `types` module. Maybe a new `diagnostic_utils` module in `ty_python_semantic`?

---

_@AlexWaygood reviewed on 2025-05-26 20:16_

---

_@MichaReiser reviewed on 2025-05-26 20:34_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-05-26 20:34_

A small part of me always dies with every a utils module 🙈

I would probably leave it for now, given that it is a single function. A module feels overkill. Unless there's some place where we have other python version code 

---

_@AlexWaygood reviewed on 2025-05-26 20:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-05-26 20:37_

I realised we already had a `utils` module, so I moved it there ;)

---

_Merged by @AlexWaygood on 2025-05-26 20:44_

---

_Closed by @AlexWaygood on 2025-05-26 20:44_

---

_Branch deleted on 2025-05-26 20:44_

---
