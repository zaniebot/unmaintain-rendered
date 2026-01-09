---
number: 4958
title: "[behavior-change] RUF010 delete unnecessary `str()`"
type: issue
state: closed
author: smackesey
labels:
  - rule
assignees: []
created_at: 2023-06-08T12:32:12Z
updated_at: 2023-06-26T21:29:31Z
url: https://github.com/astral-sh/ruff/issues/4958
synced_at: 2026-01-07T13:12:15-06:00
---

# [behavior-change] RUF010 delete unnecessary `str()`

---

_Issue opened by @smackesey on 2023-06-08 12:32_

IIUC, these two expressions are exactly equivalent:

```
f"{str(x)}"  # (1)
f"{x}"  # (2)
```

The `str` is superfluous in (1). But instead of RUF010 removing the `str` call, instead it autofixes to `f"{x!s}"`-- but the `!s` here is still superfluous-- the only time it makes sense to use `!s` is if there is an additional format specifier, e.g. `f"{x!s:20}"` (in this case `str` is applied _before_ the format specifier, whereas without `!s` it is applied after the format specifier).

So IMO RUF010 should remove `str()` unless there is a format specifier, in which case it converts to `!s`.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-08 17:41_

---

_Label `rule` added by @charliermarsh on 2023-06-08 17:41_

---

_Comment by @dhruvmanila on 2023-06-08 18:44_

@charliermarsh I can take this up unless you've already started working on it :)

---

_Comment by @charliermarsh on 2023-06-08 18:44_

It’s ok, I’ve already started on it :)

---

_Referenced in [astral-sh/ruff#4971](../../astral-sh/ruff/pulls/4971.md) on 2023-06-08 19:54_

---

_Closed by @charliermarsh on 2023-06-08 20:03_

---

_Comment by @mishamsk on 2023-06-20 20:36_

@charliermarsh @smackesey I believe this is wrong, and thus #4971 should be reverted. See below:
```python
class Test:
    def __str__(self) -> str:
        return "str"
    
    def __format__(self, __format_spec: str) -> str:
        return "format"
    
print(f"{Test()} {str(Test())} {Test()!s} {Test()!s:6} {Test():6}")
# gives: format       str         str         str         format
```

so, i.e. even the stdlib's StrEnum will not work correctly after such an auto "fix"

---

_Comment by @charliermarsh on 2023-06-20 20:42_

(Taking a look.)

---

_Comment by @charliermarsh on 2023-06-20 20:46_

Can you include an example to illustrate the behavior you're describing with `StrEnum`? In my testing, including or omitting `!s` yields the same result in that case.

---

_Comment by @mishamsk on 2023-06-20 21:22_

> Can you include an example to illustrate the behavior you're describing with `StrEnum`? In my testing, including or omitting `!s` yields the same result in that case.

ah, sorry, my bad. I was using my "polyfill" in 3.10 which apparently doesn't match 3.11 stdlib exactly. Anyways, the `{value}` calls `value.format()`, so the check and fix that was implemented here is not correct.

Here is another example with enums in 3.11:
```python
from enum import StrEnum, Enum


class Test(StrEnum):
    VALUE = "value"


class NonStrEnum(str, Enum):
    VALUE = "value"

    def __str__(self) -> str:
        return str(self.value)

    def __format__(self, format_spec: str) -> str:
        return repr(self)


print(f"{Test.VALUE} {Test.VALUE!s}")  # prints "value value"
print(f"{NonStrEnum.VALUE} {NonStrEnum.VALUE!s}")  # prints "<NonStrEnum.VALUE: 'value'> value"

```

---

_Comment by @charliermarsh on 2023-06-20 21:31_

All good. Just to be totally clear, I'm not trying to "disprove" the claim here, I think you're right that these aren't equivalent (it looks like f-strings by default are calling `__format__`, and we're assuming they're calling `__str__`), I'm just trying to assess the blast radius :)

---

_Comment by @mishamsk on 2023-06-20 21:43_

sure, no problem. Sorry for confusion with StrEnum, it just happened to be something I was (re)implementing in one of my commercial projects in 3.10 that is impacted. Can't think of an easy way to estimate the impact here, probably not a lot of custom format functions out there. But my gut tells me that introducing silent non-100%-compatible changes is not what users of Ruff expect. Since Ruff (at least originally) was supposed to lint, not "optimize" code, at least I expect it to retain everything (just like black), without any possible side-effects.

Since Ruff is so fast and I bet 80%+ just have auto-fix for most of the rules, most won't even notice such a small "benign" change, which may hit at some point if custom formatting is applied... but ultimately it's your call. 

I personally would have to make a mental notice to either wait for a release that reverts this or remember to disable auto-fix for this rule...

---

_Comment by @charliermarsh on 2023-06-20 23:12_

A couple quick comments (a little tight on time but want to keep this convo going, sorry for brevity):

- Python is so dynamic that it's hard for us to implement almost _any_ fixes with complete, 100% certainty. Even reordering imports is not a completely "safe" operation, but I think most users would expect Ruff to support import sorting. So we have to draw various lines around what we're comfortable fixing and what we're not.
- One way we're trying to improve this is by introducing autofix levels: automatic, suggested, and manual. Automatic would always be fixed, suggested would require `--fix-unsafe` (instead of `--fix`), and manual would never be autofixed. We've actually added these annotations to almost all of the fixes, but they're not yet respected on the CLI. (So, at the very least, using `--fix` would "guarantee" that we make no behavioral changes, or at least are very confident in the fixes.)
- In practice this seems really rare...
- But I actually agree that we should revert it, mostly because it will be hard to detect/notice when this _does_ do the wrong thing (unlike, e.g., removing an unused import that's actually used in some capacity). Here, it's pretty likely that nothing would break at parse-time or at run-time or even in tests, which feels dangerous to me.

---

_Comment by @mishamsk on 2023-06-21 00:20_

no worries, that’s actually a lot of great details, appreciate you taking the time.
* you’re totally right rgd dynamicity. Haven’t even thought about import order, but now as you’ve said it, I even know specific examples where order matters (basically with any packages that mingle with import machinery). 
* classifying fixes is an awesome approach. Hopefully it gets into a release sooner rather then later
* In terms of impact and/or how to classify this a fix: something that may go unnoticed and at the same time is not a lot of value (like this issue) makes more sense under `—fix-unsafe`. Things like import sort that (a) not enabled by default in Ruff (b) probably will create an immediate failure in tests or downstream (c) add a lot of value (e.g. stable git diffs) - may be considered as regular `—fix`.
* For this specific case - another justification here. I suspect if someone did add `!s` or `str()` inside f-expression, it may have been done because of unexpected `__format__` behavior. Most devs probably do not even think/know about `!s\r\a` modifiers, so wouldn’t use them unless they stumbled upon an issue. 

---

_Referenced in [astral-sh/ruff#5232](../../astral-sh/ruff/pulls/5232.md) on 2023-06-21 00:54_

---

_Comment by @smackesey on 2023-06-26 18:00_

Nice catch @mishamsk, I wasn't even aware `__format__` existed.

---

_Comment by @mishamsk on 2023-06-26 21:29_

@smackesey thanks! 

---
