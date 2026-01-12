```yaml
number: 8164
title: "Improve message for 'true-false-comparison' (E712)"
type: issue
state: closed
author: nikacvet
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-10-24T12:19:31Z
updated_at: 2024-03-06T17:54:31Z
url: https://github.com/astral-sh/ruff/issues/8164
synced_at: 2026-01-12T15:54:47Z
```

# Improve message for 'true-false-comparison' (E712)

---

_@nikacvet_

Using Ruff version `0.1.1`, for code like this: `if a == True` ruff returns the following error message:
`E712 Comparison to 'True' should be 'cond is True' or 'if cond'`

On the page describing the error, [E712](https://docs.astral.sh/ruff/rules/true-false-comparison/), it states that in PEP8 it's recommended to compare using ```if cond is True```. 

While that applies for singletons such as None, later in the [PEP8](https://peps.python.org/pep-0008/#programming-recommendations), it's stated that for True/False the comparison should be like this:
![image](https://github.com/astral-sh/ruff/assets/120791574/3afe0f2b-c65d-4644-abba-11228cad874c)

Using Pylint, the error message looks like this and is more clear on what's meant:
`Comparison 'a ==True' should be 'a is True' if checking for the singleton value True, or 'a' if testing for truthiness.`



---

_Comment by @zanieb on 2023-10-24 13:50_

This feels a little verbose for that message and I feel like many users will not understand the difference between the singleton value and truthiness without reading documentation. Ideally the documentation covers these cases with additional detail? 

---

_Label `documentation` added by @zanieb on 2023-10-24 13:51_

---

_Comment by @nikacvet on 2023-10-27 08:41_

I do agree that users should read the documentation, which does cover these cases in detail. However, if this error occurs, it's most likely that the user who wrote it didn't really read the documentation *(because if they would, why would they use comparison to True/False with `==` in the first place?)*. In such case one might resort to the [error E217 description page](https://docs.astral.sh/ruff/rules/true-false-comparison/), which describes that it's stated in PEP8 that rather than `if cond == True`, `if cond is True` should be used. However, that's not really true, as the PEP8 specifically states that comparison to True/False should use `if cond` and not `if cond is True`. Therefore, the documentation on error E217 is wrong and not according to PEP8. 

![image](https://github.com/astral-sh/ruff/assets/120791574/d56491a7-80b3-411d-8da1-c7fced786155)

Regarding verbosity, users who don't understand this difference (and haven't read the documentation) might make the assumption that one or the other makes no difference in code and is merely of stylish nature. 
As stated [here](https://docs.quantifiedcode.com/python-anti-patterns/readability/comparison_to_true.html)
> If the type of the condition is Boolean, it is obvious that comparing to True is redundant. But in Python, every non-empty value is treated as true in context of condition checking. 

Most clear difference can be seen with comparison to False, if `if not cond` is used, the following values are interpreted as false: False, None, numeric zero of all types, and empty strings and containers (including strings, tuples, lists, dictionaries, sets and frozensets). 
But if `if cond is False` is used, the only value interpreted as false is False. 

Besides, PEP8 labels it as worse than using `==`, therefore I think at least the description on the page of the error should be extended.

Here's a similar discussion: https://github.com/pylint-dev/pylint/issues/2527

---

_Comment by @NeilGirdhar on 2023-11-06 13:29_

I completely agree with this issue and came here to file a similar one.  Here's a long [discussion about this exact topic](https://github.com/networkx/networkx/issues/3995).

Generally, `if x == True` should be replaced with `if x`, and `if x is True` should be replaced with `if isinstance(x, bool) and x` or `match x: case True:`.  Please don't encourage anyone to write `if x is True`.   If anything there should be a rule preventing it!  Maybe we should add that?

---

_Comment by @tjkuson on 2023-11-06 14:04_

> I completely agree with this issue and came here to file a similar one. Here's a long [discussion about this exact topic](https://github.com/networkx/networkx/issues/3995).
> 
> Generally, `if x == True` should be replaced with `if x`, and `if x is True` should be replaced with `if isinstance(x, bool) and x` or `match x: case True:`. Please don't encourage anyone to write `if x is True`. If anything there should be a rule preventing it! Maybe we should add that?

I followed the thread you linked, and I must admit that I don't understand, in the cases where you want to check in an object is the singleton `True`, why `isinstance(foo, bool) and foo` is better than `foo is True`. It just seems more verbose.

---

_Comment by @NeilGirdhar on 2023-11-06 14:17_

> It just seems more verbose.

I think you have to look at the context in which this is usually done.  It might be something more like this:

```
def f(self, block_names: list[str] | bool) -> Generator[str, None, None]:
    for name in self.names:
        if isinstance(block_names, bool):
              if block_names:
                     print("blocked", name)
              else:
                     yield name
       else:
              if name not in block_names:
                     yield name
```
Maybe not the prettiest example (should probably take things common here), but the general idea is that switching on types is a totally separate thing from switching on values.  Usually you switch on type because a parameter is a union of types each of which has different behavior.  The switch then chooses between the behaviors.

After you switch on types, then you can separately switch on values.  Separating these things conceptually makes code easier to understand.

Usually when people try to do these things together, they either only intended to switch on values (so they should use truthiness or a comparison for non-Boolean values), or their code is (in my opinion) unnecessarily complicated.

Also, it can be hard for type checkers to deduce types when you use `is True`:
```
def f(x: bool | str):
  if x is True:
     ...
  elif x is False:
    ...
  else:
     # x  is a string, but the type checker doesn't know that!
```
Switching on the type first makes it obvious to the reader and the type checker.

---

_Comment by @tjkuson on 2023-11-06 14:54_

Thanks! Whilst I better understand the motivation and utility of the pattern now, I think it might be too opinionated to affect a recommendation from a default rule.

I prefer `is True`/`is False` in the example you listed (and this is what I have seen in most codebases). `bool` is a special type because, AFAIK, there are only ever two instances of it, so comparing the identity directly is a more readable shorthand for me. (This is also why I wouldn't do `isinstance(foo, type(None)) and not foo` to check if `foo is None`). Moreover, I was unable to reproduce the type ambiguity;`mypy` inferred `x` to be `builtins.str` for me.

Regarding the message, might `Boolean comparison should be 'if cond' (or, if checking identity, 'cond is True')` be a good compromise? It nudges users to what they probably want to do whilst keeping the identity check recommendation if they intended to do that.

---

_Comment by @NeilGirdhar on 2023-11-06 14:59_

Ah, I see type checkers have advanced.  I still think that the general pattern of switching on types should be separated from switching on values.  When you do `if x is None`, you are switching on types—alone.  So that fits with that general pattern.  Do you have a motivating real-world example of `is True`?

---

_Comment by @zanieb on 2023-11-06 15:34_

An `isinstance` call is going to be much slower than an `is True` identity check e.g.

```
❯ python -m timeit 'x = "foo"; x is True'
50000000 loops, best of 5: 9.11 nsec per loop
❯ python -m timeit 'x = "foo"; isinstance(x, bool) and x'
10000000 loops, best of 5: 25.8 nsec per loop
```

(Interestingly, when `x = True` the timings are very similar. Perhaps there's an optimization for that.)

I also find it much less clear; this is my first time seeing that pattern. I do like your general idea of separating type and value comparisons though.

> Boolean comparison should be 'if cond' (or, if checking identity, 'cond is True')

I like this more than the first suggested message because it does not rely on the user understanding singletons.

Perhaps: To check for truthiness, use `if cond` (or, if checking for identity, `if cond is True`)

I also think it would be entirely reasonable to drop the section about checking for identity entirely from the rule message. I agree that checking for identity is rare and if you're using it you should know what you're doing. I don't see a strong justification for suggesting it in every violation.

In that case, perhaps: Instead of comparing directly to `True` use `if cond` for truth checks.


---

_Comment by @tjkuson on 2023-11-06 17:40_

> Ah, I see type checkers have advanced.

Yup! Thought I am running using a development built of mypy, so it might not work in the latest official release.

>  Do you have a motivating real-world example of is True?

There are a few occasions where a value being `True` or `False` should be treated differently to a positive integer or empty list, respectively. I don't think I am well-placed to determine how rare or common this is holistically, though!

---

_Comment by @nikacvet on 2023-11-08 14:01_

> In that case, perhaps: Instead of comparing directly to True use if cond for truth checks.

Thanks for this (: I agree that the first message I recommended would be somewhat confusing or complex for most. This message is much easier to read and is in accordance to PEP8. I also think it's reasonable to omit the message for checking identity, as it is usually really never the case that someone wants this behaviour.

---

_Comment by @zanieb on 2023-11-08 17:14_

Pull request welcome here :)

---

_Label `good first issue` added by @zanieb on 2023-11-08 17:14_

---

_Assigned to @tjkuson by @charliermarsh on 2023-11-25 17:50_

---

_Closed by @AlexWaygood on 2024-03-06 17:54_

---
