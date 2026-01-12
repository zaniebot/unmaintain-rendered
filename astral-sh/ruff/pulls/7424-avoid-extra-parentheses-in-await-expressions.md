```yaml
number: 7424
title: "Avoid extra parentheses in `await` expressions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/await
created_at: 2023-09-15T21:30:27Z
updated_at: 2023-09-16T17:19:47Z
url: https://github.com/astral-sh/ruff/pull/7424
synced_at: 2026-01-12T15:55:23Z
```

# Avoid extra parentheses in `await` expressions

---

_@charliermarsh_

## Summary

This PR aligns the await parenthesizing with the unary case, which is: if the value is already parenthesized, avoid parenthesizing; otherwise, only parenthesize if the _value_ needs parenthesizing.

Closes https://github.com/astral-sh/ruff/issues/7420.

## Test Plan

`cargo test`

No change in similarity.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               399 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99923 |               648 |                18 |
| zulip        |           0.99962 |              1437 |                22 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               399 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99923 |               648 |                18 |
| zulip        |           0.99962 |              1437 |                22 |


---

_@charliermarsh reviewed on 2023-09-15 21:34_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@expression__split_empty_brackets.py.snap`:182 on 2023-09-15 21:34_

This is actually a deviation from Black now.

Given:

```python
return await self().http_client().fetch(
    f"http://127.0.0.1:{self.port}{path}", method=method, **kwargs,
)
```

We now format as:

```python
return await (
    self()
    .http_client()
    .fetch(
        f"http://127.0.0.1:{self.port}{path}",
        method=method,
        **kwargs,
    )
)
```

Whereas Black formats as:

```python
return (
    await self()
    .http_client()
    .fetch(
        f"http://127.0.0.1:{self.port}{path}",
        method=method,
        **kwargs,
    )
)
```

I need to explore this further.

---

_Converted to draft by @charliermarsh on 2023-09-15 21:34_

---

_Label `formatter` added by @charliermarsh on 2023-09-15 21:34_

---

_Comment by @charliermarsh on 2023-09-15 21:34_

Draft for now, I need to explore this with the unary operator formatting.

---

_Marked ready for review by @charliermarsh on 2023-09-16 02:15_

---

_@MichaReiser approved on 2023-09-16 14:40_

Do we need the same for `yield`, `raise` and other keyword starting expressions? 

Please verify that the PR doesn't regress the similarity index for our test projects and update the PR summary with the metrics.

---

_Comment by @charliermarsh on 2023-09-16 17:10_

Will look into `yield` and `raise`.

---

_Merged by @charliermarsh on 2023-09-16 17:10_

---

_Closed by @charliermarsh on 2023-09-16 17:10_

---

_Branch deleted on 2023-09-16 17:10_

---

_Comment by @charliermarsh on 2023-09-16 17:19_

`raise` is not necessary since it's a statement, not an expression. `yield` needs some work.

---
