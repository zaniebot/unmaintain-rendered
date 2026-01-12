```yaml
number: 2586
title: Implemented flake8-django plugin rules
type: pull_request
state: merged
author: konysko
labels:
  - plugin
assignees: []
merged: true
base: main
head: flake8-django
created_at: 2023-02-05T18:40:42Z
updated_at: 2023-02-14T10:47:27Z
url: https://github.com/astral-sh/ruff/pull/2586
synced_at: 2026-01-12T04:52:00Z
```

# Implemented flake8-django plugin rules

---

_Pull request opened by @konysko on 2023-02-05 18:40_

Hello,

I started porting rules from [flake8-django](https://github.com/rocioar/flake8-django). I will add rest of the rules in the following prs.

List of implemented plugin rules:
- [X] DJ01
- [ ] DJ03
- [ ] DJ06
- [ ] DJ07
- [X] DJ08
- [ ] DJ12
- [X] DJ13


---

_Converted to draft by @konysko on 2023-02-05 18:48_

---

_Marked ready for review by @konysko on 2023-02-05 19:08_

---

_Label `plugin` added by @charliermarsh on 2023-02-05 21:34_

---

_@charliermarsh reviewed on 2023-02-05 21:35_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:769 on 2023-02-05 21:35_

We'd generally write this as: `self.diagnostics.extend(flake8_django::rules::ModelStringFieldNullable::check(bases, body))`.

---

_@charliermarsh reviewed on 2023-02-05 21:36_

---

_Review comment by @charliermarsh on `src/rules/flake8_django/rules.rs`:22 on 2023-02-05 21:36_

The convention in the codebase is to write these as top-level functions with the same name as the violation, rather than associated methods on the violation, e.g.:

```rust
pub fn model_dunder_str(bases: &[Expr], ...) -> Option<Diagnostic> {
  ...
}
```

---

_@charliermarsh reviewed on 2023-02-05 21:36_

---

_Review comment by @charliermarsh on `src/rules/flake8_django/rules.rs`:17 on 2023-02-05 21:36_

We tend to put raw code (like `__str__` in backticks within the rule messages).

---

_@charliermarsh reviewed on 2023-02-05 21:38_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_django/DJ13.py`:1 on 2023-02-05 21:38_

It'd be preferable, I think, to copy over the existing test fixtures directly (https://github.com/rocioar/flake8-django/blob/master/tests/fixtures/model_dunder_str.py). That way, we can test for 1:1 compatibility with the existing plugin.

---

_@charliermarsh reviewed on 2023-02-05 21:44_

---

_Review comment by @charliermarsh on `src/rules/flake8_django/rules.rs`:209 on 2023-02-05 21:44_

We should use `Checker` for this to ensure that `Model` is actually `django.models.Model` or whatever.

If you thread `checker: Checker` through here, you can do:

```rust
checker.resolve_call_path(&base).map_or(false, |call_path| call_path.as_slice() == ["django", "Model"])
```

That will ensure that we support things like:

```py
from django import models

class Foo(models.Model):
  ...

from django.models import Model as Moo

class Foo(Moo):
  ...
```

...and so on. _And_ that we avoid flagging `Model` references that _aren't_ to `django.models.Model`.


---

_@charliermarsh reviewed on 2023-02-05 21:47_

---

_Review comment by @charliermarsh on `src/rules/flake8_django/rules.rs`:63 on 2023-02-05 21:47_

Can you include a comment or docstring to indicate an example of the Python code that this is trying to catch? E.g., I initially assumed this would be checking to see if class is an `abc.ABC` or similar.

---

_@charliermarsh reviewed on 2023-02-10 00:13_

---

_Review comment by @charliermarsh on `src/rules/flake8_django/rules.rs`:209 on 2023-02-10 00:13_

Just checking in -- are you able to address these refactors? Or do you want a hand?

---

_@konysko reviewed on 2023-02-10 14:12_

---

_Review comment by @konysko on `src/rules/flake8_django/rules.rs`:209 on 2023-02-10 14:12_

I didn't have time to fix those, i'll do it durning the weekend. Also good catch with resolving whole path to check whether it's the django model.

---

_Review comment by @charliermarsh on `src/rules/flake8_django/rules.rs`:209 on 2023-02-10 14:27_

Ok no worries! Was just trying to keep track of whether you planned to look into these changes or not.

---

_@charliermarsh reviewed on 2023-02-10 14:27_

---

_Comment by @konysko on 2023-02-11 16:20_

@charliermarsh i applied your suggestions, added fixtures from the original package and split rules into separate files.

---

_Comment by @charliermarsh on 2023-02-12 17:00_

Tracking remaining work in #2817.

---

_Review comment by @charliermarsh on `README.md`:1087 on 2023-02-12 17:47_

Controversially, I renamed these to add a leading zero. It deviates from upstream, but it's much more consistent with the other plugins.

---

_@charliermarsh reviewed on 2023-02-12 17:47_

---

_Merged by @charliermarsh on 2023-02-12 17:47_

---

_Closed by @charliermarsh on 2023-02-12 17:47_

---

_Comment by @mounirmesselmeni on 2023-02-13 22:36_

With version 0.0.246, the DJ001 is not raised when defining a model with a nullable CharField

```python
tax_number = models.CharField(max_length=13, null=True)
```

While the DJ013 works

---

_Comment by @charliermarsh on 2023-02-14 00:10_

@mounirmesselmeni - Do you mind posting the full code snippet? (If relevant: you have to import `models` for Ruff to flag that, e.g., `from django.db import models` or however you like.)

---

_Comment by @mounirmesselmeni on 2023-02-14 08:02_

@charliermarsh The import is there but the model is extending from another base model class btw

Example:
```python
from django.db import models
from django.utils.translation import gettext_lazy as _
from utils.models import BaseModel


class ExampleModel(BaseModel):
    name = models.CharField(max_length=255, verbose_name=_('Name'), null=True)
    description = models.TextField(verbose_name=_('Description'))

    def __str__(self) -> str:
        return self.name

```

I have these two checks selected:
`"DJ001", "DJ008"`

---

_Comment by @konysko on 2023-02-14 10:47_

@mounirmesselmeni I created [issue](https://github.com/charliermarsh/ruff/issues/2892) for this

---
