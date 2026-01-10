```yaml
number: 10528
title: "Autofix for `I001` unexpectedly altering characters from Unicode Block “Letterlike Symbols”"
type: issue
state: closed
author: namurphy
labels:
  - bug
assignees: []
created_at: 2024-03-22T18:36:29Z
updated_at: 2024-03-22T19:52:58Z
url: https://github.com/astral-sh/ruff/issues/10528
synced_at: 2026-01-10T11:09:52Z
```

# Autofix for `I001` unexpectedly altering characters from Unicode Block “Letterlike Symbols”

---

_Issue opened by @namurphy on 2024-03-22 18:36_

With ruff 0.3.4, I ran into unexpected behavior where the autofix for ruff rule [`I001`](https://docs.astral.sh/ruff/rules/unsorted-imports/) is now altering some characters from [Unicode Block “Letterlike Symbols” (U+2100)](https://www.compart.com/en/unicode/block/U+2100). I suspect that this is related to #10412. 🤔 This might not be the only Unicode block that is affected by this.

For example, [`ℏ`](https://www.compart.com/en/unicode/U+210F) (U+210F; which represents Planck's constant over $2π$) is changed to [`ħ`](https://www.compart.com/en/unicode/U+0127) (U+0127; Latin Small Letter H with Stroke). To reproduce this, I created a file called `hbar.py` that contains:

```python
from astropy.constants import hbar as ℏ
from numpy import pi as π

h = 2 * π * ℏ
```

After I ran:

```console
ruff check hbar.py  --select=I001 --fix
```

I did a `git diff` and got this:

```
@@ -1,4 +1,4 @@
-from astropy.constants import hbar as ℏ
+from astropy.constants import hbar as ħ
 from numpy import pi as π
```

Similarly, if I apply `I001` to a file containing a bunch of characters from that block:
```python
import numpy as ℂℇℊℋℌℍℎℐℑℒℓℕℤΩℨKÅℬℭℯℰℱℹℴ
```
then the diff is
```
@@ -1 +1 @@
-import numpy as ℂℇℊℋℌℍℎℐℑℒℓℕℤΩℨKÅℬℭℯℰℱℹℴ
+import numpy as CƐgHHHhIILlNZΩZKÅBCeEFio
```

My expectation was for ruff to not change variable names that are valid Python names, except for rules that are designed specifically to make these changes (e.g., `RUF001`, `RUF002`, `RUF003`).

Thank you again for creating a wonderful tool!

---

_Label `bug` added by @charliermarsh on 2024-03-22 18:39_

---

_Comment by @zanieb on 2024-03-22 18:39_

Thanks for the clear write-up!

cc @AlexWaygood 

---

_Comment by @charliermarsh on 2024-03-22 18:39_

I can take if you're off for the day, Alex, up to you.

---

_Comment by @AlexWaygood on 2024-03-22 18:42_

> I can take if you're off for the day, Alex, up to you.

Yes please, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-22 18:42_

---

_Comment by @namurphy on 2024-03-22 18:47_

Thank you for the quick response, and for respecting work-life balance!  Admittedly the affected users may be limited to physicists who spend too much of their time looking up Unicode tables, so it's not too urgent.  

Also I should clarify that `ℂℇℊℋℌℍℎℐℑℒℓℕℤΩℨKÅℬℭℯℰℱℹℴ` runs ever-so-slightly counter to my usual advice for naming things.  🙃

---

_Comment by @charliermarsh on 2024-03-22 18:50_

No prob, I like fixing stuff like this.

---

_Comment by @zanieb on 2024-03-22 19:15_

Is there a scientific repository we can add to the ecosystem checks that would catch this?

---

_Closed by @charliermarsh on 2024-03-22 19:16_

---

_Comment by @namurphy on 2024-03-22 19:52_

Thank you for the amazingly quick bugfix!  I'm starting to understand better the difficulties of dealing with Unicode edge cases...

> Is there a scientific repository we can add to the ecosystem checks that would catch this?

I found it in [PlasmaPy](https://github.com/PlasmaPy/PlasmaPy) in our [notebook on Coulomb logarithms](https://github.com/PlasmaPy/PlasmaPy/blob/main/docs/notebooks/formulary/coulomb.ipynb), using nbqa-ruff.

---
