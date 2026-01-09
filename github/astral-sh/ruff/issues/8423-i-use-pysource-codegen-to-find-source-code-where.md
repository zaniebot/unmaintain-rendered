---
number: 8423
title: I use pysource-codegen to find source code where black and ruff differ in the formatting
type: issue
state: closed
author: 15r10nk
labels:
  - formatter
assignees: []
created_at: 2023-11-01T21:48:51Z
updated_at: 2024-01-29T14:06:02Z
url: https://github.com/astral-sh/ruff/issues/8423
synced_at: 2026-01-07T13:12:15-06:00
---

# I use pysource-codegen to find source code where black and ruff differ in the formatting

---

_Issue opened by @15r10nk on 2023-11-01 21:48_


hi, I wrote [pysource-codegen](https://github.com/15r10nk/pysource-codegen) which can be used to generate random python code and I thought that it could be a good idea to use it to compare ruff with black to find source code which gets formatted differently.

here is the first of many examples which I found:

test seed  0
minimize
ruff and black are formatting the following code differently:
``` python
async def name_4[**name_0](name_3: {[name_2], name_4, name_0 >= name_0, name_3, name_2}): # type: ignore
    pass
```

black:

``` python
async def name_4[**name_0](name_3: {[name_2], name_4, name_0 >= name_0, name_3, name_2}):  # type: ignore
    pass

```

ruff:

``` python
async def name_4[**name_0](
    name_3: {[name_2], name_4, name_0 >= name_0, name_3, name_2}
):  # type: ignore
    pass

```

diff:

``` diff
--- black

+++ ruff

@@ -1,2 +1,4 @@

-async def name_4[**name_0](name_3: {[name_2], name_4, name_0 >= name_0, name_3, name_2}):  # type: ignore
+async def name_4[**name_0](
+    name_3: {[name_2], name_4, name_0 >= name_0, name_3, name_2}
+):  # type: ignore
     pass
```
ruff version: ruff 0.1.3

black version: black, 23.10.1 (compiled: no)
Python (CPython) 3.12.0

There are many of other examples where ruff and black format differently (different seeds). It is hard for me to decide which formatter formats correctly. I hope it is useful for you to improve ruff format further.

here is the script which I used:

``` python
from pysource_codegen import generate
from pysource_minimize import minimize


import subprocess as sp
from difflib import context_diff, unified_diff


def contains_bug(code, diff=False):
    """
    returns True if the code triggers a bug and False otherwise
    """

    black_result = sp.run(
        ["black", "-q", "-"], input=code.encode(), capture_output=True
    )
    black_formatted = black_result.stdout.decode()

    ruff_result = sp.run(
        ["ruff", "format", "--isolated", "-q", "-"],
        input=code.encode(),
        capture_output=True,
    )
    ruff_formatted = ruff_result.stdout.decode()

    if diff:
        print()
        print("black:\n")
        print("``` python")
        print(black_formatted)
        print("```")

        print()
        print("ruff:\n")
        print("``` python")
        print(ruff_formatted)
        print("```")

        print()
        print("diff:\n")
        print("``` diff")
        print(
            "\n".join(
                unified_diff(
                    black_formatted.splitlines(),
                    ruff_formatted.splitlines(),
                    "black",
                    "ruff",
                )
            )
        )
        print("```")

    return ruff_formatted != black_formatted


def find_issue():
    for seed in range(10000):
        print("test seed ", seed)
        code = generate(seed)

        if contains_bug(code):
            print("minimize")
            new_code = minimize(code, contains_bug)

            print("ruff and black are formatting the following code differently:")
            print("``` python")
            print(new_code)
            print("```")

            contains_bug(new_code, diff=True)

            result = sp.run(["ruff", "--version"], capture_output=True)
            print("ruff version:", result.stdout.decode())

            result = sp.run(["black", "--version"], capture_output=True)
            print("black version:", result.stdout.decode())

            return


find_issue()
```
requirements:
```
pysource-codegen
pysource-minimize
ruff
black
```






---

_Label `formatter` added by @charliermarsh on 2023-11-02 03:23_

---

_Comment by @charliermarsh on 2024-01-29 14:06_

Sorry for the late response -- realizing I forgot to comment here back when it was filed, but thank you for doing this! I'm going to close to keep the issue tracker actionable, but I appreciate your help.

---

_Closed by @charliermarsh on 2024-01-29 14:06_

---
