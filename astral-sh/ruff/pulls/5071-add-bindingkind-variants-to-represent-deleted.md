```yaml
number: 5071
title: "Add `BindingKind` variants to represent deleted bindings"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/deletions
created_at: 2023-06-14T02:49:00Z
updated_at: 2023-06-14T13:27:25Z
url: https://github.com/astral-sh/ruff/pull/5071
synced_at: 2026-01-12T15:55:17Z
```

# Add `BindingKind` variants to represent deleted bindings

---

_@charliermarsh_

## Summary

Our current mechanism for handling deletions (e.g., `del x`) is to remove the symbol from the scope's `bindings` table. This "does the right thing", in that if we then reference a deleted symbol, we're able to determine that it's unbound -- but it causes a variety of problems, mostly in that it makes certain bindings and references unreachable after-the-fact.

Consider:

```python
x = 1
print(x)
del x
```

If we analyze this code _after_ running the semantic model over the AST, we'll have no way of knowing that `x` was ever introduced in the scope, much less that it was bound to a value, read, and then deleted -- because we effectively erased `x` from the model entirely when we hit the deletion.

In practice, this will make it impossible for us to support local symbol renames. It also means that certain rules that we want to move out of the model-building phase and into the "check dead scopes" phase wouldn't work today, since we'll have lost important information about the source code.

This PR introduces two new `BindingKind` variants to model deletions:

- `BindingKind::Deletion`, which represents `x = 1; del x`.
- `BindingKind::UnboundException`, which represents:

```python
try:
  1 / 0
except Exception as e:
  pass
```

In the latter case, `e` gets unbound after the exception handler (assuming it's triggered), so we want to handle it similarly to a deletion.

The main challenge here is auditing all of our existing `Binding` and `Scope` usages to understand whether they need to accommodate deletions or otherwise behave differently. If you look one commit back on this branch, you'll see that the code is littered with `NOTE(charlie)` comments that describe the reasoning behind changing (or not) each of those call sites. I've also augmented our test suite in preparation for this change over a few prior PRs.

### Alternatives

As an alternative, I considered introducing a flag to `BindingFlags`, like `BindingFlags::UNBOUND`, and setting that at the appropriate time.

This turned out to be a much more difficult change, because we tend to match on `BindingKind` all over the place (e.g., we have a bunch of code blocks that only run when a `BindingKind` is `BindingKind::Importation`). As a result, introducing these new `BindingKind` variants requires only a few changes at the client sites. Adding a flag would've required a much wider-reaching change.


---

_Review requested from @konstin by @charliermarsh on 2023-06-14 02:49_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:196 on 2023-06-14 02:49_

I decided to rename this. "Unbound" means something different. This method is used when autofixing, to check whether we can introduce a new symbol into a file -- so, e.g., if we're going to import `foo`, we want to make sure that `foo` wasn't defined in any scope, where it might conflict.

---

_@charliermarsh reviewed on 2023-06-14 02:49_

---

_@charliermarsh reviewed on 2023-06-14 02:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:42 on 2023-06-14 02:50_

A nice side-effect: this is no longer necessary. Now that we add to the scope when we hit a deletion, we don't have to track deleted symbols separately -- since every deleted symbol will appear in `bindings`.

---

_Comment by @github-actions[bot] on 2023-06-14 03:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.6±0.40ms     6.1 MB/sec    1.02      6.7±0.33ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1366.1±53.93µs    12.2 MB/sec    1.08  1476.6±155.60µs    11.3 MB/sec
formatter/numpy/globals.py                 1.00    132.1±6.96µs    22.3 MB/sec    1.01    133.4±7.91µs    22.1 MB/sec
formatter/pydantic/types.py                1.03      2.7±0.14ms     9.5 MB/sec    1.00      2.6±0.12ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.5±0.43ms     2.8 MB/sec    1.03     14.9±0.79ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.5±0.18ms     4.8 MB/sec    1.00      3.4±0.12ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   445.5±17.29µs     6.6 MB/sec    1.01   451.1±24.99µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.25ms     4.2 MB/sec    1.00      6.1±0.30ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.24ms     5.9 MB/sec    1.03      7.1±0.37ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1482.1±46.67µs    11.2 MB/sec    1.06  1565.3±68.00µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.6±7.23µs    17.3 MB/sec    1.07   182.8±10.95µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.13ms     8.0 MB/sec    1.05      3.3±0.11ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.08ms     5.3 MB/sec    1.07      8.3±0.07ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1574.9±20.76µs    10.6 MB/sec    1.06  1666.2±19.68µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    151.6±2.56µs    19.5 MB/sec    1.07    162.5±7.45µs    18.2 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.1 MB/sec    1.07      3.4±0.05ms     7.6 MB/sec
linter/all-rules/large/dataset.py          1.01     16.4±0.13ms     2.5 MB/sec    1.00     16.3±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    504.3±5.60µs     5.9 MB/sec    1.00    499.0±6.52µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.05ms     3.6 MB/sec    1.00      7.1±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.06ms     4.9 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1763.9±17.69µs     9.4 MB/sec    1.00  1742.1±16.48µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    201.3±3.10µs    14.7 MB/sec    1.00    196.5±2.09µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.8 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @Boshen on 2023-06-14 04:03_

Is this some kind of lexical scoping? 

Is it viable to save the span an identifier covers?

---

_Comment by @charliermarsh on 2023-06-14 04:20_

> Is this some kind of lexical scoping?

What do you mean exactly? Python uses lexical scoping and we support it in our semantic model. The challenge being articulated here is that Python supports three operations on names: load, store, and delete.

```python
def f():
  x = 1  # Load
  print(x)  # Store
  del x  # Delete
```

After `del x`, `x` is unbound.

We've been modeling deletions by removing the symbol from the binding table, which is... not great. It means that post-hoc analysis after the binding phase tends to get things wrong for code that makes use of deletions. This PR replaces deletions with a first-class concept.

> Is it viable to save the span an identifier covers?

I believe we do this (assuming your "span" is what we call `TextRange`).


---

_Comment by @Boshen on 2023-06-14 04:59_

In your semantic model, 

is it possible to mark where `x` covers so we can determine if it's unbound?

Given the example

```python
def f():
  x = 1  # Load
  print(x)  # Store
  del x  # Delete
  print(x)
```

x has a cover span from `x = 1` to `del x`. The final `x` in `print(x)` is not covered so it becomes an unresolved reference, thus you can report errors on it.

---

_@konstin approved on 2023-06-14 06:44_

---

_Comment by @charliermarsh on 2023-06-14 13:27_

@Boshen - Yeah! Ruff does handle that case correctly before and after this PR. I suspect we're not modeling it in an ideal way. We treat it as "shadowing". So in:

```py
def f():
  x = 1  # Load
  x = 2  # Load
```

The `x = 2` "shadows" the `x = 1` binding. We apply the same treatment to deletions here -- `del x` shadows the `x = 1` assignment, in your example, and we treat deleted symbols as unbound when you try to load them.

How does the "cover span" concept work? Is it based on ranges in the source code? I'm wondering how it would handle a case like:

```py
def f():
  x = 1
  x = x * 2
```

The `x = 1` assignment covers the `x = 1` snippet, as well as the _right-hand side_ of the `x = x * 2` snippet. So it covers non-contiguous ranges.


---

_Merged by @charliermarsh on 2023-06-14 13:27_

---

_Closed by @charliermarsh on 2023-06-14 13:27_

---

_Branch deleted on 2023-06-14 13:27_

---
