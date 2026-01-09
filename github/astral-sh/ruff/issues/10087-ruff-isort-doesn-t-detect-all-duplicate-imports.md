---
number: 10087
title: "ruff / isort doesn't detect all duplicate imports"
type: issue
state: open
author: CharlesPerrotMinot
labels:
  - isort
assignees: []
created_at: 2024-02-22T23:40:30Z
updated_at: 2024-05-13T05:45:33Z
url: https://github.com/astral-sh/ruff/issues/10087
synced_at: 2026-01-07T13:12:15-06:00
---

# ruff / isort doesn't detect all duplicate imports

---

_Issue opened by @CharlesPerrotMinot on 2024-02-22 23:40_

Hi,

We do use `combine-as-imports` for the lint.isort settings.
However, the following passes ruff check despite the typing being imported twice:

```
import typing
from typing import Any

def test_function(my_input: Any) -> typing.Any:
  return my_input
```

My expectation would be for ruff to catch this as a duplicate import, and force the import simplification.

---

_Comment by @MichaReiser on 2024-02-23 07:58_

> My expectation would be for ruff to catch this as a duplicate import, and force the import simplification.

I can see how unifying the imports is desired but it imposes a few challenges: 

The first is that it depends on personal preference on how the final code should look like. Should `typing.Any` be replaced with `Any` or the other way round where `Any` is replaced with `typing.Any`? What if there are multiple named imports and references to `typing`? What if there are no named imports for some usages of `typing`? I suspect that this heavily depends on personal preference and some users might not want that `typing` gets removed altogether. 

That's why I think this is something that's out of scope of the `isort` rule and remains something that requires manual intervention. There's also the technical challenge that it requires rewriting all usages in the file

---

_Label `isort` added by @MichaReiser on 2024-02-23 07:58_

---

_Comment by @yoann9344 on 2024-03-04 08:23_

There are also situations with as import.
```python
Import typing as t
Import typing as plop
...
plop.Any, t.Any
```

Maybe creating temporary errors, like TEMP, to throw the error without breaking the isort autofix. Until a more complexe décision to be taken.

---

_Comment by @CharlesPerrotMinot on 2024-05-13 05:44_

> There are also situations with as import.
> 
> ```python
> Import typing as t
> Import typing as plop
> ...
> plop.Any, t.Any
> ```
> 
> Maybe creating temporary errors, like TEMP, to throw the error without breaking the isort autofix. Until a more complexe décision to be taken.

Hum... to me this should not be authorized. Dual import doesn't make much sense in my opinion

---

_Comment by @CharlesPerrotMinot on 2024-05-13 05:45_

> > My expectation would be for ruff to catch this as a duplicate import, and force the import simplification.
> 
> I can see how unifying the imports is desired but it imposes a few challenges:
> 
> The first is that it depends on personal preference on how the final code should look like. Should `typing.Any` be replaced with `Any` or the other way round where `Any` is replaced with `typing.Any`? What if there are multiple named imports and references to `typing`? What if there are no named imports for some usages of `typing`? I suspect that this heavily depends on personal preference and some users might not want that `typing` gets removed altogether.
> 
> That's why I think this is something that's out of scope of the `isort` rule and remains something that requires manual intervention. There's also the technical challenge that it requires rewriting all usages in the file

Auto-fixing it would clearly be quite hard, and as you've said, probably out of scope. But at least raise an error?

---
