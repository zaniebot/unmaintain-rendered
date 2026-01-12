```yaml
number: 12865
title: DJ012 is not raised when a parent class, that inherits from django.Model, is in a different file.
type: issue
state: open
author: HappyCthulhu
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-08-13T15:59:43Z
updated_at: 2025-10-07T17:00:38Z
url: https://github.com/astral-sh/ruff/issues/12865
synced_at: 2026-01-12T15:54:52Z
```

# DJ012 is not raised when a parent class, that inherits from django.Model, is in a different file.

---

_@HappyCthulhu_

### Search keywords 
"DJ012", "django Model"

### Ruff Version

0.5.7

### Config

<details>
    <summary> Part of pyproject.toml</summary>

```
[tool.poetry.group.dev.dependencies]
ruff = "0.5.7"

[tool.ruff]
line-length = 120
extend-exclude = ["venv", "stubs"]

[tool.ruff.lint]
select = ["ALL"]
unfixable = ["ERA001", "COM819"] # remove comment out code lines
ignore = [
    "Q", # flake8-quotes
    "D100",	# undocumented-public-module	Missing docstring in public module
    "D101",	# undocumented-public-class	"Missing" # docstring in public class
    "D102",	# undocumented-public-method	Missing docstring "in" # public method
    "D103",	# undocumented-public-function	Missing docstring in "public" # function
    "D104",	# undocumented-public-package	Missing docstring in public package	"🛠"
    "D105",	# undocumented-magic-method	Missing docstring in magic method
    "D106",	# # undocumented-public-nested-class	Missing docstring in public nested class
    "D107",	# # undocumented-public-init	Missing docstring in __init__
    "ANN101", # missing-type-self
    "TD003", # missing-todo-link missing issue link on the line following this TODO
    "TD002", # missing-todo-author  missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`Ruff
    "S101", # assert Use of assert detected	
    "ANN002", #	missing-type-args	Missing type annotation for *{name}
    "ANN003", #	missing-type-kwargs	Missing type annotation for **{name}
    "RUF001", #	ambiguous-unicode-character-string	String contains ambiguous {}. Did you mean {}? Может на русскую "с" в комменте ругаться 
    "RUF003", #	ambiguous-unicode-character-comment
    "SLF001", # private-member-access	Private member accessed: {access} | учитывая, что в Django постоянно дергаем _Meta моделей и поля в духе _prefetched_objects_cache и т.д.
    "FA100", # future-rewritable-type-annotation	Missing from __future__ import annotations, but uses {name}	
    "FA102", # future-required-type-annotation	Missing from __future__ import annotations, but uses
    "DJ001", # django-nullable-model-string-field	Avoid using null=True on string-based fields such as	
    "PGH004", # Use specific rule codes when using `noqa`
    "TRY003", # Avoid specifying long messages outside the exception class
    "EM102", # Exception must not use an f-string literal, assign to variable first
    "RUF002", # Docstring contains ambiguous `у` (CYRILLIC SMALL LETTER U). Did you mean `y` (LATIN SMALL LETTER Y)?
    "FBT001", # Boolean-typed positional argument in function definition
    "FBT002", # Boolean default positional argument in function definition
    "ANN204", # Missing return type annotation for special method `__init__`
]

[tool.ruff.lint.flake8-annotations]
mypy-init-return = true # ANN204 | checkers often allow you to omit the return type annotation for __init__ methods, as long as at least one argument has a type annotation

[tool.ruff.lint.isort]
force-wrap-aliases = true
combine-as-imports = true
```

    
</details>


### Bug Description

`DJ012` checking doesnt work properly. If a model with incorrect order of attributes and methods inherits from a Django model-based class, located in a separate file, the error is not raised.

### Details

<details>
    <summary>Proper behavior</summary>

With this code, ruff works correctly: the error is raised:
```
from django.db import models


class MyModel(models.Model):
    name = models.CharField(
        max_length=500,
        verbose_name="Название",
        default="",
    )

    def __str__(self) -> str:
        return "self.name"

    class Meta:
        verbose_name = "Модель"
        verbose_name_plural = "Модели"
```

Ruff output
```
[N] (venv) valera@t-t14 ~/P/a/s/backend (NATNP-3701)> ruff check src/my_subsystem/models/my_model.py
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
src/my_subsystem/models/my_model.py:14:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
   |
12 |           return "self.name"
13 |
14 |       class Meta:
   |  _____^
15 | |         verbose_name = "Модель"
16 | |         verbose_name_plural = "Модели"
   | |___________________________________^ DJ012
   |

Found 1 error.
```

</details>

<details>
    <summary>Another correct behavior</summary>

Here i added `ParentModel`. Now `MyModel` inherits from it. The error is still present. Everything works correctly.

```
from django.db import models


class ParentModel(models.Model):
    text = models.CharField(max_length=100)

    class Meta:
        abstract = True

    def __str__(self) -> str:
        return super().__str__()


class MyModel(ParentModel):
    name = models.CharField(
        max_length=500,
        verbose_name="Название",
        default="",
    )

    def __str__(self) -> str:
        return "self.name"

    class Meta:
        verbose_name = "Модель"
        verbose_name_plural = "Модели"
```

Ruff-output
```
[N] (venv) valera@t-t14 ~/P/a/s/backend (NATNP-3701) [0|1]> ruff check src/my_subsystem/models/my_model.py
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
src/my_subsystem/models/my_model.py:24:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
   |
