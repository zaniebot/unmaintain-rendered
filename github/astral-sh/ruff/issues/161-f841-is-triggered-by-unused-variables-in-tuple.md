---
number: 161
title: F841 is triggered by unused variables in tuple unpacking (unlike flake8)
type: issue
state: closed
author: Jackenmen
labels: []
assignees: []
created_at: 2022-09-12T00:17:37Z
updated_at: 2022-09-17T01:46:30Z
url: https://github.com/astral-sh/ruff/issues/161
synced_at: 2026-01-07T13:12:14-06:00
---

# F841 is triggered by unused variables in tuple unpacking (unlike flake8)

---

_Issue opened by @Jackenmen on 2022-09-12 00:17_

Unlike flake8, ruff adds F841 error when an unused variable is in tuple unpacking. Due to this being an incredibly common pattern, it would be good if it didn't work like this. In the code base I tested this on, it has caused over 50 errors and I don't think that replacing meaningful names with `_` improves the readability of the code.

There can be rare cases where this could indicate spots where code could be simplified, for example `dict.items()` usage when using only key or only value, *or* `enumerate()` usage when not actually using the index *but* that's at the cost of detecting a lot more of non-issues.
Maybe if #134 gets implemented to allow usage of something like `_var_name` to indicate an unused variable, it could still be helpful while not requiring the developer to lower the readability of the code (though still annoying them a bit by requiring them to rename their variable :smile:) *but* if so, I think it would be better suited for a separate check, rather than a check named after a rule in flake8 that doesn't act like this (I'm aware that ruff doesn't need to produce strictly equivalent output but this is a quite significant difference in behavior).

Here's a bunch of examples based on flake8's unit tests (comments say how flake8 behaves):
```py
def f(tup):
    x, y = tup  # this does NOT trigger F841


def f():
    x, y = 1, 2  # this triggers F841 as it's just a simple assignment where unpacking isn't needed


def f():
    (x, y) = coords = 1, 2  # this does NOT trigger F841
    if x > 1:
        print(coords)


def f():
    (x, y) = coords = 1, 2  # this triggers F841 on coords


def f():
    coords = (x, y) = 1, 2  # this triggers F841 on coords
```

---

_Comment by @Jackenmen on 2022-09-12 00:30_

Not entirely the same but similar story for `for` loops:
```py
def f():
    for i in range(10):  # this does NOT trigger F841 with flake8
        pass
```

---

_Comment by @charliermarsh on 2022-09-12 00:30_

Interesting, thank you, I'll have to look at how they do this 🤔 

---

_Comment by @Jackenmen on 2022-09-12 00:38_

Hopefully my somewhat mass-creating of issues isn't discouraging. It's just that this project looks cool and I liked the idea of running flake8 in CI and ruff in pre-commit from #120 to see differences while this project is maturing. So I'm just putting ruff through the entire codebase and seeing what fails :smile: 

---

_Comment by @charliermarsh on 2022-09-12 00:43_

Not at all! I know ruff isn't ready for production yet so I appreciate anyone that's willing to try it out and file actionable issues (just like you've been doing).

---

_Comment by @charliermarsh on 2022-09-12 00:46_

Ah, ok, I see how they handle it. It shouldn't be too difficult to match their behavior.

---

_Label `enhancement` added by @charliermarsh on 2022-09-12 00:46_

---

_Referenced in [astral-sh/ruff#163](../../astral-sh/ruff/pulls/163.md) on 2022-09-12 01:50_

---

_Comment by @charliermarsh on 2022-09-12 01:50_

#163 fixes the for-loop and common unpacking cases. It doesn't handle repeated assignments like `(x, y) = coords = 1, 2` but I think that's rarer?

---

_Closed by @charliermarsh on 2022-09-12 01:53_

---

_Comment by @Jackenmen on 2022-09-12 02:56_

Works great! Now I'm down to 8 errors all caused by #150 so I might need to start trying ruff out on other code bases :smile: 

> It doesn't handle repeated assignments like `(x, y) = coords = 1, 2` but I think that's rarer?

Probably, yeah. I didn't see that in the code base I ran it on, I took that particular example from pyflakes's unit test.

---

_Referenced in [astral-sh/ruff#217](../../astral-sh/ruff/pulls/217.md) on 2022-09-17 01:45_

---

_Comment by @andersk on 2022-09-17 01:46_

> It doesn't handle repeated assignments like `(x, y) = coords = 1, 2` but I think that's rarer?

I hit this in the wild: https://github.com/zulip/zulip/blob/7efd59b6d7d7f8b98c69086fa7cda7a2e0179091/zerver/lib/markdown/__init__.py#L614

I agree it’s probably rare, but here’s a fix: #217.

---

_Referenced in [astral-sh/ruff#890](../../astral-sh/ruff/issues/890.md) on 2022-11-23 14:46_

---
