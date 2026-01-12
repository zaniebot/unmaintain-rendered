```yaml
number: 13806
title: "[red-knot] mdtest suite: formatting and cleanup"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-clean-up
created_at: 2024-10-18T07:42:10Z
updated_at: 2024-10-18T10:05:06Z
url: https://github.com/astral-sh/ruff/pull/13806
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] mdtest suite: formatting and cleanup

---

_@sharkdp_

## Summary

Minor cleanup and consistent formatting of the Markdown-based tests.

- Removed lots of unnecessary `a`, `b`, `c`, … variables.
- Moved test assertions (`# revealed:` comments) closer to the tested object.
- Always separate `# revealed` and `# error` comments from the code by two spaces, according to the discussion [here](https://github.com/astral-sh/ruff/pull/13746/files#r1799385758). This trades readability for consistency in some cases.
- Fixed some headings


---

_Label `red-knot` added by @sharkdp on 2024-10-18 07:42_

---

_Review requested from @carljm by @sharkdp on 2024-10-18 07:42_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-18 07:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-18 07:42_

---

_@sharkdp reviewed on 2024-10-18 07:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/not_none.md`:9 on 2024-10-18 07:49_

We have the exact same test in `narrow/conditionals_is_not.md`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:14 on 2024-10-18 08:37_

```suggestion
reveal_type(a)  # revealed: float
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:9 on 2024-10-18 08:39_

Is it relevant for the tests that we assign to the variables or could we just do 
```suggestion
reveal_type(3 - 4)
```

the same as you do in another test further down

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/literal/collections/tuple.md`:16 on 2024-10-18 08:41_

```suggestion
reveal_type( (1, (2, 3)))  # revealed: tuple[Literal[1], tuple[Literal[2], Literal[3]]]
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/literal/f_string.md`:14 on 2024-10-18 08:42_

Nit: Can we inline the expressions?

```suggestion
reveal_type(f'h {x})  # revealed: Literal["h 0"]
```



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/subscript/tuple.md`:17 on 2024-10-18 08:43_

```suggestion
a = t[4]  # error: [index-out-of-bounds]
reveal_type(a)  # revealed: Unknown

b = t[-4]  # error: [index-out-of-bounds]
reveal_type(b)  # revealed: Unknown
```

---

_@MichaReiser approved on 2024-10-18 08:43_

Nice! Thank you

---

_Merged by @sharkdp on 2024-10-18 09:07_

---

_Closed by @sharkdp on 2024-10-18 09:07_

---

_Branch deleted on 2024-10-18 09:07_

---

_Comment by @AlexWaygood on 2024-10-18 10:05_

Big improvement, thank you!!

---
