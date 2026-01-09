---
number: 8700
title: "Distinguish between F821 \"undefined name\" for undefined variables VS undefined string types"
type: issue
state: open
author: BenQuigley
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-11-15T17:55:00Z
updated_at: 2025-10-25T21:12:25Z
url: https://github.com/astral-sh/ruff/issues/8700
synced_at: 2026-01-07T13:12:15-06:00
---

# Distinguish between F821 "undefined name" for undefined variables VS undefined string types

---

_Issue opened by @BenQuigley on 2023-11-15 17:55_

I'm running into a similar issue as https://github.com/astral-sh/ruff/issues/7175 and I see that the user who raised the issue and maintainers ended up agreeing that ruff should complain about, e.g. `def foo(bar: "Bar")` if `Bar` is not defined as a type. That makes sense to me, but my team and I would disable that particular error from ruff (which is error `F821`). We use Python type annotations mostly the same way we use code comments; that is, we view them as helpful but we don't programmatically check them, a system that works well for us.

The problem is that running ruff with `ignore=[F821]` against our code base would also disable checking for undefined variables, which are important to check for.

Example script:

```python
def document_function(doc: "Document"):  # type annotation crimes, but valid python
    pass

Document  # NameError!!
```

ruff output:
```shell
ruff test.py
test.py:1:24: F821 Undefined name `Foo`
test.py:4:1: F821 Undefined name `Foo`
Found 2 errors.
```

I would like to be able to update my `pyproject.toml` to ignore the undefined type annotation, but not the programming error in line 4; so I would like to request for the "undefined forward type annotation" error to be broken out into a distinct error code.

---

_Label `needs-decision` added by @zanieb on 2024-01-10 21:22_

---

_Label `rule` added by @zanieb on 2024-01-10 21:22_

---

_Comment by @BenQuigley on 2024-02-19 15:37_

Updated the example to make a little more sense :)

---

_Comment by @zayenzl on 2024-06-17 10:45_

I've seen this same problem, and I think it would be _very_ valuable to separate these two very different cases. Especially as [PEP 484](https://peps.python.org/pep-0484/#forward-references) explicitly says that strings can be used for forward references
> When a type hint contains names that have not been defined yet, that definition may be expressed as a string literal, to be resolved later.

---

_Comment by @HWiese1980 on 2025-02-12 07:40_

Yeah, forward referencing is a common means to mitigate circular imports and valid Python. Ruff should consider this and allow it. 

---

_Comment by @AntiSol on 2025-05-13 03:56_

It's not just circular imports, there are also other perfectly valid cases where a type might not be resolvable - that's precisely why we have the ability to place them in strings as a forward reference. 

For example, it can also be an efficiency thing - e.g I don't actually need to import all of numpy, slowing down my program's startup, just so that my configuration class can specify that a particular value is a numpy array. 

Instead I should be able to use a forward reference to signify that the type isn't resolveable - the intended use of forward references - and have ruff respect that.

This should absolutely be split out into a separate rule. I am currently in a situation where my options are:
1. Remove typing information from a couple of vars, making them harder to comprehend for everybody
2. Import numpy for a script that does not need it, slowing down script startup.
3. Disable F821, losing the advantages that rule brings when it's not spitting out false positives

The _entire point_ of forward referencing is that I'm saying to the interpreter "you won't be able to resolve this name yet, so don't try". For ruff to then complain that it can't resolve that name is incorrect and broken on a very fundamental level.


---

_Referenced in [21cmfast/21cmFAST#532](../../21cmfast/21cmFAST/pulls/532.md) on 2025-07-19 09:05_

---

_Comment by @mikedh on 2025-10-25 21:12_

Thanks @BenQuigley  for raising this again with the further detail! I still have not found a good way to add type hints without adding circular imports everywhere, and I really think that forward references should be supported in more cases. Imports in Python are often expensive, and adding imports "just for the hint" seems to me to be the tail wagging the dog. 

I guess what I think the best behavior would be here is: "fully defined forward references are always allowed, i.e. `"trimesh.Trimesh"` or `"scipy.spatial.cKDTree"` can be used anywhere regardless of import status. I've just been disabling the checks, i.e. `"trimesh.Trimesh":  # type: ignore[name-defined]` but would love a better solution.


---

_Referenced in [astral-sh/ruff#10451](../../astral-sh/ruff/issues/10451.md) on 2025-12-01 17:55_

---
