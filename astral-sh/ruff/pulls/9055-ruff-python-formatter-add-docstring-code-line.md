```yaml
number: 9055
title: "ruff_python_formatter: add docstring-code-line-width internal setting"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - docstring
  - formatter
assignees: []
merged: true
base: main
head: ag/fmt/line-length
created_at: 2023-12-08T18:50:59Z
updated_at: 2023-12-11T13:21:00Z
url: https://github.com/astral-sh/ruff/pull/9055
synced_at: 2026-01-10T23:40:55Z
```

# ruff_python_formatter: add docstring-code-line-width internal setting

---

_Pull request opened by @BurntSushi on 2023-12-08 18:50_

## Summary

This does the light plumbing necessary to add a new internal option that permits setting the line width of code examples in docstrings. The plan is to add the corresponding user facing knob in #8854.

Note that this effectively removes the `same-as-global` configuration style discussed [in this comment](https://github.com/astral-sh/ruff/issues/8855#issuecomment-1847230440). It replaces it with the `{integer}` configuration style only.

There are a lot of commits here, but they are each tiny to make review easier because of the changes to snapshots.

## Test Plan

I added a new docstring test configuration that sets `docstring-code-line-width = 60` and examined the differences.

---

_Label `internal` added by @BurntSushi on 2023-12-08 18:50_

---

_Label `docstring` added by @BurntSushi on 2023-12-08 18:50_

---

_Label `formatter` added by @BurntSushi on 2023-12-08 18:50_

---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-08 18:50_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-08 18:50_

---

_Comment by @github-actions[bot] on 2023-12-08 19:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/options.rs`:56 on 2023-12-08 22:29_

I'd expect this to default to the formatter's line-length, but I _think_ that happens at a level above, when we create the `Settings`.

---

_@charliermarsh reviewed on 2023-12-08 22:29_

---

_@charliermarsh approved on 2023-12-08 22:29_

---

_@MichaReiser reviewed on 2023-12-10 23:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/options.rs`:56 on 2023-12-10 23:38_

Good point. I don't know if serde supports defaults that depend on another field's value.

---

_@MichaReiser approved on 2023-12-10 23:41_

---

_@BurntSushi reviewed on 2023-12-11 13:20_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/options.rs`:56 on 2023-12-11 13:20_

I specifically didn't attempt this, because this would imply a `same-as-global` setting which is distinct from a specific fixed integer. I think that if we do want that mode, then we ought to make it an explicit setting so that users can specifically opt into it in other contexts. So for example, instead of using `LineWidth` here, I'd do:

```rust
enum DocstringCodeLineWidth {
    SameAsGlobal,
    Fixed(LineWidth),
}
```

And then hopefully we'll add `Dynamic` (pending my experimentation today).

But we can also hash this out a bit more when I add the user-facing version of this.

---

_Merged by @BurntSushi on 2023-12-11 13:20_

---

_Closed by @BurntSushi on 2023-12-11 13:20_

---

_Branch deleted on 2023-12-11 13:21_

---
