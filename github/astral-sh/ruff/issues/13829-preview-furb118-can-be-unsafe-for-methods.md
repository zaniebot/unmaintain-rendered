---
number: 13829
title: "[preview] FURB118 can be unsafe for methods"
type: issue
state: closed
author: henryiii
labels:
  - bug
  - preview
assignees: []
created_at: 2024-10-20T06:05:18Z
updated_at: 2024-11-28T12:58:24Z
url: https://github.com/astral-sh/ruff/issues/13829
synced_at: 2026-01-07T13:12:16-06:00
---

# [preview] FURB118 can be unsafe for methods

---

_Issue opened by @henryiii on 2024-10-20 06:05_

If you run with `preview` set, then the FURB118 check makes an unsafe replacement, you can see me rolling back the change https://github.com/wntrblm/nox/pull/870/commits/a91dbe2356a42bf8d2f03021ccd3b19a903484c8 to make the tests pass, the difference in behavior can be shown with this MWE:

```pycon
>>> class A:
...     pass
...  
>>> A.__eq__ = lambda a, b: a == b
>>> A() == A()
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    A() == A()
  File "<python-input-4>", line 1, in <lambda>
    A.__eq__ = lambda a, b: a == b
                            ^^^^^^
  File "<python-input-4>", line 1, in <lambda>
    A.__eq__ = lambda a, b: a == b
                            ^^^^^^
  File "<python-input-4>", line 1, in <lambda>
    A.__eq__ = lambda a, b: a == b
                            ^^^^^^
  [Previous line repeated 988 more times]
RecursionError: maximum recursion depth exceeded
>>> import operator
>>> A.__eq__ = operator.eq
>>> A() == A()
Traceback (most recent call last):
  File "<python-input-8>", line 1, in <module>
    A() == A()
TypeError: eq expected 2 arguments, got 1
```

---

_Label `preview` added by @AlexWaygood on 2024-10-20 13:32_

---

_Comment by @AlexWaygood on 2024-10-20 13:55_

Thanks for opening the issue. FURB118 has been [something of a problematic rule](https://github.com/astral-sh/ruff/issues?q=is%3Aissue+furb118) for us in terms of the number of issues we've had regarding it :/

Here's a slightly smaller repro for the issue here, that makes it slightly clearer how the suggested change can break working code:

```pycon
>>> import operator
>>> class Spam:
...     foo = lambda self, other: self == other
... 
>>> class Eggs:
...     foo = operator.eq
... 
>>> Spam().foo(Spam())
False
>>> Eggs().foo(Eggs())
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: eq expected 2 arguments, got 1
```

It seems like the `operator` functions are always unsuitable for use as method definitions (whether monkeypatched or otherwise), because they don't have `__get__` methods in the same way that user-defined functions do. When you access the `foo` method on an instance of `Spam` in my REPL example, you don't actually get the `foo` function; behind the scenes, `foo.__get__` is called and you get back a `MethodType` instance; this is why you don't need to pass in `self` explicitly in most method calls. But when you access the `foo` method on an instance of `Eggs`, you get back the original `operator.eq` function instead of a `MethodType` instance, because `operator.eq` has no `__get__` method to call.

The next question is what we can do about this. I think there are a few things we can do:
- Document more clearly that this rule's autofix is always unsafe. It _is_ always categorised as an unsafe fix (you have to opt into it with `--unsafe-fixes`), but our [docs](https://docs.astral.sh/ruff/rules/reimplemented-operator/#fix-safety) currently emphasise that it's "usually safe". While I think it _is_ true that the fix is _usually_ safe, we don't actually explicitly state anywhere in our docs currently that it's always categorised as an unsafe fix; we probably should.
- We should list the missing `__get__` method on the `operator` functions as another reason _why_ the fix is unsafe in our docs
- We should avoid emitting the diagnostic at all for direct assignments in class bodies, like in my REPL example above

What I think we _can't_ do, unfortunately, is avoid emitting the diagnostic in dynamic cases where you're monkey-patching a method onto a class, like the original situation you ran into in your `nox` PR. Possibly we _might_ be able to do that with more type inference, but for now I think it's well beyond our capabilities to reliably detect whether the thing you're monkeypatching the function onto is a class object or not.

---

_Label `bug` added by @AlexWaygood on 2024-10-20 13:55_

---

_Comment by @henryiii on 2024-10-21 14:29_

I was under the impression it was a "safe" fix, but I reran it and it does only show up as an unsafe fix. (Aside: it would be nice if unsafe fixes were also indicated in the default printout, like `[**]` or something).

All three improvements sound good. Probably no rush as it's not out of preview yet.

---

_Comment by @AlexWaygood on 2024-10-21 14:38_

> (Aside: it would be nice if unsafe fixes were also indicated in the default printout, like `[**]` or something).

This might possibly be addressed by https://github.com/astral-sh/ruff/issues/7352, which is definitely something we want to do at some point!

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-11-21 17:29_

---

_Referenced in [astral-sh/ruff#14639](../../astral-sh/ruff/pulls/14639.md) on 2024-11-27 17:52_

---

_Closed by @AlexWaygood on 2024-11-28 12:58_

---
