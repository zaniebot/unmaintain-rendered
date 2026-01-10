```yaml
number: 7315
title: "Formatter undocumented deviation: annotation wrapping"
type: issue
state: closed
author: JonathanPlasse
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-09-12T20:46:05Z
updated_at: 2023-09-29T17:27:36Z
url: https://github.com/astral-sh/ruff/issues/7315
synced_at: 2026-01-10T11:09:49Z
```

# Formatter undocumented deviation: annotation wrapping

---

_Issue opened by @JonathanPlasse on 2023-09-12 20:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Black formatting
```python
bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb: Bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb = (
    Bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb()
)
```
Ruff formatting
```python
bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb: (
    Bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
) = Bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb()
```

Use ruff 0.0.289.
Both are set to a line length of 100.

---

_Comment by @MichaReiser on 2023-09-13 07:05_

Thanks for reporting this deviation. Black seems to preserve parentheses in type annotation positions without ever introducing new parentheses. 

This is inconsistent with other places where expression appear as a direct child of a statement (other than expression statement) where Black prefers to strip any unnecessary parentheses (and applies the custom "can omit parentheses" layout). 

This can be solved "easily" by changing this line to use `annotation.format()` instead of `maybe_parenthesize` but it means that Ruff is more likely to exceed the line width

https://github.com/astral-sh/ruff/blob/c05e4628b1d385d106e7e3d355f5d1d76fbfe401/crates/ruff_python_formatter/src/statement/stmt_ann_assign.rs#L22-L30

Changing the line accordingly improves the similarity index for typeshed by 0.00003

This is probably related to https://github.com/astral-sh/ruff/issues/7322

---

_Comment by @MichaReiser on 2023-09-13 07:10_

Copied over from #7322 

> I can see how Black's formatting may seem better but Black's code exceeds the configured line width of 100, whereas Ruff's formatting stays in the limit. 

> Black is also a bit inconsistent here because it uses the same formatting as ruff if you change this to an assignment:

  ```python
  animation_data = (
	  bpy.types.AnimData
  )  # TODO(jonathan): Change to Optional when fake-bpy-module is updated
  ```

> I propose that we stick to Ruff's layout because:
>  * It is more consistent with other syntax
>  * It stays within the configured line length


> I'll leave this open for now to let other people chime in.

---

_Label `formatter` added by @MichaReiser on 2023-09-13 07:10_

---

_Label `needs-decision` added by @MichaReiser on 2023-09-13 07:10_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-13 07:10_

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-27 15:45_

---

_Comment by @charliermarsh on 2023-09-27 15:47_

This is a known deviation -- can be closed once it's documented.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 18:45_

---

_Label `documentation` added by @MichaReiser on 2023-09-28 09:47_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---
