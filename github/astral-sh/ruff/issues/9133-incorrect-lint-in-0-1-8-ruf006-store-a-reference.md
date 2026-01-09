---
number: 9133
title: "Incorrect lint in 0.1.8? RUF006 Store a reference to the return value of `asyncio.create_task`"
type: issue
state: closed
author: danking
labels:
  - bug
assignees: []
created_at: 2023-12-14T17:09:38Z
updated_at: 2023-12-20T17:12:28Z
url: https://github.com/astral-sh/ruff/issues/9133
synced_at: 2026-01-07T13:12:15-06:00
---

# Incorrect lint in 0.1.8? RUF006 Store a reference to the return value of `asyncio.create_task`

---

_Issue opened by @danking on 2023-12-14 17:09_

Hey there! A huge thank you for building ruff. It's been an invaluable tool for us over at https://github.com/hail-is/hail .

Am I being incredibly dense or should this rule not trigger on this code?

```bash
cat >pyproject.toml <<'EOF'
[tool.ruff]
select = ["F", "E", "W", "I", "PL", "RUF"]
EOF

cat >foo.py <<'EOF'
import asyncio

x = asyncio.create_task(asyncio.sleep(1))
EOF
ruff --version
python3 --version
python3 -m pip show ruff
which ruff
which python3
ruff foo.py
```
```
ruff 0.1.8
Python 3.10.9
Name: ruff
Version: 0.1.8
Summary: An extremely fast Python linter and code formatter, written in Rust.
Home-page: https://docs.astral.sh/ruff
Author: Charlie Marsh <charlie.r.marsh@gmail.com>
Author-email: "Astral Software Inc." <hey@astral.sh>
License: MIT
Location: /Users/dking/miniconda3/lib/python3.10/site-packages
Requires: 
Required-by: 
/Users/dking/miniconda3/bin/ruff
/Users/dking/miniconda3/bin/python3
foo.py:3:5: RUF006 Store a reference to the return value of `asyncio.create_task`
Found 1 error.
```


---

_Renamed from "Incorrect lint? RUF006 Store a reference to the return value of `asyncio.create_task`" to "Incorrect lint in 0.1.8? RUF006 Store a reference to the return value of `asyncio.create_task`" by @danking on 2023-12-14 17:09_

---

_Comment by @charliermarsh on 2023-12-14 17:34_

Thanks for the kind words!

You're right, we need to consider the control flow in this case. Marking as a bug, sorry for the disruption.

---

_Label `bug` added by @charliermarsh on 2023-12-14 17:34_

---

_Comment by @charliermarsh on 2023-12-15 01:05_

Do you have a real-world example from `hail` or another project where this triggered in a manner like the above? It's just hard to reason about the "correct" behavior so looking for help.

---

_Comment by @danking on 2023-12-15 03:05_

Ah, heh, I oversimplified 😉 . The exact code in question is [this](https://github.com/hail-is/hail/blob/main/hail/python/hailtop/aiotools/fs/copier.py#L504-L530).

The code is a bit opaque and could probably be written better but I think I see the issue. Ruff cannot prove that we always `asyncio.wait` or `await` the task because of a correlated if. To simplify, the following code always awaits the task but ruff can't prove that to itself. It even fails if the second if is `if x:`. 

```python3
import asyncio

async def foo(x: bool):
    if x:
        t = asyncio.create_task(asyncio.sleep(1))
    else:
        t = None
    try:
        await asyncio.sleep(1)  # in reality: do some concurrent work
    finally:
        if t:
            await t

```

OK! So, I now do not believe this is a bug in ruff. It's just inherent to ruff's incompleteness.

I *do* think there is a bug in the error message. Can we change it to:
```
RUF006 Cannot prove result of `asyncio.create_task` is awaited in all program traces.
```
That message would've given me enough clew to solve this on my own without bothering you :).

If you like the message: https://github.com/astral-sh/ruff/pull/9144; but I realize it violates the imperative phrasing you prefer in these messages.

---

_Referenced in [astral-sh/ruff#9144](../../astral-sh/ruff/pulls/9144.md) on 2023-12-15 03:21_

---

_Comment by @danking on 2023-12-15 03:30_

Hmm. Getting sucked into your codebase a bit! It seems like this is not the intention of `bindings.is_used()`, right? That ought to return true (i.e. references is non-empty) because the *binding* is, syntactically, referenced.

I can imagine a more sophisticated notion of "usage" that ties an allocation site to a use-site , but that doesn't seem to be the intention of Binding.

---

_Comment by @charliermarsh on 2023-12-15 03:48_

Thanks for the thorough follow-up :)

Ahh I see. So, it works if you remove the `else`: https://play.ruff.rs/00a2d9af-223c-4934-8be5-02860e66898e. The issue is that we don't do any sophisticated branch analysis yet, so when we see `t = None`, it shadows the `t = asyncio.create_task(asyncio.sleep(1))` binding, and so the `t = asyncio.create_task(asyncio.sleep(1))` binding appears to have no usages. We can solve this by looking ahead to see if any _shadowed_ bindings are used, since those are very likely to be fine usages.

---

_Comment by @danking on 2023-12-15 05:12_

Interesting! Hmm. I’ve not studied Python semantics closely but my instinct is that assignment in both branches of an if isn’t shadowing a la:
```
x = asyncio.create_task(…)
x = 5
```
I think of assignment in an if as suggesting that a “binding” can have multiple definition sites just as it can have multiple references. If you accept that, then this lint rule needs to evaluate: if at least one definition site is a task, does there exist at least one reference.

That seems to me different from the above example. I can imagine a very similar lint that ensures objects with a close method have their close method called. I can definitely imagine myself inadvertently shadowing it in the manner above and wanting that to be linted! 

---

_Comment by @charliermarsh on 2023-12-15 05:14_

That's correct -- it's not _actually_ shadowing, but our semantic model doesn't take branching into account in a meaningful way, so it treats the code _as if_ the `else` is shadowing the assignment in the `if`.

---

_Referenced in [hail-is/hail#14093](../../hail-is/hail/pulls/14093.md) on 2023-12-19 17:45_

---

_Comment by @yakMM on 2023-12-20 12:57_

Same false positive (I assume) with the following code:

```py
if some_condition:
    task = asyncio.create_task(coro)
else:
    task = asyncio.create_task(another_coro)
```

Workaround on my side:

```py

if not some_condition:
    coro = another_coro

task = asyncio.create_task(coro)
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-20 16:14_

---

_Referenced in [astral-sh/ruff#9215](../../astral-sh/ruff/pulls/9215.md) on 2023-12-20 16:57_

---

_Closed by @charliermarsh on 2023-12-20 17:07_

---

_Comment by @charliermarsh on 2023-12-20 17:12_

Both of these cases should be fixed in the next release.

---
