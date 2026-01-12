```yaml
number: 15935
title: "Make `Binding::range()` point to the range of a type parameter's name, not the full type parameter"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - linter
assignees: []
merged: true
base: main
head: alex/tvar-binding-ranges
created_at: 2025-02-04T14:02:35Z
updated_at: 2025-02-04T14:14:23Z
url: https://github.com/astral-sh/ruff/pull/15935
synced_at: 2026-01-12T15:55:53Z
```

# Make `Binding::range()` point to the range of a type parameter's name, not the full type parameter

---

_@AlexWaygood_

## Summary

A PEP-695 type parameter creates a binding that is tracked by Ruff's semantic model:

```py
def f[S](): ...
    # ^ `S` symbol bound here
```

But if the type parameter has a bound or constraint, the range of the `Binding` recorded by Ruff's semantic model includes the bounds or constraints of the type parameter:

```py
def f[T: int](): ...
    # ^^^^^^ Semantic model's `Binding` for `T` has this range

def g[U: (int, str)](): ...
    # ^^^^^^^^^^^^^ Semantic models' binding for `U` has this range
```

This is problematic, as `S: int` is not a valid Python identifier, and nor is `T: (int, str)`. All other bindings in Ruff's semantic model have their range point to the _name_ that is bound, not the full expression/statement that creates the binding. A specific effect that this has is that if you have a `Binding` `x` that points to a PEP-695 type parameter, calling `x.name(source)` does not actually return the name of the PEP-695 type parameter. Instead, if `x` pointed to the binding for `T` in the example above, calling `x.name(source)` would return `"T: int"`, and if `x` pointed to the binding for `U`, calling `x.name(source)` would return `"U: (int, str)"`.

This PR changes the range for PEP-695 bindings so that it points to the name that is bound by the type parameter. This makes these `Binding`s consistent with other bindings, makes `Binding::name()` work as expected in all situations, allows us to simplify some code in the implementation of the PYI019 rule, and unblocks #15862

## Test Plan

`cargo test -p ruff_linter --lib`

Co-authored-by: Brent Westbrook <36778786+ntBre@users.noreply.github.com>

---

_Label `internal` added by @AlexWaygood on 2025-02-04 14:02_

---

_Label `linter` added by @AlexWaygood on 2025-02-04 14:02_

---

_@MichaReiser approved on 2025-02-04 14:06_

Haha nice find. 

Do you expect that this breaks any diagnostic range? E.g. do you know if the binding range is used in any diagnostic / fix code? I know, this is probably hard to figure out, just something that I'm slightly worried about but relying on our tests/user reports might be fine to catch this.

---

_Comment by @github-actions[bot] on 2025-02-04 14:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-02-04 14:13_

> Do you expect that this breaks any diagnostic range?

It's very hard to tell... :(

> E.g. do you know if the binding range is used in any diagnostic / fix code? I know, this is probably hard to figure out, just something that I'm slightly worried about but relying on our tests/user reports might be fine to catch this.

Agreed, and this is partly why I wanted to separate this out as a separate PR from #15862 (see my comment in https://github.com/astral-sh/ruff/pull/15862#discussion_r1940982081). This is also why I worked around the bug in https://github.com/astral-sh/ruff/pull/15888 rather than fixing it prior to making that PR... but then @ntBre ran into exactly the same bug the next day in #15862! And I don't think it'll be so easy to workaround the bug in his PR. Plus, `Binding::name()` not working consistently for all bindings is a massive footgun. So I think it's time to fix the bug.

The fact that none of our snapshots change and the ecosystem check is clean gives me confidence that, if this _does_ change our user-facing behaviour in some cases, they're at least rare/edge cases. And PEP-695 type parameters are anyway new syntax that are still rarely used in the real world. So I _think_ we're good here.

---

_Merged by @AlexWaygood on 2025-02-04 14:14_

---

_Closed by @AlexWaygood on 2025-02-04 14:14_

---

_Branch deleted on 2025-02-04 14:14_

---
