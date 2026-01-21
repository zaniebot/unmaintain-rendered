```yaml
number: 17386
title: False positive reports for F821 lint rule
type: issue
state: open
author: chenzhekl
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-04-14T05:31:41Z
updated_at: 2026-01-21T22:38:36Z
url: https://github.com/astral-sh/ruff/issues/17386
synced_at: 2026-01-21T23:08:13Z
```

# False positive reports for F821 lint rule

---

_@chenzhekl_

### Summary

When we use Ruff with jaxtyping, Ruff falsely report F821 error for the following type annotation:

```python
import torch
from jaxtyping import Float, jaxtyped
from beartype import beartype

@jaxtyped(typechecker=beartype)
def foo(a: Float[torch.Tensor, "dim"]) -> None:
    pass
```

```
Undefined name `dim` Ruff[F821](https://docs.astral.sh/ruff/rules/undefined-name)
```

This only happens when the tensor is of 1D.

### Version

ruff 0.11.2

---

_Comment by @Daverball on 2025-04-14 09:57_

This requires type inference/multi-file analysis. Ruff doesn't know that `jaxtyping.Float` is an alias for `typing.Annotated`, so `dim` will be treated as a forward reference.

The only way to address this in the short term would be to, as @MichaReiser previously suggested, add a new setting that contains a mapping for re-exports, so you can tell ruff that `jaxtyping.Float` is the same as `typing.Annotated`. This would be useful in a number of places to address the lack of multi-file analysis.

---

_Comment by @Daverball on 2025-04-14 10:04_

A (admittedly kind of silly) workaround in the meantime would be to instead write it like this:

```python
import torch
from jaxtyping import Float, jaxtyped
from beartype import beartype

dim = "dim"

@jaxtyped(typechecker=beartype)
def foo(a: Float[torch.Tensor, dim]) -> None:
    pass
```

---

_Label `bug` added by @ntBre on 2025-04-14 12:41_

---

_Label `type-inference` added by @ntBre on 2025-04-14 12:41_

---

_Comment by @ntBre on 2025-04-14 12:44_

Thanks for the report, and thanks @Daverball for the explanation.

I found #6014, which at least sounds related, but maybe not closely enough to call this a duplicate since it's for a different `typing` re-export.

---

_Comment by @Orionnss on 2025-05-23 09:43_

An awful workaround that seems to work, you can add a whitespace before the name of the dimension as in 
`labels: Int[torch.Tensor, " nb_sentences"],` and it seems to work.

If someone is able to explain why this works, I would be very happy :) 

---

_Comment by @Daverball on 2025-05-23 09:47_

> If someone is able to explain why this works, I would be very happy :)

It's possible the annotations parser fails or gets skipped if the annotation starts with a whitespace, although I'd consider that a bug, that should get fixed, it's still a valid expression with the whitespace.

---

_Comment by @Daverball on 2025-05-23 09:52_

Also #9860 is technically related, I only just saw this issue pop up as a duplicate recently. Maybe this issue should be closed in favor of that issue, although to be fair re-exporting heavily special cased type forms like `typing.Annotated` and `typing.Literal` under different names is a more general problem that isn't specific to jaxtyping, any library can do that and cause the same problems.

---

_Comment by @knyazer on 2025-06-01 19:58_

By the way, this is also an issue (or a very similar issue it seems) in the original pyflakes :) https://github.com/PyCQA/pyflakes/issues/789

Maybe as a workaround we could split F821 into two rules, F821a and F821, where F821a refers to annotations? So that this rule can be disabled on its own. WDYT?

Also, another rule that is somewhat related is F722: in jaxtyping, the annotations of kind `Float[Array, ""]` are common, which triggers this rule, for, I presume, the same reason of using `Annotated as`.

---

_Comment by @erlebach on 2025-06-11 14:29_

I'd like to know why this issue is so difficult to fix given the level of sophistication of so many other tools. 

---

_Comment by @patrick-kidger on 2025-09-12 15:50_

Have just come across this. As per the [jaxtyping FAQ](https://docs.kidger.site/jaxtyping/faq/#flake8-or-ruff-are-throwing-an-error) then adding a space at the start of the string -- `Float[Array, " foo"]` -- is my recommended work around. This converts the F821 error into an ignorable F722 error instead.

---

_Comment by @kalk-ak on 2026-01-21 22:38_

Came here because I was facing the same issue

---
