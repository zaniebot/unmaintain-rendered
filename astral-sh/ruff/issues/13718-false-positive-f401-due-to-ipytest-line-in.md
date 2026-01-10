```yaml
number: 13718
title: "False positive F401 due to `%%ipytest` line in notebook"
type: issue
state: closed
author: tovrstra
labels:
  - notebook
assignees: []
created_at: 2024-10-11T18:28:12Z
updated_at: 2024-10-14T15:42:22Z
url: https://github.com/astral-sh/ruff/issues/13718
synced_at: 2026-01-10T11:09:55Z
```

# False positive F401 due to `%%ipytest` line in notebook

---

_Issue opened by @tovrstra on 2024-10-11 18:28_

Keywords to find existing issue: F401 and ipytest. The following is very similar, but still different, it seems: https://github.com/astral-sh/ruff/issues/10454

A minimal working example would be a Jupyter notebook with the following two cells:

```python
import ipytest
import pytest

ipytest.autoconfig()
```

```python
%%ipytest

def test_simple():
    with pytest.raises(TypeError):
        print(1 + "a")
```

Ruff will report `F401 (unused-import)` for the line `import pytest`.

To reproduce the issue, create a notebook with the above cells, called `bug.ipynb` and run the command:

```bash
ruff check demos/bug.ipynb --fix
````

I've tested with with ruff 0.6.3 and 0.6.9. I have the following in my pyproject.toml, but I don't think these settings matter:

```toml
[tool.ruff]
line-length = 100
target-version = "py311"

[tool.ruff.lint]
select = ["E", "F", "UP", "B", "I", "PGH", "PL", "RUF"]
ignore = ["E741", "PLR2004", "PLR0913", "PLR0912", "PLW2901", "PLR0915"]
````

As a workaround, one can (obviously) add a comment `# noqa: F401` to the import line

When removing the line `%%ipytest`, the problem goes away, but it that doesn't help because the line is doing something useful.

Given the easy workaround, this is probably not urgent.

By the way, thanks for creating Ruff!

---

_Label `notebook` added by @AlexWaygood on 2024-10-11 22:17_

---

_Comment by @MichaReiser on 2024-10-13 14:23_

I don't fully understnad yet what's happening but the code works as expected when I change `%%ipytest` to `%ipytest`



---

_Comment by @MichaReiser on 2024-10-14 08:58_

@dhruvmanila do you have an idea why using the `%%magic` form makes the diagnostic appear?

---

_Comment by @dhruvmanila on 2024-10-14 09:02_

Yeah, the double percent like `%%magic` are cell magics which apply to the entire cell and so we ignore those cells except for a few: https://github.com/astral-sh/ruff/blob/fd5fcc498b82c13b81dfb1f073bd18167c561e7b/crates/ruff_notebook/src/cell.rs#L217-L244

While, the single percent like `%magic` are line magics that considers only the line on which it is defined.

---

_Comment by @MichaReiser on 2024-10-14 09:04_

Oh that's good to know. Thanks. So the solution here would be to move the `%%ipytest` command to its own cell?

---

_Comment by @dhruvmanila on 2024-10-14 09:05_

I think it is in its own cell. It's just that the entire cell is currently ignored by Ruff because `ipytest` is being special cased. I think we _could_ do it because the lines following the `ipytest` magic should contain valid Python code. @tovrstra I'm not familiar with the magic command so just want to confirm if that's valid. I can check as well.

---

_Comment by @MichaReiser on 2024-10-14 09:22_

What I mean is that a possible workaround is to change the notebook to have three cells:

```python
import ipytest
import pytest

ipytest.autoconfig()
```

```python
%%ipytest
```

```python
def test_simple():
    with pytest.raises(TypeError):
        print(1 + "a")
```

---

_Comment by @dhruvmanila on 2024-10-14 09:36_

I think then the magic command won't work because it won't be applied to the cell it was previously targeting.

---

_Comment by @dhruvmanila on 2024-10-14 09:46_

I just tested it, it's safe to include `ipytest` in the allowed cell magic in the following code. And, the implementation of `%%ipytest` also confirms that it runs the cell first and then runs pytest on top of it. So, the cell can be considered as being without the magic itself.

 https://github.com/astral-sh/ruff/blob/fd5fcc498b82c13b81dfb1f073bd18167c561e7b/crates/ruff_notebook/src/cell.rs#L241-L244

<img width="1102" alt="Screenshot 2024-10-14 at 15 14 42" src="https://github.com/user-attachments/assets/d3ba2d08-c904-4f4c-91e7-b04aeffef47c">


---

_Closed by @dhruvmanila on 2024-10-14 10:18_

---

_Comment by @tovrstra on 2024-10-14 15:42_

Thank you all for the comments and the fix. My apologies for commenting late. Indeed, the `%%ipytest` should be on the first line of the cell containing unit tests. It means "this cell contains unit tests". If it is put in a separate cell or further down, the tests will not get executed when running the cell. 

---
