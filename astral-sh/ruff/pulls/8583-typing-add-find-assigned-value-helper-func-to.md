```yaml
number: 8583
title: "[`typing`] Add `find_assigned_value` helper func to `typing.rs` to retrieve value of a given variable `id`"
type: pull_request
state: merged
author: qdegraaf
labels: []
assignees: []
merged: true
base: main
head: feat/refactorbindingcheck
created_at: 2023-11-09T13:49:09Z
updated_at: 2024-01-02T14:58:06Z
url: https://github.com/astral-sh/ruff/pull/8583
synced_at: 2026-01-12T15:55:26Z
```

# [`typing`] Add `find_assigned_value` helper func to `typing.rs` to retrieve value of a given variable `id`

---

_@qdegraaf_

## Summary

Adds `find_assigned_value` a function which gets the `&Expr` assigned to a given `id` if one exists in the semantic model. 

Open TODOs:

- [ ] Handle `binding.kind.is_unpacked_assignment()`: I am bit confused by this one. The snippet from its documentation does not appear to be counted as an unpacked assignment and the only ones I could find for which that was true were invalid Python like:
```python
x, y = 1 
```
- [ ] How to handle AugAssign. Can we combine statements like:
```python
(a, b) = [(1, 2, 3), (4,)]
a += (6, 7)
```
to get the full value for a? Code currently just returns `None` for these assign types

- [ ] Multi target assigns
```python
m_c = (m_d, m_e) = (0, 0)
trio.sleep(m_c)  # OK
trio.sleep(m_d)  # TRIO115
trio.sleep(m_e)  # TRIO115
```

## Test Plan

Used the function in two rules:

- `TRIO115`
- `PERF101`

Expanded both their fixtures for explicit multi target check


---

_Comment by @github-actions[bot] on 2023-11-09 23:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @qdegraaf on 2023-11-13 12:43_

---

_Comment by @qdegraaf on 2023-11-13 12:44_

@charliermarsh I'm thinking the `binding.kind.is_unpacked_assignment()` can be left as a TODO for later when it is necessary? Let me know if that is acceptable. I think it would be good to review current set-up and logic first regardless, before expanding further.

---

_@charliermarsh reviewed on 2023-11-22 00:01_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/typing.rs`:594 on 2023-11-22 00:01_

What happens here in the case of multi-target assignments?

---

_@qdegraaf reviewed on 2023-11-22 10:19_

---

_Review comment by @qdegraaf on `crates/ruff_python_semantic/src/analyze/typing.rs`:594 on 2023-11-22 10:19_

Good catch. It works for 

```python
x = y = 0
```

Added example of that to TRIO115 fixture

Does not work for 

```python
(x, y) = [a, b] = (0, 0)
```

Which will only manage to retrieve the values for `x` and `y` right now. 

Would it be an idea to set up unit tests for this and the other new func in the module? If so, is there a leading example of non plugin unit tests I can have a look at? 

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/typing.rs`:594 on 2023-11-22 18:31_

Is it able to detect `y` in `x = y = 0`?

---

_@charliermarsh reviewed on 2023-11-22 18:31_

---

_@charliermarsh reviewed on 2023-11-22 18:33_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/typing.rs`:594 on 2023-11-22 18:33_

Yeah, we don't have great infrastructure for this. I'd probably just recommend adding more trio test cases as a means of testing this behavior.

---

_Comment by @charliermarsh on 2023-11-22 18:45_

I think there's one more case to consider:

```python
m_c = (m_d, m_e) = (0, 0)
trio.sleep(m_c)  # OK
trio.sleep(m_d)  # TRIO115
trio.sleep(m_e)  # TRIO115
```

Right now, these _all_ get matched to `(0, 0)`.


---

_@qdegraaf reviewed on 2023-11-22 18:57_

---

_Review comment by @qdegraaf on `crates/ruff_python_semantic/src/analyze/typing.rs`:594 on 2023-11-22 18:57_

> Is it able to detect `y` in `x = y = 0`?

Yeah this one works. But if one of the targets is a collection there are issues. e.g.

```python
a = (x, y) = (0, 0)
(a, b) = [c, d] = [0, 0]
```
Not entirely sure how to best fix that. Flag it as a multi assign if the top-level targets has a length larger than 1 and add a separate branch for that? Or are there single target scenarios that could be mistaken then? 


---

_Comment by @qdegraaf on 2023-11-22 19:01_

> I think there's one more case to consider:
> 
> ```python
> m_c = (m_d, m_e) = (0, 0)
> trio.sleep(m_c)  # OK
> trio.sleep(m_d)  # TRIO115
> trio.sleep(m_e)  # TRIO115
> ```
> 
> Right now, these _all_ get matched to `(0, 0)`.

Ho missed this one when I posted https://github.com/astral-sh/ruff/pull/8583#discussion_r1402583307 

Added it to TODOs. Gonna look at this again, see if there's a way to be aware of the multi target nature of the assign before drilling down. 

---

_Renamed from "[`typing`] Add `get_assigned_value` helper func to `typing.rs` to retrieve value of a given variable `id`" to "[`typing`] Add `find_assigned_value` helper func to `typing.rs` to retrieve value of a given variable `id`" by @qdegraaf on 2023-11-24 10:24_

---

_Converted to draft by @qdegraaf on 2023-11-30 17:53_

---

_@charliermarsh approved on 2023-12-13 00:19_

---

_Marked ready for review by @charliermarsh on 2023-12-13 00:19_

---

_Merged by @charliermarsh on 2023-12-13 00:24_

---

_Closed by @charliermarsh on 2023-12-13 00:24_

---

_Branch deleted on 2024-01-02 14:58_

---
