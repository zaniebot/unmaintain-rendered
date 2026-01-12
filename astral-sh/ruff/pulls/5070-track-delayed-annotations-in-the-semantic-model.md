```yaml
number: 5070
title: "Track \"delayed\" annotations in the semantic model"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/annotations
created_at: 2023-06-14T01:14:15Z
updated_at: 2023-06-14T18:14:54Z
url: https://github.com/astral-sh/ruff/pull/5070
synced_at: 2026-01-12T03:43:30Z
```

# Track "delayed" annotations in the semantic model

---

_Pull request opened by @charliermarsh on 2023-06-14 01:14_

## Summary

This PR tackles a corner case that we'll need to support local symbol renaming. It relates to a nuance in how we want handle annotations (i.e., `AnnAssign` statements with no value, like `x: int` in a function body).

When we see a statement like:

```python
x: int
```

We create a `BindingKind::Annotation` for `x`. This is a special `BindingKind` that the resolver isn't allowed to return. For example, given:

```python
x: int
print(x)
```

The second line will yield an `undefined-name` error.

So why does this `BindingKind` exist at all? In Pyflakes, to support the `unused-annotation` lint:

```python
def f():
    x: int  # unused-annotation
```

If we don't track `BindingKind::Annotation`, we can't lint for unused variables that are only "defined" via annotations.

There are a few other wrinkles to `BindingKind::Annotation`. One is that, if a binding already exists in the scope, we actually just discard the `BindingKind`. So in this case:

```python
x = 1
x: int
```

When we go to create the `BindingKind::Annotation` for the second statement, we notice that (1) we're creating an annotation but (2) the scope already has binding for the name -- so we just drop the binding on the floor. This has the nice property that annotations aren't considered to "shadow" another binding, which is important in a bunch of places (e.g., if we have `import os; os: int`, we still consider `os` to be an import, as we should). But it also means that these "delayed" annotations are one of the few remaining references that we don't track anywhere in the semantic model.

This PR adds explicit support for these via a new `delayed_annotations` attribute on the semantic model. These should be extremely rare, but we do need to track them if we want to support local symbol renaming.

### This isn't the right way to model this

This isn't the right way to model this.

Here's an alternative:

- Remove `BindingKind::Annotation`, and treat annotations as their own, separate concept.
- Instead of storing a map from name to `BindingId` on each `Scope`, store a map from name to... `SymbolId`.
- Introduce a `Symbol` abstraction, where a symbol can point to a current binding, and a list of annotations, like:

```rust
pub struct Symbol {
  binding: Option<BindingId>,
  annotations: Vec<AnnotationId>
}
```

If we did this, we could appropriately model the semantics described above. When we go to resolve a binding, we ignore annotations (always). When we try to find unused variables, we look through the list of symbols, and have sufficient information to discriminate between annotations and bound variables. Etc.

The main downside of this `Symbol`-based approach is that it's going to take a lot more work to implement, and it'll be less performant (we'll be storing more data per symbol, and our binding lookups will have an added layer of indirection).


---

_Comment by @github-actions[bot] on 2023-06-14 01:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.8±0.03ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1383.9±4.22µs    12.0 MB/sec    1.02   1405.5±1.51µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    136.4±0.40µs    21.6 MB/sec    1.02    138.5±8.13µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.02ms     9.3 MB/sec    1.01      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.01     14.5±0.03ms     2.8 MB/sec    1.00     14.4±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.7±1.47µs     8.0 MB/sec    1.00    369.7±1.47µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.05ms     4.1 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.01ms     5.7 MB/sec    1.00      7.1±0.03ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1528.6±6.45µs    10.9 MB/sec    1.00   1511.5±3.67µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.3±0.47µs    18.1 MB/sec    1.00    162.5±1.43µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.00ms     7.7 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.05ms     5.2 MB/sec    1.01      7.9±0.04ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1592.4±18.14µs    10.5 MB/sec    1.01  1605.4±29.36µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    155.6±2.02µs    19.0 MB/sec    1.00    156.2±2.84µs    18.9 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     7.9 MB/sec    1.00      3.2±0.03ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.16ms     2.4 MB/sec    1.01     16.9±0.16ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.04ms     3.9 MB/sec    1.01      4.3±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    441.3±9.61µs     6.7 MB/sec    1.00    436.8±5.47µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.12ms     3.5 MB/sec    1.00      7.3±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.08ms     4.8 MB/sec    1.00      8.5±0.16ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1775.0±22.64µs     9.4 MB/sec    1.00  1775.2±23.26µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.7±1.73µs    15.6 MB/sec    1.00    190.4±5.23µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.6 MB/sec    1.00      3.9±0.04ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-14 17:54_

---

_Closed by @charliermarsh on 2023-06-14 17:54_

---

_Branch deleted on 2023-06-14 17:54_

---
