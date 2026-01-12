```yaml
number: 8214
title: Formatter -- Length of line exceeds the default width
type: issue
state: closed
author: vigneshmanick
labels:
  - formatter
assignees: []
created_at: 2023-10-25T12:58:29Z
updated_at: 2023-10-25T16:08:59Z
url: https://github.com/astral-sh/ruff/issues/8214
synced_at: 2026-01-12T15:54:47Z
```

# Formatter -- Length of line exceeds the default width

---

_@vigneshmanick_

Hello,

Congrats on releasing the new formatter. Working very well so far. 

I have noticed a deviation from black formatting for the sample code below.  The line `loader=PackageLoader(package_name="longnameofpkg.longmodule_nameee", package_path="resources"),`  is still longer than 88 after formatting with ruff. The long variable name, is also not wrapped to next line.

I have added the output of both black and ruff below.  

Is this a known deviation from black?

```
ruff -V
ruff 0.1.2

black --version
black, 23.10.0 (compiled: no)
Python (CPython) 3.11.6
```

```py
from jinja2 import Environment, PackageLoader


class TestClass:
    def __init__(self):
        self.averylooooooooooooooooooongvariablenameeeeeeeeeeeeee = ["something",  "somethingelse"]

    def some_function(self):
        environment = Environment(
            loader=PackageLoader(
                package_name="longnameofpkg.longmodule_nameee", package_path="resources"
            ),
            trim_blocks=True,
        )
        template = environment.get_template("Template.txt")
        template.filename
```

### Ruff
```py
class TestClass:
    def __init__(self):
        self.averylooooooooooooooooooongvariablenameeeeeeeeeeeeee = ["something", "somethingelse"]

    def some_function(self):
        environment = Environment(
            loader=PackageLoader(package_name="longnameofpkg.longmodule_nameee", package_path="resources"),
            trim_blocks=True,
        )
        template = environment.get_template("Template.txt")
        template.filename
```

### Black

```py
class TestClass:
    def __init__(self):
        self.averylooooooooooooooooooongvariablenameeeeeeeeeeeeee = [
            "something",
            "somethingelse",
        ]

    def some_function(self):
        environment = Environment(
            loader=PackageLoader(
                package_name="longnameofpkg.longmodule_nameee", package_path="resources"
            ),
            trim_blocks=True,
        )
        template = environment.get_template("Template.txt")
        template.filename
```


---

_Comment by @vigneshmanick on 2023-10-25 13:58_

False alarm, the line-length from pyproject.toml was the issue. Black naturally didn't read `tool.ruff` setting. 

Sorry!

---

_Closed by @vigneshmanick on 2023-10-25 13:58_

---

_Comment by @charliermarsh on 2023-10-25 16:08_

Phew, this one surprised me! :)

---

_Label `formatter` added by @charliermarsh on 2023-10-25 16:08_

---
