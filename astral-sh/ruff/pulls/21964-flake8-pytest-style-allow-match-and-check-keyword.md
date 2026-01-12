```yaml
number: 21964
title: "[`flake8-pytest-style`] Allow `match` and `check` keyword arguments without an expected exception type (`PT010`)"
type: pull_request
state: merged
author: mahiro72
labels:
  - rule
assignees: []
merged: true
base: main
head: pt010-allow-match-check-kwargs
created_at: 2025-12-13T18:49:49Z
updated_at: 2025-12-18T15:42:06Z
url: https://github.com/astral-sh/ruff/pull/21964
synced_at: 2026-01-12T15:57:37Z
```

# [`flake8-pytest-style`] Allow `match` and `check` keyword arguments without an expected exception type (`PT010`)

---

_@mahiro72_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Updates PT010(`pytest-raises-without-exception`) to recognize `match` and `check` keyword arguments as valid alternatives to specifying an exception class.

As of pytest 8.4.0, `pytest.raises()` can be called with only `match` or `check` keyword arguments without an expected exception.

Fixes #18653

## Test Plan

<!-- How was it tested? -->

- Added test cases for `match`-only, `check`-only, and both arguments.
- `cargo test -p ruff_linter -- "pytestraiseswithoutexception"` passes


---

_Comment by @astral-sh-bot[bot] on 2025-12-13 18:59_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @mahiro72 on 2025-12-13 19:00_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:129 on 2025-12-17 17:22_

Let's try to unify this with the rest of the docs rather than a separate note. I think something like:

```
/// The rule will also accept calls without an expected exception but with
/// `match` and/or `check` keyword arguments, which are also valid after
/// pytest version 8.4.0.
```

appended to the preceding paragraph would flow well enough.

It might also be nice to add an example to the `Use instead` section of one of these. But maybe it's okay as-is. Three examples feels like too much to me for sure, so maybe it's better to stay with one.

---

_@ntBre reviewed on 2025-12-17 17:26_

Looks great, thank you! I just had one nit about the docs.

I also thought a bit about making this a preview change in case people had previously had noqa comments like:

```py
with pytest.raises(match="some pattern"):  # noqa: PT010
```

but I didn't find any examples of that in a GitHub code [search](https://github.com/search?q=language%3Apython+path%3Atest+%2Fpytest.raises%5C%28%28match%7Ccheck%29%3D.*noqa%2F&type=code), and the ecosystem check is obviously clean, so I think it's okay to land in stable.

---

_Label `rule` added by @ntBre on 2025-12-17 17:26_

---

_Renamed from "[PT010] Allow `match` and `check` keyword arguments without exception class (pytest 8.4.0+)" to "[`flake8-pytest-style`] Allow `match` and `check` keyword arguments without an expected exception type (`PT010`)" by @ntBre on 2025-12-17 17:26_

---

_@mahiro72 reviewed on 2025-12-18 09:56_

---

_Review comment by @mahiro72 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:129 on 2025-12-18 09:56_

Thanks for the review! I've updated the docs to integrate the note into the preceding paragraph as you suggested.

Regarding the example in the `Use instead` section - I agree that three examples would be too many, so I'll leave it as-is for now.

[docs: unify note format in PT010 documentation](https://github.com/astral-sh/ruff/pull/21964/commits/b28c54dab671b8c75c52bdd5e03ea35d1b0a9bb5)

---

_@ntBre approved on 2025-12-18 15:41_

Thank you!

---

_Merged by @ntBre on 2025-12-18 15:42_

---

_Closed by @ntBre on 2025-12-18 15:42_

---
