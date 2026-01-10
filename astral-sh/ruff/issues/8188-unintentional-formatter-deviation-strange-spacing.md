```yaml
number: 8188
title: "Unintentional formatter deviation: strange spacing around `TypeAlias` declarations"
type: issue
state: closed
author: stinodego
labels:
  - formatter
  - needs-decision
assignees: []
created_at: 2023-10-24T22:10:49Z
updated_at: 2023-10-26T15:11:18Z
url: https://github.com/astral-sh/ruff/issues/8188
synced_at: 2026-01-10T11:09:50Z
```

# Unintentional formatter deviation: strange spacing around `TypeAlias` declarations

---

_Issue opened by @stinodego on 2023-10-24 22:10_

Reproducible example:
```python
from typing import TypeAlias

MultiColSelector: TypeAlias = (
    "slice | range | list[int] | list[str] | list[bool] | Series"
)
```

black leaves this alone, while ruff formats it as follows:
```diff
- MultiColSelector: TypeAlias = (
-     "slice | range | list[int] | list[str] | list[bool] | Series"
- )
+ MultiColSelector: (
+     TypeAlias
+ ) = "slice | range | list[int] | list[str] | list[bool] | Series"
```

This looks like an undesirable change to me.

In our code base, I worked around this by rewriting the string as a `Union` of types.

Command: `ruff format .`
Formatter settings: None
Version: ruff `0.1.2`

_P.S.: Other than the two minor issues I just reported, the new ruff formatter is working excellently. It has improved formatting in quite a few cases. Thank you for the amazing work!_

---

_Comment by @MichaReiser on 2023-10-24 23:47_

Thank's for reporting and thank you for the kind words. 

This deviation is documented as [type annotations may be parenthesized](https://docs.astral.sh/ruff/formatter/black/#type-annotations-may-be-parenthesized-when-expanded) and is related to [7315](https://github.com/astral-sh/ruff/issues/7315). 

Let's keep this issue open to collect some more feedback as I can see that the formatting in this case is not ideal. I also want to re-test if implementing https://github.com/astral-sh/ruff/issues/6975 (preview style) will make fixing it obsolete. 

---

_Label `formatter` added by @MichaReiser on 2023-10-24 23:47_

---

_Label `needs-decision` added by @MichaReiser on 2023-10-24 23:47_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-24 23:47_

---

_Closed by @charliermarsh on 2023-10-26 02:51_

---

_Comment by @henryiii on 2023-10-26 13:57_

AFAICT, the rule should be that type annotations are only parenthesized if the type annotation itself is long. It should never parenthesize a type annotation to make something after the equals sign have more space.

---

_Comment by @charliermarsh on 2023-10-26 15:11_

We ended up reverting this decision (so we no longer have this Black deviation). Once we implement preview style, we should have roughly the behavior you describe.

---
