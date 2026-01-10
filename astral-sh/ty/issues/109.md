```yaml
number: 109
title: Check implementation consistency for an overloaded function
type: issue
state: open
author: dhruvmanila
labels:
  - help wanted
  - overloads
assignees: []
created_at: 2025-04-30T18:56:06Z
updated_at: 2025-06-11T01:01:14Z
url: https://github.com/astral-sh/ty/issues/109
synced_at: 2026-01-10T02:08:20Z
```

# Check implementation consistency for an overloaded function

---

_Issue opened by @dhruvmanila on 2025-04-30 18:56_

The task here is to check whether the overloaded signatures is consistent with the implementation signature if it's present. Specifically, from the spec:

> If an overload implementation is defined, type checkers should validate that it is consistent with all of its associated overload signatures. The implementation should accept all potential sets of arguments that are accepted by the overloads and should produce all potential return types produced by the overloads. In typing terms, this means the input signature of the implementation should be [assignable](https://typing.python.org/en/latest/spec/glossary.html#term-assignable) to the input signatures of all overloads, and the return type of all overloads should be assignable to the return type of the implementation.
> 
> If the implementation is inconsistent with its overloads, a type checker should report an error.
>
> https://typing.python.org/en/latest/spec/overload.html#implementation-consistency

The following function is the one where all overload checks happen:

https://github.com/astral-sh/ruff/blob/bd5fb826bce149d8a834767e6c175c2e4d90105a/crates/red_knot_python_semantic/src/types/infer.rs#L990-L990

Specifically, it loops over each of the overloaded function that needs to be checked here:

https://github.com/astral-sh/ruff/blob/bd5fb826bce149d8a834767e6c175c2e4d90105a/crates/red_knot_python_semantic/src/types/infer.rs#L1036-L1039

which are used to perform various checks.

**Notes:**
* We can reuse the [`INVALID_OVERLOAD`](https://github.com/astral-sh/ruff/blob/bd5fb826bce149d8a834767e6c175c2e4d90105a/crates/red_knot_python_semantic/src/types/diagnostic.rs#L487-L487) lint rule to raise a diagnostic for this check


---
