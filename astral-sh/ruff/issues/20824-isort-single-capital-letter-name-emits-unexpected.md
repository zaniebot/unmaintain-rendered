```yaml
number: 20824
title: "`isort` single-capital-letter name emits unexpected sorting diagnostic"
type: issue
state: closed
author: bzoracler
labels:
  - question
  - isort
assignees: []
created_at: 2025-10-12T22:11:27Z
updated_at: 2025-10-14T11:16:59Z
url: https://github.com/astral-sh/ruff/issues/20824
synced_at: 2026-01-12T15:54:57Z
```

# `isort` single-capital-letter name emits unexpected sorting diagnostic

---

_@bzoracler_

### Summary

Ruff says that the following is unsorted (see [playground](https://play.ruff.rs/7883baeb-e3fa-46c2-a2b7-92b96f5e8795)):

```python
from mod import A, AA, AAA  # Import block is un-sorted or un-formatted (I001) [Ln 1, Col 1]
# ruff wants this instead: `AA, AAA, A`
```

However, this is OK ([playground](https://play.ruff.rs/668e35cb-ef75-405f-ab6d-f39bcd307cba)):

```python
from mod import a, aa, aaa
```

### Version

0.14.0

---

_Label `isort` added by @MichaReiser on 2025-10-13 08:45_

---

_Comment by @MichaReiser on 2025-10-13 08:50_

Yeah, this is surprising. Took me a bit of debugging to understand what's happening. 

isort considers what the import member is. At least, it tries to guess the kind based on the naming schema:

* `A`: Is a class 
* `AA` and `AAA` are constants

Constants are sorted before classes; that's why you get `AA`, `AAA` (both constants), and then `A`, which is determined to be a class. There could be an argument that `A` should be considered a constant too, but I think it's just ambiguous and hard to tell by only looking at the name.

So in that sense, the behavior is expected, but certainly non obvious.

In your case, is `A` a constant or a class? You could consider disabling [order-by-type](https://docs.astral.sh/ruff/settings/#lint_isort_order-by-type) or adding `A` to constants or classes

---

_Label `question` added by @MichaReiser on 2025-10-13 08:50_

---

_Comment by @bzoracler on 2025-10-13 10:10_

Thank you for the explanation!

My real case actually has the names `N`, `NN`, and `NNNs`. These are either classes or type aliases to some kind of integer sequence (and could be made from either `: TypeAlias` or `= TypeAliasType()`; choice depends on runtime introspection requirements). The naming scheme indicates the total number of type arguments the class/type expects to receive (and `Ns` indicates a `TypeVarTuple` parameter).

So, semantically they are either types or classes. But I'm guessing the use of `: TypeAlias = <string forward alias>` could be seen by ruff as a constant (I didn't check this too thoroughly).

I'm not too concerned about the actual order (even though it looked strange to have `AA, AAA, A`), but was more concerned that this behaviour is not guaranteed and may change. From your explanation, though, it sounds like the behaviour *is* guaranteed (or at least has a deliberate implementation behind it), so I'll play around with order-by-type to see if I can tweak to get something sensible in the configuration file.


---

_Comment by @MichaReiser on 2025-10-14 06:30_

> So, semantically they are either types or classes. But I'm guessing the use of : TypeAlias = <string forward alias> could be seen by ruff as a constant (I didn't check this too thoroughly).

Unfortunately, Ruff isn't able to analyze types across modules and isort only looks at the name to guess what kind a variable is. We're working on multifile analysis which should allow more accurate categorization.

> I'm not too concerned about the actual order (even though it looked strange to have AA, AAA, A), but was more concerned that this behaviour is not guaranteed and may change

No, this shouldn't change. You can use `order-by-type` or add the type vars to https://docs.astral.sh/ruff/settings/#lint_isort_classes

---

_Closed by @bzoracler on 2025-10-14 11:16_

---
