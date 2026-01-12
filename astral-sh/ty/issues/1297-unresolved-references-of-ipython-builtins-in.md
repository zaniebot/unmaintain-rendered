```yaml
number: 1297
title: Unresolved references of IPython builtins in notebooks
type: issue
state: open
author: jfaldanam
labels:
  - imports
  - notebook
assignees: []
created_at: 2025-10-02T11:15:39Z
updated_at: 2026-01-09T09:51:32Z
url: https://github.com/astral-sh/ty/issues/1297
synced_at: 2026-01-12T15:54:24Z
```

# Unresolved references of IPython builtins in notebooks

---

_@jfaldanam_

### Summary

When running `ty` on a Jupyter notebook, the IPython builtin `display` is flagged as an `unresolved-reference` unless it is explicitly imported. In Jupyter, `display` is provided by IPython and is commonly used without an import.

As an user, for `.ipynb` files executed in an IPython/Jupyter context, `ty` should recognize IPython-provided builtins (e.g., `display`) and not report them as unresolved.

The actual behavior is that `ty` reports `error[unresolved-reference]: Name 'display' used when not defined` for code cells that call `display(...)` without an explicit import.

# Steps to reproduce
The following minimal working example illustrates how it diagnoses and unresolve-reference for the first use of display, but not when imported explicitly.

To reproduce:
1. Save the following snippet as `test.ipynb`
  * A very simple notebook with 2 cells, the first only runs `display("Hello, world!")` while the second one runs `from IPython.display import display` and right after `display("Hello, world!")`.
2. Run `uvx ty check test.ipynb `
3. Observe the diagnostic for the first cell but not the second
```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                 error[unresolved-reference]: Name `display` used when not defined
 --> tutorial/test.ipynb:cell 1:1:1
  |
1 | display("Hello, world!")
  | ^^^^^^^
  |
info: rule `unresolved-reference` is enabled by default

Found 1 diagnostic
```

## Example to reproduce the issue

```ipynb
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "id": "9c682895",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'Hello, world!'"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "display(\"Hello, world!\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "id": "fc6f3693",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'Hello, world!'"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "from IPython.display import display\n",
    "display(\"Hello, world!\")"
   ]
  }
 ],
 "metadata": {
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.11.12"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
```

### Version

ty 0.0.1-alpha.21

---

_Comment by @sharkdp on 2025-10-06 08:25_

Thank you for reporting this. Do you know how other type-checkers handle this situation (if they have support for notebooks)? Is there a stub file available somewhere that would list all of the "builtins"?

---

_Label `imports` added by @sharkdp on 2025-10-06 08:25_

---

_Comment by @jfaldanam on 2025-10-06 10:49_

I have tried to research this with not much luck.

Empirically, I have tested with this same file with `nbqa` + `mypy` (`mypy` alone does seem to require some configuration to support notebooks).

With the same example provided above, it reports that there are no issues found:

```bash
$ uvx --with mypy nbqa mypy test.ipynb # Same file as above
Success: no issues found in 1 source file
```

However, if I remove the import in the second cell (`from IPython.display import display`), it fails on both (so it is taking any import in future cells to check for a name being defined in previous cells):

```bash
$ uvx --with mypy nbqa mypy test.ipynb  # After removing import from second cell
test.ipynb:cell_1:1: error: Name "display" is not defined  [name-defined]
test.ipynb:cell_2:2: error: Name "display" is not defined  [name-defined]
Found 2 errors in 1 file (checked 1 source file)
```

So seems like this is not widely supported, but it still is what I would expect for this kind of tools. Workarounds exist by just importing `display` or relaying on Jupyter magic calling `display` on an object if its the object alone is the last line of a cell.

---

_Label `notebook` added by @carljm on 2025-10-06 15:20_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:39_

---

_Comment by @FrancoisHuet on 2025-11-25 21:44_

Re: Do you know how other type-checkers handle this situation

To silence this error with pylint, I've been using "--additional-builtins=display".  I'm in the same boat as jfaldanam, and would be happy with a similar option -though of course, better notebook support would be ideal-.  Thanks for all your work!

Also, if I do import display, Ruff complains:
> Import `display` is shadowing a Python builtin Ruff [A004](https://docs.astral.sh/ruff/rules/builtin-import-shadowing)

---

_Comment by @Gankra on 2025-12-16 22:45_

It appears `display = IPython.display.display` and `__IPYTHON__ = True` are the only notebook builtins:

https://github.com/ipython/ipython/blob/94fb0eb544a883bb3c2fb35db9a546056006e1d8/IPython/core/interactiveshell.py#L847-L852

---

_Comment by @Gankra on 2025-12-16 22:52_

This appears to be the function definition of interest: https://github.com/ipython/ipython/blob/94fb0eb544a883bb3c2fb35db9a546056006e1d8/IPython/core/display_functions.py#L85

---

_Comment by @MichaReiser on 2026-01-09 09:51_

We now support a custom `__builtins__.pyi` where you can define the IPython builtins. This obvioulsy isn't very convenient, but a work around for now

---
