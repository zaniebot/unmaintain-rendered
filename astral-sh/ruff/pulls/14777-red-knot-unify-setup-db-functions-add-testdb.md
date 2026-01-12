```yaml
number: 14777
title: "[red-knot] Unify `setup_db()` functions, add `TestDb` builder"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/test-db-builder
created_at: 2024-12-04T20:05:40Z
updated_at: 2024-12-04T20:36:56Z
url: https://github.com/astral-sh/ruff/pull/14777
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Unify `setup_db()` functions, add `TestDb` builder

---

_@sharkdp_

## Summary

- Instead of seven (more or less similar) `setup_db` functions, use just one in a single central place.
- For every test that needs customization beyond that, offer a `TestDbBuilder` that can control the Python target version, custom typeshed, and pre-existing files.

The main motivation for this is that we're soon going to need customization of the Python version, and I didn't feel like adding this to each of the existing `setup_db` functions.

It feels like there is much more we can do, especially around writing files, but let's first see if everyone is happy with this first step.

---

_Label `red-knot` added by @sharkdp on 2024-12-04 20:05_

---

_Review requested from @carljm by @sharkdp on 2024-12-04 20:05_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-04 20:05_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-04 20:05_

---

_@sharkdp reviewed on 2024-12-04 20:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3822 on 2024-12-04 20:07_

This is the only functional change, without any consequences. But it feels okay, since `typing.NoDefault` is only available since 3.13). I will need this — and similar things — soon in the statically-know branch, so I'm putting it into place already.

---

_Comment by @github-actions[bot] on 2024-12-04 20:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3822 on 2024-12-04 20:13_

I think once we have the statically-known branch, we will probably want to add more tests around some of these "exists in `typing` in recent versions and in `typing_extensions` as fallback" known symbols, in terms of how they behave on older and newer Pythons. But this change seems fine for this PR and this particular test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3819 on 2024-12-04 20:15_

@AlexWaygood previously expressed a preference for using `unwrap()` in tests, rather than having tests return a `Result` and then using `?` in the test. We have some existing tests using both styles. I don't have strong feelings, but I think I shared Alex's preference after writing tests in both styles; the need to end the test with `Ok(())` is irritating.

Do you have a preference in the other direction?

---

_@carljm approved on 2024-12-04 20:17_

Thank you! Much nicer. This always bugged me slightly but never enough to fix it :)

---

_@sharkdp reviewed on 2024-12-04 20:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3819 on 2024-12-04 20:23_

> Do you have a preference in the other direction?

I do not. I usually also use `.unwrap()` liberally in tests. Sometimes the code looks a lot nicer if there are *many* `.unwrap()`s that can be replaced with `?`, so I guess it's okay to not have a strict guideline(?).

For `TestDbBuilder` specifically, I first wrote a version that didn't return a `Result`. I then changed it because I thought there might be more complex test setups in the future where it would be useful for the developer to see a good error message if they did something wrong in the setup of the test.

And note that `setup_db()` still returns a plain `TestDb`, not a `Result`. It's just the builder that returns a `Result`.

For the particular example here... nothing can go wrong when just setting the Python version, so I'll change this back to `.unwrap()` style.

---

_Merged by @sharkdp on 2024-12-04 20:36_

---

_Closed by @sharkdp on 2024-12-04 20:36_

---

_Branch deleted on 2024-12-04 20:36_

---
