---
number: 4830
title: Check for decorators
type: issue
state: closed
author: Bendabir
labels: []
assignees: []
created_at: 2023-06-03T10:46:39Z
updated_at: 2023-06-03T14:13:02Z
url: https://github.com/astral-sh/ruff/issues/4830
synced_at: 2026-01-07T13:12:14-06:00
---

# Check for decorators

---

_Issue opened by @Bendabir on 2023-06-03 10:46_

Hi,
I was wondering if Ruff would be able to check if a function, a method or a class is actually decorated by a given decorator. PyLint does support such thing through custom checkers and plugins (not native, but possible). Can we do the same with Ruff 🤔 ?

The first use case I see is for deprecations. To the best of my knowledge, there's no deprecation mecanism built-in with Python, so it requires to use some extra stuff (such as [deprecated](https://pypi.org/project/Deprecated/)) to decorate deprecated functions/methods. One downside of these approaches is that they only trigger (some warnings) at runtime. I could be nice to spot these deprecations ahead.

Thanks for your time 😄 

PS : I couldn't find any issue/question related to this, so feel free to redirect me if I missed something !

---

_Comment by @JonathanPlasse on 2023-06-03 12:10_

Yes, the decorators of functions and classes are available in the AST.

---

_Comment by @Bendabir on 2023-06-03 12:58_

Okay, nice. I'll have a look. Thanks !

---

_Closed by @Bendabir on 2023-06-03 12:58_

---

_Comment by @Bendabir on 2023-06-03 14:13_

Hmmm, I checked what's available in the AST and I'm not sure that what I had in mind is feasible. I can indeed access the decorators when declaring the function. For example :

```python
@deprecated(reason = "Will be removed.", version = "1.0.0")
def some_function():
    ...
```

However, when using the function itself, it's basically a call statement where I don't have access to much information.

```python
some_function()
```

As I would like to check for decorated functions at call time, I would need to get the decorators information  and I'm not sure that's possible.

---
