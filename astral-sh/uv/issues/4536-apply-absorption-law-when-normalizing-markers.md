```yaml
number: 4536
title: Apply Absorption Law when normalizing markers
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-06-26T01:29:10Z
updated_at: 2024-07-08T18:59:22Z
url: https://github.com/astral-sh/uv/issues/4536
synced_at: 2026-01-12T15:58:50Z
```

# Apply Absorption Law when normalizing markers

---

_@charliermarsh_

E.g., `python_version < '3.11' or (python_version < '3.11' and implementation_name == 'cpython')` should be reduced to `python_version < '3.11'`.

---

_Label `enhancement` added by @charliermarsh on 2024-06-26 01:29_

---

_Comment by @charliermarsh on 2024-06-26 01:32_

I wonder if we should do something like this: https://docs.rs/boolean_expression/latest/boolean_expression/enum.Expr.html#method.simplify_via_bdd

---

_Comment by @charliermarsh on 2024-06-26 01:36_

Alternatively, maybe we can just apply this in the `or` and `and` methods (make them smart constructors)?

---

_Comment by @BurntSushi on 2024-06-26 02:02_

I think smart constructors are the way to go. To make smart constructors work, you have to make sure they are the only things capable of constructing the internal representation. You also need to make all normalization steps inductive. On top of this, I think the normalization logic (which would need to be ported to smart constructors in `pep508_rs`) currently lives in `uv-resolver` because it relies on a `pubgrub` type. So that dependency on `pubgrub` would probably need to be broken.

I would love to do this personally as I love smart constructors and they've worked out great when I used them in the past (like in `regex-syntax`). But it's a meaty task I think.

---

_Comment by @ibraheemdev on 2024-06-26 04:02_

I'm not sure the smart constructor helps with the simplification. For example, the expression `((x and y) or z)` is fully simplified, but combining that with `or y` we get `((x and y) or z) or y`, which simplifies to `y or z`. Importantly, the complete simplification cannot be performed inductively, so a SAT solver ([such as this](https://docs.sympy.org/latest/modules/logic.html#sympy.logic.boolalg.to_dnf)) would need to be run after the entire marker tree has been constructed.

I think it would be helpful to know what kind of expressions the lockfile generates to see how far we need to take this. We can solve this issue using our current recursive approach and see whether more complex cases come up. It's totally possible that an inductive approach that only looks one layer deep is enough for us, but I would hesitate to add smart constructors and them end up not being useful because we need to do a recursive solve anyways.

---

_Comment by @notatallshaw-gts on 2024-07-01 17:23_

FYI I came across this today with the following example:

```
$ echo "pylint" | uv pip compile - --annotation-style line --python-version 3.10 --universal 2>/dev/null | grep dill
dill==0.3.8 ; python_version < '3.11' or python_version >= '3.11'  # via pylint
```

---

_Comment by @charliermarsh on 2024-07-01 17:34_

Confusingly there are Python versions that do not match either of those bounds. For example, `3.11.0a0` would not match either.

---

_Comment by @notatallshaw-gts on 2024-07-01 19:46_

> Confusingly there are Python versions that do not match either of those bounds. For example, `3.11.0a0` would not match either.

I don't feel like that's a correct interpretation of Python version numbers.

When you are installing into a Python installation there is only one specific version number available for that Python. So if you are installing into Python 3.11.0a0 then all versions of Python available (i.e. the one you are installing into) are pre-releases and therefore `python_version>= 3.11` matches `3.11.0a0`.

But that's just me thinking out loud, when I get a moment I'll check what pip does and read through the spec.

---

_Comment by @charliermarsh on 2024-07-01 19:48_

I think per the spec it's correct. I actually agree that we should probably simplify that, but I think that's why it doesn't get simplified today.

`python_version>= 3.11` is implicitly `python_version>= 3.11.0`, and `3.11.0a0` is not `>= 3.11.0`, right?


---

_Comment by @notatallshaw-gts on 2024-07-01 19:56_

> and `3.11.0a0` is not `>= 3.11.0`, right

My understanding is per PEP 440 `3.11.0a0` matches `>= 3.11.0`, when `3.11.0a0` is the only version available because then pre-releases are implied. So my logic was that when you are installing into Python it's version is the only version available, and therefore you can always take pre-releases to be applied.

But, this was just all off the top of my head, I should of really waited to get home and test it out. I will do so later.

---

_Comment by @notatallshaw on 2024-07-01 23:14_

Got home and ran some tests and read some specs and I still think uv isn't correct here, I created a seperate issue: https://github.com/astral-sh/uv/issues/4714

---

_Comment by @charliermarsh on 2024-07-02 02:31_

I filed a separate issue for that normalization here: https://github.com/astral-sh/uv/issues/4719. (It's different than what's described in this issue.)

---

_Closed by @ibraheemdev on 2024-07-08 18:59_

---
