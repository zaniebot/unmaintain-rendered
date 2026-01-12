```yaml
number: 13664
title: "DJ008: Should it also work for tolomea/django-auto-prefetch?"
type: issue
state: open
author: TheLovinator1
labels:
  - configuration
assignees: []
created_at: 2024-10-07T15:50:20Z
updated_at: 2024-10-07T15:57:01Z
url: https://github.com/astral-sh/ruff/issues/13664
synced_at: 2026-01-12T15:54:53Z
```

# DJ008: Should it also work for tolomea/django-auto-prefetch?

---

_@TheLovinator1_

# [DJ008](https://docs.astral.sh/ruff/rules/django-model-without-dunder-str/) - Django model without \_\_str\_\_ method

[django-auto-prefetch](https://github.com/tolomea/django-auto-prefetch) uses `auto_prefetch.Model` instead of `models.Model` so the rule does not trigger. Should this be changed or is there a way to make it work with `auto_prefetch.Model` via pyproject.toml?

## What the rule does

```text
Checks that a __str__ method is defined in Django models.
```

## Example

```python
from django.db import models


class Book(models.Model): # Will trigger DJ008
    author = models.ForeignKey("Author", on_delete=models.CASCADE)

    class Meta:
        verbose_name = "Book"
```

```python
import auto_prefetch
from django.db import models


class Book(auto_prefetch.Model): # Will not trigger DJ008
    author = auto_prefetch.ForeignKey("Author", on_delete=models.CASCADE)

    class Meta(auto_prefetch.Model.Meta):
        verbose_name = "Book"
```

Keywords: django-auto-prefetch, DJ008 and django-model-without-dunder-str.

Thanks, TheLovinator.


---

_Comment by @MichaReiser on 2024-10-07 15:56_

Yeah, our Django rules are very limited at the moment until we finish migrating Ruff to a multifile-capable analysis (which we're working on). The underlying issue is the same as for https://github.com/astral-sh/ruff/issues/12865: Ruff isn't capable of reasoning about imports from other modules. 

A setting could be a short-term workaround that I'm open considering. 

---

_Label `configuration` added by @MichaReiser on 2024-10-07 15:57_

---
