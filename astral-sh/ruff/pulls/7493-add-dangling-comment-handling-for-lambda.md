```yaml
number: 7493
title: "Add dangling comment handling for `lambda` expressions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/lambda
created_at: 2023-09-18T15:17:06Z
updated_at: 2023-09-19T19:23:53Z
url: https://github.com/astral-sh/ruff/pull/7493
synced_at: 2026-01-12T02:39:10Z
```

# Add dangling comment handling for `lambda` expressions

---

_Pull request opened by @charliermarsh on 2023-09-18 15:17_

## Summary

This PR adds dangling comment handling for `lambda` expressions. In short, comments around the `lambda` and the `:` are all considered dangling. Comments that come between the `lambda` and the `:` may be moved after the colon for simplicity (this is an odd position for a comment anyway), unless they also precede the lambda parameters, in which case they're formatted before the parameters.

Closes https://github.com/astral-sh/ruff/issues/7470.

## Test Plan

`cargo test`

No change in similarity.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               398 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99929 |               648 |                16 |
| zulip        |           0.99962 |              1437 |                22 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               398 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99929 |               648 |                16 |
| zulip        |           0.99962 |              1437 |                22 |


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-18 15:17_

---

_Review requested from @konstin by @charliermarsh on 2023-09-18 15:17_

---

_Label `bug` added by @charliermarsh on 2023-09-18 15:18_

---

_Label `formatter` added by @charliermarsh on 2023-09-18 15:18_

---

_Label `bug` removed by @charliermarsh on 2023-09-18 17:47_

---

_@MichaReiser reviewed on 2023-09-18 19:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1636 on 2023-09-18 19:37_

What's the reason for making all comments dangling vs forcing line breaks when formatting the lambda if the argument e.g. has leading comments?

I generally try to avoid using dangling comments and instead attach comments to nodes. It tends to work better, especially if we ever want to support node-level suppression comments.

---

_@MichaReiser reviewed on 2023-09-18 19:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1666 on 2023-09-18 19:38_

Can we consider own line comments as leading comments?

---

_@MichaReiser reviewed on 2023-09-18 19:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1678 on 2023-09-18 19:39_

Same here, what about own line comments? Could they be leading comments of the body

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1666 on 2023-09-18 19:47_

How would that work for this case?

```python
(
  lambda  # 1
  # 2
  : # 3
  # 4
  x
)
```

We could make `# 4` a leading comment, but I don't think any of the others can be treated as such.

---

_@charliermarsh reviewed on 2023-09-18 19:47_

---

_@charliermarsh reviewed on 2023-09-18 19:51_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1636 on 2023-09-18 19:51_

Are you suggesting that `# 1` here should be a leading comment? Would you expect it to be moved to an own-line comment, or rendered as-is in this snippet?

---

_@charliermarsh reviewed on 2023-09-18 20:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1678 on 2023-09-18 20:04_

Yes the own-line comments could in this case. I found it simpler to have fewer cases but I can change that.

---

_@konstin reviewed on 2023-09-19 07:19_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1636 on 2023-09-19 07:19_

we could do it similar to lists, where 1 is dangling and 2 is leading:
```python
x = [  # 1
    # 2
    "a",
    "b",
    "c",
]
```

```
7..10, None, Some((ExprConstant, 23..26)), (ExprList, 4..47), "# 1"
15..18, None, Some((ExprConstant, 23..26)), (ExprList, 4..47), "# 2"
{
    Node {
        kind: ExprList,
        range: 4..47,
        source: `[  # 1⏎`,
    }: {
        "leading": [],
        "dangling": [
            SourceComment {
                text: "# 1",
                position: EndOfLine,
                formatted: true,
            },
        ],
        "trailing": [],
    },
    Node {
        kind: ExprConstant,
        range: 23..26,
        source: `"a"`,
    }: {
        "leading": [
            SourceComment {
                text: "# 2",
                position: OwnLine,
                formatted: true,
            },
        ],
        "dangling": [],
        "trailing": [],
    },
}
x = [  # 1
    # 2
    "a",
    "b",
    "c",
]
```

---

_@konstin approved on 2023-09-19 07:19_

---

_Comment by @charliermarsh on 2023-09-19 19:23_

Okay, I played with this one too and I think the current solution is simpler than trying to make some of these leading comments. Part of the issue is that if we make any of these leading or trailing comments, it's hard to disambiguate between these cases:

```python
(
    lambda:
    # comment
    x
)

(
    lambda:  (
        # comment
        x
    )
)
```

It's not _impossible_, but it requires more special-casing in placement, and then (more importantly) the `lambda` formatter becomes more complicated, since the logic for inserting a hard line break after the `:` has to take into account whether the body has leading own-line comments within or outside of parentheses.

---

_Merged by @charliermarsh on 2023-09-19 19:23_

---

_Closed by @charliermarsh on 2023-09-19 19:23_

---

_Branch deleted on 2023-09-19 19:23_

---
