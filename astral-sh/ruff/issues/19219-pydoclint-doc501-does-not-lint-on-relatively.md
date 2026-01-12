```yaml
number: 19219
title: "[`pydoclint`] `DOC501` does not lint on relatively imported names"
type: issue
state: open
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2025-07-08T21:38:14Z
updated_at: 2025-07-08T21:38:14Z
url: https://github.com/astral-sh/ruff/issues/19219
synced_at: 2026-01-12T15:54:56Z
```

# [`pydoclint`] `DOC501` does not lint on relatively imported names

---

_@MeGaGiGaGon_

### Summary

I found this while working on #18972, it's the edge case mentioned in #19218. [docstring-missing-exception (DOC501)](https://docs.astral.sh/ruff/rules/docstring-missing-exception/#docstring-missing-exception-doc501) does not lint if the name is from a relative import. It does work on absolute imports though.

https://play.ruff.rs/a248e30e-399e-4e58-a6c4-a060d65b82ea
```py
from my_module import TruePositiveImport

def true_positive_import():
    """True positive."""  # DOC501
    raise TruePositiveImport


class TruePositiveDefined:...

def true_positive_defined():
    """True positive."""  # DOC501
    raise TruePositiveDefined


from . import FalseNegative

def false_negative():
    """False negative."""  # No error
    raise FalseNegative


def maybe_true_negative():
    """Maybe true negative?"""  # No error
    raise NotDefinedException  # F821 (Undefined name)
```

As said in #19218, imports in the function body have to undergo name resolution to figure out if they are `NotImplementedError`, which is why `DOC501` doesn't lint on undefined names. I think the issue with relative imports comes from the fact that they don't have an associated top module, so some part of the code is defaulting to skipping that case.


### Version

playground

---
