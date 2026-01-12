```yaml
number: 14377
title: Looks like ruff ignores useless return statement in functions annotated to return not None
type: issue
state: open
author: go-to-mjrecent-on-TG
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-16T11:25:52Z
updated_at: 2024-11-17T19:46:26Z
url: https://github.com/astral-sh/ruff/issues/14377
synced_at: 2026-01-12T15:54:53Z
```

# Looks like ruff ignores useless return statement in functions annotated to return not None

---

_@go-to-mjrecent-on-TG_

Today, I went to [Ruff playground](https://play.ruff.rs), enabled [PLR1711 (useless-return) rule](https://docs.astral.sh/ruff/rules/useless-return/)  and pasted this code:

```py
def f() -> int:
    print()
    return None
```

In Diagnostics tab I saw only `Everything is looking good!`.
I then removed ` -> int` for the code to be:

```py
def f():
    print()
    return None
```

and saw ```Useless `return` statement at end of function (PLR1711) [Ln 3, Col 5] ```.
Ruff complained in the same way when I annotated function to return None:

```py
def f() -> None:
    print()
    return None
```

All the same goes for cases with `return` instead of `return None`. 
Is this an intended behavior? If it is, I think [the documentation on this rule](https://docs.astral.sh/ruff/rules/useless-return/) should inform about this.

I have checked how pylint handles such cases by running cell with the following in fresh google colab runtime:

```
!pip install pylint>/dev/null
!printf 'def f() -> int:\n    print()\n    return None' > a.py
!pylint --version && pylint --disable C a.py
```

I got

```
pylint 3.3.1
astroid 3.3.5
Python 3.10.12 (main, Sep 11 2024, 15:47:36) [GCC 11.4.0]
************* Module a
a.py:1:0: R1711: Useless return at end of function or method (useless-return)

-----------------------------------
Your code has been rated at 6.67/10
```

How can I get ruff to complain about useless return statements in cases when function is not annotated to return None?
You can also help [on StackOverflow](https://stackoverflow.com/questions/79195035/how-to-make-ruff-complain-about-useless-return-statements-in-functions-annotated)

---

_Comment by @dylwil3 on 2024-11-16 16:21_

Thanks for bringing this up!

It appears the decision to skip the lint when the return annotation is present and not `None` was made [in the resolution of this issue](https://github.com/astral-sh/ruff/issues/3704), for compatibility with `mypy`.

We should certainly update the documentation to reflect this. But if there is also some interest from the community in revisiting this decision, or perhaps even making it configurable, it'd be nice to hear more about what's most helpful here!

---

_Comment by @MichaReiser on 2024-11-16 16:45_

Ruff's current behavior seems correct to me. The return isn't useless. It's just that the returned value doesn't match the annotated type. But that's more a type checker concern than a concern of the useless return rule.

---

_Label `question` added by @MichaReiser on 2024-11-16 16:45_

---

_Label `rule` added by @MichaReiser on 2024-11-16 16:45_

---

_Comment by @go-to-mjrecent-on-TG on 2024-11-16 17:49_

> Thanks for bringing this up!
> 
> It appears the decision to skip the lint when the return annotation is present and not `None` was made [in the resolution of this issue](https://github.com/astral-sh/ruff/issues/3704), for compatibility with `mypy`.
> 
> We should certainly update the documentation to reflect this. But if there is also some interest from the community in revisiting this decision, or perhaps even making it configurable, it'd be nice to hear more about what's most helpful here!

Is there currently any way of making ruff complain about such returns? If not, I would be happy if that was made possible. So revise the decision or add the other rule for this, please.

Thank you for ruff!

---

_Comment by @go-to-mjrecent-on-TG on 2024-11-16 18:04_

> Ruff's current behavior seems correct to me. The return isn't useless. It's just that the returned value doesn't match the annotated type. But that's more a type checker concern than a concern of the useless return rule.

How are such returns not useless? What do you mean?
So you think that this should be written in docs:


> [What it does](https://docs.astral.sh/ruff/rules/useless-return/#what-it-does)
> 
> Checks for functions that end with an unnecessary return or return None, and contain no other return statements.
> [Why is this bad?](https://docs.astral.sh/ruff/rules/useless-return/#why-is-this-bad)
> 
> Python implicitly assumes a None return at the end of a function, making it unnecessary to explicitly write return None.
> [Example](https://docs.astral.sh/ruff/rules/useless-return/#example)
> 
> ```py
> def f():
>     print(5)
>     return None
> ```
> Use instead:
> 
> ```py
> def f():
>     print(5)
> ```
> 
> 

But ruff should not flag this?:

```py
def f() -> ...:
    print()
    return None
```

---

_Comment by @MichaReiser on 2024-11-16 21:30_

I don't mind extending the documentation.

> How are such returns not useless? What do you mean?

The return in the given example is incorrect: It returns `None` instead of a number. But it isn't that the return is useless. Instead, the user should either:

* Fix the return type annotation and change it to `None` or remove it entirely. Ruff then flags the return statement
* Fix the return statement to return an int.

To my knowledge, there's currently no rule to catch this because catching that the returned value `None` is incomaptible (not assignable to) with `int` is something that type checkers do. We plan to add support for a rule that catches the incorrect return with our type checker red knot, but that's still far out. 


---

_Comment by @dylwil3 on 2024-11-16 21:41_

I don't know, I'm not sure if I'm fully on board with this interpretation. From my point of view, there are two separate things happening:

1. The use of `return None` or `return` vs the _equivalent_ implicit return of None.
2. The question of whether a returned value (implicit or explicit) has compatible type with the return annotation.

I think (1) is a question of style and (2) is a question for a type checker. It makes sense to me that one would want to enforce (1) uniformly in a project as a lint rule (really either way - to always have explicit returns or always implicit returns of None - just a style preference), and then separately have a type checker complain about the values being returned from your function and what type hint you used.

Sorry for being difficult! I'm fine leaving it as is but just wanted to share this perspective.



---

_Comment by @MichaReiser on 2024-11-16 21:50_

That interpretation makes sense to me. But to me, showing both PLR1711 and a violation that the returned value doesn't match the annotated type is confusing for users using a type checker. It's provided fix is may also be incorrect, depending on the situations. That's why I think it's better to not raise the violation at all.

---

_Comment by @go-to-mjrecent-on-TG on 2024-11-17 14:17_

So I would answer yes to the question whether it should there should be an option to make ruff complain about such returns:

```py
def f() -> {not None}:
    ...
    return None
```
Because it might very well be that somebody wants this code to be valid:

```py
# returns some int
def f() -> int: ...

exec('redefine f. example: def f(): return 0')
r = f()
assert isinstance(r, int)
print(r)
```
but get complains about the same code with:
```py
def f() -> int:
    ...
    return None
```
or with
```py
def f() -> int:
    """docs in the signature function, description..."""
    return None
```
Actually, ruff doesnt complain even about the following now ([so does pylint](https://stackoverflow.com/questions/79196716/pylint-and-ruff-dont-complain-about-useless-unnecessary-return-in-functions-w)): 
```py
def f():
    """docs in the signature function, description..."""
    return None
```
I myself didn't know that functions with docstring and without return at the end are valid. And I can imagine how I could have ended up not knowing about it and working with some other developers who know that this is valid, use ruff, share the ruff config with their teammates, and want ruff to tell newbies that they are fine with:
```py
def f():
    """docs in the signature function, description..."""
```





---

_Comment by @dylwil3 on 2024-11-17 18:53_

I think there is an underlying assumption in applying static program analysis to a highly dynamic language like Python that behavior involving `exec`/`eval` can't really be detected. (But someone should correct me if I'm wrong). Basically the best that can be done in these cases is detect the _use_ of `exec`/`eval` and warn against it: [S102](https://docs.astral.sh/ruff/rules/exec-builtin/), [S307](https://docs.astral.sh/ruff/rules/suspicious-eval-usage/)

---

_Comment by @go-to-mjrecent-on-TG on 2024-11-17 19:04_

> I think there is an underlying assumption in applying static program analysis to a highly dynamic language like Python that behavior involving `exec`/`eval` can't really be detected. (But someone should correct me if I'm wrong). Basically the best that can be done in these cases is detect the _use_ of `exec`/`eval` and warn against it: [S102](https://docs.astral.sh/ruff/rules/exec-builtin/), [S307](https://docs.astral.sh/ruff/rules/suspicious-eval-usage/)

What do I do if I want to allow exec and eval, and allow the definition of function signatures, but also get warnings when there is some return None that is not needed there?

---

_Comment by @dylwil3 on 2024-11-17 19:09_

Yes I think that's a fair question (and orthogonal to the use of `eval`/`exec`). That's I think the core remaining issue here - deciding whether to not support this, support it as a configurable variant of the rule, or support it as a separate rule. Might take some further discussion and consideration. Thank you for the examples and clarifications!

---

_Label `question` removed by @MichaReiser on 2024-11-17 19:19_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-17 19:19_

---

_Comment by @go-to-mjrecent-on-TG on 2024-11-17 19:46_

Examples of 'real world' code that contains useless return, but that ruff doesn't complain about (with abstractmethod, not exec or eval):

```py
from typing import Any
from abc import ABC, abstractmethod


class DataProcessor(ABC):
    @abstractmethod
    def process(self) -> Any:
        ...
        return None
```

```py
from abc import ABC, abstractmethod


class DataProcessor(ABC):
    @abstractmethod
    def process(self):
        """processes the data"""
        return None
```

---
