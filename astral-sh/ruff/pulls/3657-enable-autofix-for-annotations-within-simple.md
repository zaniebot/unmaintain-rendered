```yaml
number: 3657
title: "Enable autofix for annotations within 'simple' string literals"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/quoted-annotations
created_at: 2023-03-21T20:40:32Z
updated_at: 2023-03-22T16:58:57Z
url: https://github.com/astral-sh/ruff/pull/3657
synced_at: 2026-01-12T15:55:13Z
```

# Enable autofix for annotations within 'simple' string literals

---

_@charliermarsh_

## Summary,

Today, we avoid applying any autofixes for code within deferred string type annotations. For example:

```py
# Ruff will flag this, and convert it to `list[int]`.
x: List[int] = []

# Ruff will flag this, but won't attempt to fix it.
x: "List[int]" = []
```

This PR enables autofix for code within deferred string type annotations _in certain cases_. Namely, we allow these snippets to be fixed if and only if the annotation is "simple", defined here as not containing any implicit concatenations or escaped characters. For example, we don't even try to fix this:

```py
x: "Li" "st[int]" = []
```

...which is valid, but unusual.

Another way to think of this definition is: can we accurately predict the source code location for expressions within the string? If a string is "simple", then we can do a "located parse" starting at the beginning of the content, and all the parsed locations will be correct within the indexing scheme of the parent.

Note that if an annotation is "complex", we fallback to our existing behavior, whereby we modify the location of every located expression within the parsed annotation with the location of the _string itself_. So, e.g., the `int` in `"Li" "st[int]"` is given the location of the entire string annotation.

Closes #3656.

Closes #3508.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-21 20:40_

---

_Review requested from @konstin by @charliermarsh on 2023-03-21 20:40_

---

_Comment by @github-actions[bot] on 2023-03-21 21:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.02ms     2.9 MB/sec    1.01     14.2±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.4±1.91µs     6.8 MB/sec    1.00    435.4±1.55µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.00      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.01ms     5.2 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1711.8±3.84µs     9.7 MB/sec    1.00   1685.3±2.00µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.1±0.70µs    17.0 MB/sec    1.01    175.5±2.11µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.01      3.7±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.1±0.10ms     2.7 MB/sec    1.00     14.9±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.01ms     4.0 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    453.3±3.41µs     6.5 MB/sec    1.00    450.0±4.85µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.02ms     3.8 MB/sec    1.00      6.7±0.04ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.02ms     4.9 MB/sec    1.00      8.1±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1765.7±7.78µs     9.4 MB/sec    1.00   1736.8±5.83µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.4±0.99µs    16.3 MB/sec    1.00    180.7±2.60µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4549 on 2023-03-22 08:05_

Nit: We can try to use `drain` to avoid the need for reversing and manually call pop

```rust
let mut string_type_definitions = self.deferred.string_type_definitions.drain(..).rev();
for ((range, value, (in_annotation, in_type_checking_block), deferral)) = string_type_definitions {
	...
}
```

If the borrow checker gets mad because of multiple mutable borrows of `self`, then try this:

```rust
let mut string_type_definitions = std::mem::take(&mut self.deferred.string_type_definitions);
for ((range, ..)) = string_type_definitions.drain(..).rev() {
  ..
}

// Restore vector, to re-use the heap allocations
self.deferred.string_type_definitions = string_type_definitions; 
```

Both these approaches assume that the loop's body does not read from or write to `self.deferred.string_type_definitions.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/scope.rs`:2 on 2023-03-22 08:06_

Revert?

---

_@MichaReiser approved on 2023-03-22 08:07_

---

_Review comment by @konstin on `crates/ruff_python_ast/src/typing.rs`:76 on 2023-03-22 09:53_

isn't that a case for `derive(PartialEq, Eq)]`?

---

_@konstin reviewed on 2023-03-22 09:53_

---

_@konstin approved on 2023-03-22 09:53_

---

_@charliermarsh reviewed on 2023-03-22 16:09_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4549 on 2023-03-22 16:09_

I think we actually _do_ want to write to `self.deferred.string_type_definitions`, but we probably _don't_ do it currently (https://github.com/charliermarsh/ruff/issues/3655).

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/typing.rs`:76 on 2023-03-22 16:30_

This macro gives us something slightly different -- we can do `annotation.is_simple()`, similar to what we get with `Result`.

---

_@charliermarsh reviewed on 2023-03-22 16:30_

---

_Merged by @charliermarsh on 2023-03-22 16:45_

---

_Closed by @charliermarsh on 2023-03-22 16:45_

---

_Branch deleted on 2023-03-22 16:45_

---
