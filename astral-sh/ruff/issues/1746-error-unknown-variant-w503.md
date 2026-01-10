```yaml
number: 1746
title: "error unknown variant `W503`"
type: issue
state: closed
author: 12rambau
labels: []
assignees: []
created_at: 2023-01-09T16:15:45Z
updated_at: 2023-01-09T16:30:11Z
url: https://github.com/astral-sh/ruff/issues/1746
synced_at: 2026-01-10T11:09:43Z
```

# error unknown variant `W503`

---

_Issue opened by @12rambau on 2023-01-09 16:15_

I am moving from `flake8` to `ruff` and one of my ignore statements is not recognized by 0.0.215. 

```toml
[tool.pytest.ini_options]
testpaths = "tests"

[tool.ruff]
fix = true
select = ["E", "F", "W", "I", "D", "N", "RUF"]
ignore = [
    "E501",  # line too long | Black take care of it
    "W605",  # invalid escape sequence | we escape specific characters for sphinx
    "W503",  # line break before binary operator | Black takes care of it
]

# init file are here to hide the internal structure to the user of the lib
exclude = ["*/__init__.py", "docs/source/conf.py"]

[tool.ruff.flake8-quotes]
docstring-quotes = "double"
```

I checked and W503 do exist: https://www.flake8rules.com/rules/W503.html

Is it normal ? is it missing ? can I safely remove this ignore statement if it's not taken into account ?


---

_Comment by @charliermarsh on 2023-01-09 16:20_

Thanks for trying Ruff!

The main limitation when comparing Ruff to Flake8 is that Ruff currently doesn't implement the Flake8 rules that are made obsolete by Black (see [FAQ](https://github.com/charliermarsh/ruff#how-does-ruff-compare-to-flake8)). So that includes several of the pycodestyle rules that start with `E` and `W`, including `W503`. (That actually aligns with the comment you have above: `# line break before binary operator | Black takes care of it`.)

So, yes, if you're using Black, it should be totally fine to ignore.


---

_Closed by @charliermarsh on 2023-01-09 16:20_

---

_Comment by @12rambau on 2023-01-09 16:29_

thanks!  deploying in my CI then! 


---
