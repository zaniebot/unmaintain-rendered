```yaml
number: 4941
title: "[bug] TCH004 false positive with function-scoped imports"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2023-06-07T21:09:22Z
updated_at: 2023-06-07T22:22:23Z
url: https://github.com/astral-sh/ruff/issues/4941
synced_at: 2026-01-10T11:09:47Z
```

# [bug] TCH004 false positive with function-scoped imports

---

_Issue opened by @smackesey on 2023-06-07 21:09_

I was unable to create a simple reproduction of this, but TCH004 on ruff 0.0.271 gives several false positives in Dagster when there are function-local imports. Running ruff on https://github.com/dagster-io/dagster/blame/master/python_modules/dagster/dagster/_core/definitions/op_definition.py will give this error:

```
python_modules/dagster/dagster/_core/definitions/op_definition.py:53:42: TCH004 [*] Move import `.decorators.op_decorator.DecoratedOpFunction` out of type-checking block. Import is used for more than type hinting.
```

But this isn't the case-- all the places where `DecoratedOpFunction` is used outside of type annotations, there is a function-local import covering it. When ruff autofixes this it pulls the import out of the `TYPE_CHECKING` block which then causes a circular import error.

---

_Comment by @charliermarsh on 2023-06-07 21:12_

Thanks, that's a helpful example.

---

_Label `bug` added by @charliermarsh on 2023-06-07 21:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-07 21:20_

---

_Comment by @charliermarsh on 2023-06-07 22:22_

Oh, hah, this was also fixed by #4942.

---

_Closed by @charliermarsh on 2023-06-07 22:22_

---
