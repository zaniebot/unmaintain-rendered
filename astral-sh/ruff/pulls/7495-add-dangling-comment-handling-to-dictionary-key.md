```yaml
number: 7495
title: Add dangling comment handling to dictionary key-value pairs
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/dict
created_at: 2023-09-18T16:24:51Z
updated_at: 2023-09-19T19:17:22Z
url: https://github.com/astral-sh/ruff/pull/7495
synced_at: 2026-01-12T15:55:24Z
```

# Add dangling comment handling to dictionary key-value pairs

---

_@charliermarsh_

## Summary

This PR fixes a formatting instability by changing the comment handling around the `:` in a dictionary to mirror that of the `:` in a lambda: we treat comments around the `:` as dangling, then format them after the `:`.

Closes https://github.com/astral-sh/ruff/issues/7458.

## Test Plan

`cargo test`

No change in similarity.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99956 |              2587 |               404 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99929 |               648 |                16 |
| zulip        |           0.99969 |              1437 |                21 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99956 |              2587 |               404 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99929 |               648 |                16 |
| zulip        |           0.99969 |              1437 |                21 |


---

_Label `formatter` added by @charliermarsh on 2023-09-18 16:24_

---

_Comment by @codspeed-hq[bot] on 2023-09-18 17:24_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/dict)

### Merging #7495 will **not alter performance**

<sub>Comparing <code>charlie/dict</code> (86f3d0e) with <code>main</code> (b792140)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-18 17:37_

---

_Review requested from @konstin by @charliermarsh on 2023-09-18 17:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1260 on 2023-09-18 19:33_

I wonder if this gets comments wrong, when the value or `key` are perenthesized. 

I lean towards making comments before `:` trailing comments of `key` and all other comments leading comments of value. You may need to force hard line break in the dictionary formatting when the value has any leading comments or the key has a trailing comment end of line comment)

---

_@MichaReiser reviewed on 2023-09-18 19:34_

---

_@charliermarsh reviewed on 2023-09-18 19:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1260 on 2023-09-18 19:54_

It does in some cases but not all -- some cases are already handled by `handle_parenthesized_comment`. I will fix the others.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1260 on 2023-09-18 20:00_

I guess I can try this but candidly it feels more complicated. I will almost certainly need to change the `trailing_comments` implementation -- otherwise, this:
```python
{
    x:  # comment
    y
}
```

Will get formatted as:

```python
{
    x:# comment
    y
}
```

Let's see how it goes.


---

_@charliermarsh reviewed on 2023-09-18 20:00_

---

_@charliermarsh reviewed on 2023-09-18 20:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1260 on 2023-09-18 20:02_

We will also have to be able to differentiate between these cases when formatting the dictionary:

```python
{
    x:  # comment
    y
}
{
    x: (  # comment
        y
    )
}
```

Since in the first case, we don't want to insert any spaces after the `:` (`trailing_comments` would need to do this), whereas in the second case we do. I like that the current solution gets this right automatically, since the parenthesized comment is a leading comment but the unparenthesized comment is dangling.


---

_@charliermarsh reviewed on 2023-09-18 20:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1260 on 2023-09-18 20:03_

I'm actually not quite sure how to support that second case (of the unparenthesized vs. parenthesized leading end-of-line comment).

---

_@charliermarsh reviewed on 2023-09-18 20:11_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1260 on 2023-09-18 20:11_

Unless we are comfortable making `# comment` an own-line comment? Pushing it to the next line? But that feels off.

---

_@konstin approved on 2023-09-19 07:57_

---

_Comment by @charliermarsh on 2023-09-19 19:11_

I'm gonna move forward with this -- I think it's an improvement, and I find the dangling comments approach easier to reason about than the alternatives right now.

---

_Merged by @charliermarsh on 2023-09-19 19:17_

---

_Closed by @charliermarsh on 2023-09-19 19:17_

---

_Branch deleted on 2023-09-19 19:17_

---
