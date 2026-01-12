```yaml
number: 14796
title: "RUF052: do not apply to function arguments?"
type: issue
state: closed
author: inducer
labels:
  - rule
  - preview
assignees: []
created_at: 2024-12-05T21:07:26Z
updated_at: 2024-12-06T14:54:44Z
url: https://github.com/astral-sh/ruff/issues/14796
synced_at: 2026-01-12T15:54:54Z
```

# RUF052: do not apply to function arguments?

---

_@inducer_

I feel that the connotation of a leading underscore is quite different between local variables and function arguments. I agree with the premise of the rule that an underscored local variable is an unused dummy thing. But an underscored function arguments to me is an internal thing, comparable to an underscored method in a class, not to be considered part of the public interface. As such, I would prefer if the rule could separate function argument names from other local variables.

---

_Comment by @dylwil3 on 2024-12-05 21:26_

Incidentally, adopting this point of view would also solve #14790 

So that makes it tempting to agree - but perhaps for selfish reasons. Curious what other folks think!

---

_Label `rule` added by @dylwil3 on 2024-12-05 21:27_

---

_Comment by @zanieb on 2024-12-05 21:29_

> But an underscored function arguments to me is an internal thing, comparable to an underscored method in a class, not to be considered part of the public interface. As such, I would prefer if the rule could separate function argument names from other local variables.

So... would this be [`unused-function-argument`](https://docs.astral.sh/ruff/rules/unused-function-argument/) instead?

---

_Comment by @dylwil3 on 2024-12-05 21:35_

It sounds like the OP is saying that an underscored argument should be indicating that it is "private" rather than "unused". So, for example:

```python
def foo(x:int, y:str, _z:bool = False):
# <-- do stuff -->
if _z:
    #<-- something... -->
#<--- more stuff --->
```
I _think_ (but I could be wrong) that the OP is suggesting that the connotation of `_z` is "this is a private, internal setting I can use" as opposed to "this is a dummy variable". In particular, the above code snippet should trigger neither [used-dummy-variable (RUF052)](https://docs.astral.sh/ruff/rules/used-dummy-variable/#used-dummy-variable-ruf052) nor [unused-function-argument (ARG001)](https://docs.astral.sh/ruff/rules/unused-function-argument/#unused-function-argument-arg001)

---

_Comment by @zanieb on 2024-12-05 21:45_

Ah yes, of course — yeah I agree it's weird to raise this (RUF052) for function arguments.

My point is if that if we're not going to treat it as a dummy variable then if it's _unused_ we should raise a violation

```
❯ cat example.py
def foo(x: int, y: str, _z: bool = False):
    return 10

❯ uvx ruff@latest check example.py --select ARG001
Installed 1 package in 1ms
example.py:1:9: ARG001 Unused function argument: `x`
  |
1 | def foo(x: int, y: str, _z: bool = False):
  |         ^ ARG001
2 |     return 10
  |

example.py:1:17: ARG001 Unused function argument: `y`
  |
1 | def foo(x: int, y: str, _z: bool = False):
  |                 ^ ARG001
2 |     return 10
  |

Found 2 errors.
```

Note we don't flag `_z` here and the documentation says

> If a variable is intentionally defined-but-not-used, it should be prefixed with an underscore, or some other value that adheres to the [lint.dummy-variable-rgx](https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx) pattern.

---

_Comment by @dylwil3 on 2024-12-05 21:56_

Yeah that's a little awkward... it kinda seems like both interpretations of the underscore could make sense in different contexts. I would personally find it okay to ignore both checks in the presence of underscores.

I guess I don't have a good sense of how many people would be happy to receive a lint error (either of them) alerting to the case where they did or didn't use a function argument that was named with an underscore. Like - what is the prevalence of people _accidentally_ putting an underscore on a function argument and wishing they hadn't? 

---

_Comment by @zanieb on 2024-12-05 22:00_

I don't really know of a context where you'd define an intentionally unused variable in a function signature. Matching a parent signature comes to mind, but... you can't add an underscore to resolve that because it changes the signature.

---

_Comment by @dylwil3 on 2024-12-05 22:15_

I think you can change the name for position-only arguments.

But I agree I don't know who's doing this...

```python
class Foo:
    def bar(self, x:int, /) -> int:
        return x

class Bar(Foo):
    def bar(self, _x:int, /) -> int:
        return 17
```

---

_Comment by @zanieb on 2024-12-05 22:37_

Yeah I guess so haha getting increasingly dubious though.

This can be a separate debate, but I guess I find it weird that we would not apply `RUF052` because it's not a dummy argument but... we also wouldn't apply `ARG001` because it is a dummy argument.

---

_Label `preview` added by @dylwil3 on 2024-12-05 23:06_

---

_Closed by @AlexWaygood on 2024-12-06 14:54_

---
