```yaml
number: 13987
title: "Use binary semantics when `__iadd__` et al are unbound"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/unbound
created_at: 2024-10-30T02:39:22Z
updated_at: 2024-10-30T13:18:43Z
url: https://github.com/astral-sh/ruff/pull/13987
synced_at: 2026-01-12T15:55:46Z
```

# Use binary semantics when `__iadd__` et al are unbound

---

_@charliermarsh_

## Summary

I noticed that augmented assignments on floats were yielding "not supported" diagnostics. If the dunder isn't bound at all, we should use binary operator semantics, rather than treating it as not-callable.


---

_Comment by @codspeed-hq[bot] on 2024-10-30 02:44_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/unbound)

### Merging #13987 will **not alter performance**

<sub>Comparing <code>charlie/unbound</code> (5cb09d1) with <code>main</code> (e6dcdf3)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Marked ready for review by @charliermarsh on 2024-10-30 02:57_

---

_Review requested from @carljm by @charliermarsh on 2024-10-30 02:57_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-30 02:57_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-30 02:57_

---

_Comment by @github-actions[bot] on 2024-10-30 03:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1447 on 2024-10-30 08:28_

```suggestion
            if !class_member.is_unbound() {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1448 on 2024-10-30 08:28_

```suggestion
                let call = class_member.call(self.db, &[target_type, value_type]);
```

(https://github.com/astral-sh/ruff/pull/13981#discussion_r1822075752)

---

_@AlexWaygood approved on 2024-10-30 08:33_

We also need to consider situations where `__iadd__` _might_ be bound, but also _might not_ be, such as

```py
def bool_instance() -> bool:
    return True

flag = bool_instance()

class Foo:
    def __add__(self, other: Foo) -> Foo:
        return Foo()

    if bool_instance():
        def __iadd__(self, other: Foo) -> int:
            return 42

f = Foo()
f += Foo()

# This should be `Foo | int`,
# the union of the "type where `__iadd__` is bound"
# and the "type where we fallback to `__add__`"
reveal_type(f)
```

For examples of how we handle this kind of thing elsewhere in red-knot, you can grep for uses of the `.may_be_unbound` method and the `replace_unbound_with` method in `infer.rs`.

However, I'm okay with deferring handling this to a followup!

---

_Renamed from "Use binary semantics when `__iadd_` et al are unbound" to "Use binary semantics when `__iadd__` et al are unbound" by @AlexWaygood on 2024-10-30 11:54_

---

_Label `red-knot` added by @charliermarsh on 2024-10-30 13:05_

---

_Merged by @charliermarsh on 2024-10-30 13:09_

---

_Closed by @charliermarsh on 2024-10-30 13:09_

---

_Branch deleted on 2024-10-30 13:09_

---
