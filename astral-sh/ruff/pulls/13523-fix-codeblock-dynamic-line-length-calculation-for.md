```yaml
number: 13523
title: Fix codeblock dynamic line length calculation for indented examples
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/fix-docstring-codeblock-dynamic
created_at: 2024-09-26T13:20:52Z
updated_at: 2024-09-27T07:09:09Z
url: https://github.com/astral-sh/ruff/pull/13523
synced_at: 2026-01-10T20:59:36Z
```

# Fix codeblock dynamic line length calculation for indented examples

---

_Pull request opened by @MichaReiser on 2024-09-26 13:20_

## Summary

This PR fixes the `line-length` calculation for the mode `dynamic` for docstring code blocks. 

```python
def length(numbers: list[int]) -> int:
    """Get the length of the given list of numbers.

    Example:
        >>> length([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20])
        20
    """
    return len(numbers)
```

The current implementation only accounted for the docstring's indent and the space taken by the `>>> ` but it didn't account for the indent *inside* the docstring (for spaces relative to `Example`). This PR changes the line length calculation to consider the relative indent.

Fixes https://github.com/astral-sh/ruff/issues/13358


## Test Plan

I added a few new tests that and they demonstrate that the lines remain collapsed but expand in preview mode (with the fix).

I reviewed all ecosystem changes and they look correct. Many of those docstrings also have `E501` suppressions that won't be needed with the fix.

---

_Label `formatter` added by @MichaReiser on 2024-09-26 13:21_

---

_Label `preview` added by @MichaReiser on 2024-09-26 13:21_

---

_Comment by @github-actions[bot] on 2024-09-26 13:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+20 -6 lines in 4 files in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/3de9e1481d2aa7e737ba98526570d5e339e1eebd/airflow/models/baseoperator.py#L1264'>airflow/models/baseoperator.py~L1264</a>
```diff
             # This is equivalent to
             with DAG(...):
                 generate_content = GenerateContentOperator(task_id="generate_content")
-                send_email = EmailOperator(..., html_content="{{ task_instance.xcom_pull('generate_content') }}")
+                send_email = EmailOperator(
+                    ..., html_content="{{ task_instance.xcom_pull('generate_content') }}"
+                )
                 generate_content >> send_email
 
         """
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+14 -4 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/b9488218ae27b70d1669a932ab16e8ce5a257cf0/pandas/core/arrays/base.py#L1793'>pandas/core/arrays/base.py~L1793</a>
```diff
                # type for the array, to the physical storage type for
                # the data, before passing to take.
 
-               result = take(data, indices, fill_value=fill_value, allow_fill=allow_fill)
+               result = take(
+                   data, indices, fill_value=fill_value, allow_fill=allow_fill
+               )
                return self._from_sequence(result, dtype=self.dtype)
         """  # noqa: E501
         # Implementer note: The `fill_value` parameter should be a user-facing
```
<a href='https://github.com/pandas-dev/pandas/blob/b9488218ae27b70d1669a932ab16e8ce5a257cf0/pandas/plotting/_core.py#L247'>pandas/plotting/_core.py~L247</a>
```diff
     .. plot::
         :context: close-figs
 
-        >>> data = {"length": [1.5, 0.5, 1.2, 0.9, 3], "width": [0.7, 0.2, 0.15, 0.2, 1.1]}
+        >>> data = {
+        ...     "length": [1.5, 0.5, 1.2, 0.9, 3],
+        ...     "width": [0.7, 0.2, 0.15, 0.2, 1.1],
+        ... }
         >>> index = ["pig", "rabbit", "duck", "chicken", "horse"]
         >>> df = pd.DataFrame(data, index=index)
         >>> hist = df.hist(bins=3)
```
<a href='https://github.com/pandas-dev/pandas/blob/b9488218ae27b70d1669a932ab16e8ce5a257cf0/pandas/plotting/_core.py#L833'>pandas/plotting/_core.py~L833</a>
```diff
         :context: close-figs
 
         >>> df = pd.DataFrame(
-        ...     {"length": [1.5, 0.5, 1.2, 0.9, 3], "width": [0.7, 0.2, 0.15, 0.2, 1.1]},
+        ...     {
+        ...         "length": [1.5, 0.5, 1.2, 0.9, 3],
+        ...         "width": [0.7, 0.2, 0.15, 0.2, 1.1],
+        ...     },
         ...     index=["pig", "rabbit", "duck", "chicken", "horse"],
         ... )
         >>> plot = df.plot(title="DataFrame Plot")
```
<a href='https://github.com/pandas-dev/pandas/blob/b9488218ae27b70d1669a932ab16e8ce5a257cf0/pandas/plotting/_core.py#L1614'>pandas/plotting/_core.py~L1614</a>
```diff
             ...         "signups": [5, 5, 6, 12, 14, 13],
             ...         "visits": [20, 42, 28, 62, 81, 50],
             ...     },
-            ...     index=pd.date_range(start="2018/01/01", end="2018/07/01", freq="ME"),
+            ...     index=pd.date_range(
+            ...         start="2018/01/01", end="2018/07/01", freq="ME"
+            ...     ),
             ... )
             >>> ax = df.plot.area()
 
```

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pytest-dev/pytest/blob/6486c3f3a858a0c8043f5c3f7c24297b82a0abe4/src/_pytest/python_api.py#L858'>src/_pytest/python_api.py~L858</a>
```diff
 
        Given that ``pytest.raises`` matches subclasses, be wary of using it to match :class:`Exception` like this::
 
-           with pytest.raises(Exception):  # Careful, this will catch ANY exception raised.
+           with pytest.raises(
+               Exception
+           ):  # Careful, this will catch ANY exception raised.
                some_function()
 
        Because :class:`Exception` is the base class of almost all exceptions, it is easy for this to hide
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2024-09-26 14:52_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-09-26 14:52_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/preview.rs`:55 on 2024-09-26 17:34_

How come this is treated as a new style instead of a bug fix? (Sorry, I'm probably out of step with what our policy is here.)

---

_@BurntSushi approved on 2024-09-26 17:34_

---

_@MichaReiser reviewed on 2024-09-26 18:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/preview.rs`:55 on 2024-09-26 18:35_

The formatter versioning policy is rather strict:

> The stable style changed to prevent invalid syntax, changes to the program's semantics, or removal of comments

It only allows hard correctness errors. The only other permitted changes are bug fixes that don't change any already formatted code. For example, we can fix a bug where the formatter incorrectly collapses two blank lines to one instead of preserving one or two. 

The motivation for this strict rule is that a `ruff format --check` command shouldn't start failing between patch versions for already formatted code. 

---

_Merged by @MichaReiser on 2024-09-27 07:09_

---

_Closed by @MichaReiser on 2024-09-27 07:09_

---

_Branch deleted on 2024-09-27 07:09_

---
