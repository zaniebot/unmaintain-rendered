```yaml
number: 10881
title: Format suppression is ignored for imports
type: issue
state: closed
author: b-phi
labels: []
assignees: []
created_at: 2024-04-11T14:55:35Z
updated_at: 2024-04-11T16:40:19Z
url: https://github.com/astral-sh/ruff/issues/10881
synced_at: 2026-01-10T11:09:53Z
```

# Format suppression is ignored for imports

---

_Issue opened by @b-phi on 2024-04-11 14:55_

I have a long import line that I'd like to custom format to avoid exceeding the configured line length. Neither `# fmt: skip` or `# fmt: off` seem to prevent ruff from reformatting the line. I confirmed that format suppression does work for other statements in the file.


Input
```python
x =           2  # fmt: skip
from my.very_very_very_very_very_very_very_long_package\
    import my_very_very_very_very_very_very_long_name  # fmt: skip

# or

# fmt: off
x =           2
from my.very_very_very_very_very_very_very_long_package\
    import my_very_very_very_very_very_very_long_name
# fmt: on
```

Output
```python
x =           2  # fmt: skip
from my.very_very_very_very_very_very_very_long_package import (
    my_very_very_very_very_very_very_long_name
)

# or

# fmt: off
x =           2
from my.very_very_very_very_very_very_very_long_package import (
    my_very_very_very_very_very_very_long_name
)
# fmt: on
```

Version: 0.3.5

Config
```yaml
- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.3.5
  hooks:
    - id: ruff-format
    - id: ruff
```

```toml
[tool.ruff]
line-length = 79
fix = true

[tool.ruff.format]
quote-style = 'single'

[tool.ruff.lint]
extend-select = ['E501', 'I']
```



<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @MichaReiser on 2024-04-11 14:58_

Thanks for the detailed write up. I pasted your example in the playground and the formatting looks correct (the formatter doesn't reformat the file).

What I suspect is that isort reformats the file which then makes the formatter format the file.

---

_Comment by @b-phi on 2024-04-11 15:15_

Hey Micha, thanks for the quick reply. You are right, if I remove the `I` from extend-select, the import is no longer formatted. Is there a way to disable the import formatting for a specific line?

Edit: The isort action comments don't work

---

_Comment by @MichaReiser on 2024-04-11 16:22_

I think this works, but it doesn't just disable the formatting of isort, it also means that `isort` won't sort that line anymore

```python
from my.very_very_very_very_very_very_very_long_package\ # isort:skip
    import my_very_very_very_very_very_very_long_name  # fmt: skip
```

and it looks super silly. 

---

_Comment by @b-phi on 2024-04-11 16:28_

> I think this works, but it doesn't just disable the formatting of isort, it also means that `isort` won't sort that line anymore
> 
> ```python
> from my.very_very_very_very_very_very_very_long_package\ # isort:skip
>     import my_very_very_very_very_very_very_long_name  # fmt: skip
> ```
> 
> and it looks super silly.

Comment after a backslash seems to be a syntax error, but this does work. Probably even sillier though. 
```python
# isort: off
# fmt: off
from my.very_very_very_very_very_very_very_long_package\
    import my_very_very_very_very_very_very_long_name
# isort: on
# fmt: on
```

---

_Comment by @MichaReiser on 2024-04-11 16:37_

Oh right. lol. So it worked because it had a syntax error. Silly me. But yes, that's similar. You could probably keep the `fmt: skip` at the end of the line but I don't think it makes a real difference.

---

_Comment by @b-phi on 2024-04-11 16:40_

Thanks for the help. Maybe `# fmt: off` / `# fmt: skip` should imply the isort variants, just a thought.

---

_Closed by @b-phi on 2024-04-11 16:40_

---
