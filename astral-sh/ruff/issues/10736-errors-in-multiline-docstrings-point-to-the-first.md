```yaml
number: 10736
title: Errors in multiline docstrings point to the first line of the string
type: issue
state: closed
author: ts933
labels:
  - docstring
assignees: []
created_at: 2024-04-02T12:35:59Z
updated_at: 2024-04-03T02:10:05Z
url: https://github.com/astral-sh/ruff/issues/10736
synced_at: 2026-01-12T15:54:50Z
```

# Errors in multiline docstrings point to the first line of the string

---

_@ts933_

_ruff version==0.2.1_

pyproject.toml
```

[tool.ruff]
preview = true

select = [
  "C90", # McCabe
  "D",   # pydocstyle
  "E",   # pycodestyle
  "F",   # pyflakes
  "NPY", # numpy specific
  "PL",  # pylint
  "PTH", # flake8-use-pathlib
  "SIM", # flake8-simplify
  "TD",  # flake8-todos
  "UP",  # pyupgrade
  "W",   # pycodestyle
]
line-length = 120

[tool.ruff.pydocstyle]
convention = "google"

[tool.ruff.mccabe]
max-complexity = 5


```


---

_Comment by @MichaReiser on 2024-04-02 12:51_

@ts933 would you mind providing a minimal code example that demonstrates the problem? It would help us to understand what you're asking for or consider a bug. Thank you

---

_Label `needs-info` added by @MichaReiser on 2024-04-02 12:51_

---

_Comment by @ts933 on 2024-04-02 12:56_

Code with 4 errors all pointing to the same line --> 10

```
`def extract_category(dataframe,  # type: ignore   # pragma: no cover
                     keyword,
                     filter_on_column='category',
                     label='io') -> pd.DataFrame:
    """Extracts rows from a given dataframe based on a filter condition and assigns a label of 0 or 1 to each row.

    Args:

        dataframe (pyspark.sql.DataFrame): The input dataframe.
        keyword (str): The keyword to match in the filter column.
        filter_on_column (str, optional): The column to filter on.
        label (str): If we want to label the data as IO or NIO.
    Returns:

        pd.DataFrame: The extracted rows with an additional 'label' column assigned with a value of 1.
    """`
```

file.py:10:5: D410 [*] Missing blank line after section ("Args")
file.py:10:5: D412 [*] No blank lines allowed between a section header and its content ("Args")
file.py:10:5: D411 [*] Missing blank line before section ("Returns")
file.py:10:5: D412 [*] No blank lines allowed between a section header and its content ("Returns")

---

_Comment by @MichaReiser on 2024-04-02 13:03_

I would need to look into what the reason for this is. @charliermarsh any  chance it is so that the noqa comments work for multiline docstrings (because they match on the start of the line?)

---

_Comment by @charliermarsh on 2024-04-02 13:45_

No, that shouldn’t be necessary for the noqa enforcement. I can take a look at this. My guess is we highlight the whole docstring, and so the start character is indeed the first line? Not sure though, I’ll look later.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-02 13:45_

---

_Comment by @charliermarsh on 2024-04-02 18:15_

Yeah, these rules just use the range of the docstring. We could consider refining them.

---

_Label `needs-info` removed by @charliermarsh on 2024-04-02 18:22_

---

_Label `docstring` added by @charliermarsh on 2024-04-02 18:22_

---

_Closed by @charliermarsh on 2024-04-03 02:10_

---
