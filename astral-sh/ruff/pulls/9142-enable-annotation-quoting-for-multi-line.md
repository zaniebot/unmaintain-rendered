```yaml
number: 9142
title: Enable annotation quoting for multi-line expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/multi-quote
created_at: 2023-12-15T00:57:36Z
updated_at: 2023-12-15T01:11:02Z
url: https://github.com/astral-sh/ruff/pull/9142
synced_at: 2026-01-12T15:55:27Z
```

# Enable annotation quoting for multi-line expressions

---

_@charliermarsh_

Given:

```python
x: DataFrame[
    int
] = 1
```

We currently wrap the annotation in single quotes, which leads to a syntax error:

```python
x: "DataFrame[
    int
]" = 1
```

There are a few options for what to suggest for users here... Use triple quotes:

```python
x: """DataFrame[
    int
]""" = 1
```

Or, use an implicit string concatenation (which may require parentheses):

```python
x: ("DataFrame["
    "int"
"]") = 1
```

The solution I settled on here is to use the `Generator`, which effectively means we write it out on a single line, like:

```python
x: "DataFrame[int]" = 1
```

It's kind of the "least opinionated" solution, but it does mean we'll expand to a very long line in some cases.

Closes https://github.com/astral-sh/ruff/issues/9136.

---

_Label `bug` added by @charliermarsh on 2023-12-15 00:59_

---

_Marked ready for review by @charliermarsh on 2023-12-15 01:00_

---

_Merged by @charliermarsh on 2023-12-15 01:03_

---

_Closed by @charliermarsh on 2023-12-15 01:03_

---

_Branch deleted on 2023-12-15 01:03_

---

_Comment by @MichaReiser on 2023-12-15 01:05_

> Or, use an implicit string concatenation (which may require parentheses):

I wonder if that trips over static analysis tools. 

> The solution I settled on here is to use the Generator, which effectively means we write it out on a single line, like:

The main downside of this is that it undoes any formatting and the formatter won't be able to properly format the type annotation (except if we would parse it) on the next run, because it is now an unsplittable string. But I think it's a good enough solution to fix the bug.

---

_Comment by @github-actions[bot] on 2023-12-15 01:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
