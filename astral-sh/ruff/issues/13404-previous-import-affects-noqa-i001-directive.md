```yaml
number: 13404
title: "previous import affects # noqa: I001 directive"
type: issue
state: closed
author: AntiSol
labels:
  - question
assignees: []
created_at: 2024-09-19T03:40:55Z
updated_at: 2024-09-19T08:02:34Z
url: https://github.com/astral-sh/ruff/issues/13404
synced_at: 2026-01-12T15:54:53Z
```

# previous import affects # noqa: I001 directive

---

_@AntiSol_

Hi, I'm seeing a weird issue when I ignore import sorting for one module. If I have another import statement before the `#noqa: I001` directive, ruff ignores the `noqa` directive and tries to sort imports for that file. However having an import statement _after_ the manually-sorted imports works as expected*.

* I tried searching for a few things like I001, isort, etc, didn't see any likely candidates that described my issue

* code without the issue (ruff is happy): https://play.ruff.rs/bf6b5099-2f83-4277-8777-08d286384606

* code showing the issue (simply add an import before the import with the #noqa: I001 directive to make ruff unhappy): https://play.ruff.rs/cd78f390-38c1-4769-85c3-55267a4f6fec
* Notice that applying the "fix" also "fixes" unsorted imports even for the file with the noqa directive applied.
* Notice that moving the `from a_module import something` import to below the multi-line import does not trigger I001, which may or may not be considered correct behaviour - I want manually-sorted imports _within_ the z_file import, but IMO the a_file import should probably still be sorted before z_file. However one could argue that disabling I001 for the z_file import removes it from the module sort order (I consider this less desirable, but it's a matter of opinion)

* The current Ruff version: `ruff 0.6.5`


---

_Comment by @AntiSol on 2024-09-19 03:54_

Update: workaround: add a block comment between your imports: https://play.ruff.rs/4ed0f148-ee97-487d-8d9e-89fda8e3d568
 

---

_Label `question` added by @MichaReiser on 2024-09-19 06:51_

---

_Comment by @MichaReiser on 2024-09-19 07:01_

Thanks for the nice write-up and for sharing the examples in the playground!

This behavior is a consequence that Ruff only creates a single diagnostic for the entire import block and adding a new import at the top changes the diagnostic range to the first import's line. 

I recommend you use the [`isort` action comments](https://docs.astral.sh/ruff/linter/#action-comments) to suppress sorting over `noqa`. New imports should not be impacted for as long as they are all added above or below the existing import. 

```python
from a_module import something

# isort: off
# imports from z_module have their own manual order
from z_module import (
    # functions
    d,
    a,
    # variables
    b,
    e,
    c,
)
# isort: on

from b import c
```

(for some reason the isort action comments are ignored in the playground)



---

_Comment by @AntiSol on 2024-09-19 07:54_

Hey, thanks for the quick response, glad my report is useful.

The isort off/on directives did indeed do the trick, too, thanks.

---

_Closed by @MichaReiser on 2024-09-19 08:02_

---