22 |           return "self.name"
23 |
24 |       class Meta:
   |  _____^
25 | |         verbose_name = "Модель"
26 | |         verbose_name_plural = "Модели"
   | |___________________________________^ DJ012
   |

Found 1 error.
[I] (venv) valera@t-t14 ~/P/a/s/backend (NATNP-3701) [0|1]>
```

</details>


<details>
    <summary>Bug</summary>

The only change made was moving ParentModel to a separate file (in the same directory as my_model.py). After this change, the error disappeared.

parent_model.py
```
from django.db import models
from django.db.models import Model


class ParentModel(Model):
    text = models.CharField(max_length=100)

    class Meta:
        abstract = True

    def __str__(self) -> str:
        return super().__str__()

```

my_model.py
```
from django.db import models

from my_subsystem.models.parent import ParentModel


class MyModel(ParentModel):
    name = models.CharField(
        max_length=500,
        verbose_name="Название",
        default="",
    )

    def __str__(self) -> str:
        return "self.name"

    class Meta:
        verbose_name = "Модель"
        verbose_name_plural = "Модели"

```

Ruff output
```
[N] (venv) valera@t-t14 ~/P/a/s/backend (NATNP-3701) [0|1]> ruff check src/my_subsystem/models/my_model.py
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
All checks passed!
```

</details>

---

_Label `bug` added by @MichaReiser on 2024-08-13 16:04_

---

_Label `multifile-analysis` added by @MichaReiser on 2024-08-13 16:04_

---

_Comment by @charliermarsh on 2024-08-29 20:12_

👍 This is known and one of our multifile limitations right now.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---

_Comment by @ulgens on 2025-08-05 11:28_

@charliermarsh Hey, can I ask more about why this is related to multifile limitation? I have this class:

```python
class MyModel(models.Model):
    ...

    def __str__(self):
        return ...

    class Meta:
        verbose_name = "..."
```

DJ012 detects the issue successfully. If I replace the base class, it fails to find the issue, even though there is nothing related to the base class itself - the issue is right there on the class, contained in a single file.


---

_Comment by @MichaReiser on 2025-08-05 12:03_

@ulgens 

Can you share a full code example because I'm unable to reproduce the issue in the playground; https://play.ruff.rs/bdf4614f-9c68-4943-947f-05a1cf2a16f2

---

_Comment by @ulgens on 2025-08-05 12:11_

@MichaReiser https://play.ruff.rs/6c3fa123-7dea-4830-a465-da719b39ef37

I hope it demonstrates one aspect of the issue. Even though `BaseModel` is defined in the same file, ruff fails to detect DJ012. 

In my case, `BaseModel` is defined in a different file but my point was that there is no need to check that other file. DJ012 doesn't even make sense in multifile context.

---

_Comment by @MichaReiser on 2025-08-06 06:24_

Thanks. Yes, that helps to understand the issue. Ruff doesn't perform any alias analysis for this rule (analysing if a variable `x` points to `BaseModel`). That's why it isn't picked up. Ruff supports this in very limited way for some rules but it isn't very consistent about it. The solution here is, as well, a more powerful analysis engine which we are building in ty and plan to make use of in Ruff in the future.

Can you tell me more about why you're aliasing `BaseModel`?

---

_Comment by @ulgens on 2025-08-06 06:38_

@MichaReiser This is the `BaseModel`: https://github.com/ulgens/django-blasphemy/blob/50805be4727e88aebbabc4c521604dcee232a9ab/src/core/db/models.py#L35 I'm not actually aliasing it, it's a  base class that provides common functionalities to all models in the project. 

> Ruff doesn't perform any alias analysis for this rule

I'm a bit lost about this. What I expect here is that any rule defined for a model class should check all classes that inherit from `django.db.models.Model` and not care about how they are defined.

---

_Comment by @MichaReiser on 2025-08-06 06:42_

You're aliasing `BaseModel` here

```py
BaseModel = models.Model
```

The rule works as expected if you use

```py
class MyModel(models.Model):
    """..."""

    def __str__(self) -> str:
        return "string"

    class Meta:
        verbose_name = "..."

```

> I'm a bit lost about this. What I expect here is that any rule defined for a model class should check all classes that inherit from django.db.models.Model and not care about how they are defined.

I agree that this is the ideal and we're working towards it. It's just not as "easy" to detect all those cases statically

---

_Comment by @ulgens on 2025-08-06 07:00_

> You're aliasing BaseModel here

That was just a simplified case to demonstrate that the issue is not related to multifile setup, but I still think it shouldn't be relevant.

> It's just not as "easy" to detect all those cases statically

I think this is not a "there are many cases that we should find but we can't because it's hard" case. I think the bug here is much simpler (not saying it's easier to solve). Let me try to demonstrate the issue with Python:

Given

```python
from django.db.models import Model
```

The current implementation checks

```python
Model in MyModel.__bases__
```

which is not true if a class is not defined as `MyModel(models.Model)`

The correct way of checking if a class is a Django model should be 

```python
import inspect
Model in inspect.getmro(MyModel)
```

To summarize, the issue here is not 

> DJ012 is not raised when a parent class, that inherits from django.Model, is in a different file.

It's that DJ rules can't properly detect a model class.

---
