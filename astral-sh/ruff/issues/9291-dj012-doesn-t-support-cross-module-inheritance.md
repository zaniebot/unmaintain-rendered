```yaml
number: 9291
title: "DJ012 Doesn't Support Cross-Module Inheritance"
type: issue
state: closed
author: max-muoto
labels:
  - type-inference
assignees: []
created_at: 2023-12-27T04:43:37Z
updated_at: 2024-10-24T14:34:12Z
url: https://github.com/astral-sh/ruff/issues/9291
synced_at: 2026-01-12T15:54:49Z
```

# DJ012 Doesn't Support Cross-Module Inheritance

---

_@max-muoto_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


In a Django app, you might have some custom base model that all of your other models inherit from.

```python
class BaseModel(models.Model):
    default_field_1 = models.CharField(max_length=255, default="")
    default_field_2 = models.CharField(max_length=255, default="")

    class Meta:
        abstract = True

class ExampleModel(BaseModel):
    def __str__(self) -> str: # Ruff flags as this being incorrectly ordered.
        return super().__str__()

    class Meta:
        ...
```

However, if you split these across modules, Ruff will not flag incorrect ordering in `ExampleModel` with [DJ012](https://docs.astral.sh/ruff/rules/django-unordered-body-content-in-model/) enabled.

Command: `ruff check .`
Version `0.1.9`



---

_Comment by @charliermarsh on 2023-12-27 13:48_

Thanks! This is part of a much broader project to enable multi-file analysis, so we're just merging generic symptoms of that issue into https://github.com/astral-sh/ruff/issues/7447.

---

_Closed by @charliermarsh on 2023-12-27 13:48_

---

_Label `multifile-analysis` added by @charliermarsh on 2023-12-27 13:48_

---

_Comment by @max-muoto on 2023-12-28 06:59_

Sounds good, thanks!

---

_Comment by @mounirmesselmeni on 2024-02-13 17:24_

@charliermarsh I would suggest to update the rule docs about this limitation.

Only works if you don't use custom base model defined in another file or module.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:34_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---
