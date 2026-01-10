---
number: 21305
title: "Support and lints for `lazy` imports in Python 3.15+"
type: issue
state: open
author: dylwil3
labels:
  - python315
assignees: []
created_at: 2025-11-06T20:22:43Z
updated_at: 2025-11-10T12:45:03Z
url: https://github.com/astral-sh/ruff/issues/21305
synced_at: 2026-01-10T01:23:02Z
---

# Support and lints for `lazy` imports in Python 3.15+

---

_Issue opened by @dylwil3 on 2025-11-06 20:22_

A long ways off, but creating this tracking issue to add ideas as they arise.

Python 3.15 will introduce [PEP 810 - Explicit lazy imports](https://peps.python.org/pep-0810). We will certainly have to do the following:

- [ ] Update the parser
- [ ] Update the formatter
- [ ] Update the semantic model (though according to [this subheading](https://peps.python.org/pep-0810/#typing-and-tools) these will be treated the same as ordinary imports for our purposes)

But we may also want to consider some lints. For example:

1. Sorting these imports (this requires a few design decisions)
2. Adjust the fixes in [`flake8-type-checking`](https://docs.astral.sh/ruff/rules/#flake8-type-checking-tc) to suggest that typing-only imports be _lazily_ imported rather than gated behind `if TYPE_CHECKING`, as per [this use-case in the PEP](https://peps.python.org/pep-0810/#what-about-type-annotations-and-type-checking-imports).
3. Relatedly - replacing existing `if TYPE_CHECKING: ...` imports with lazy imports.
4. Suggesting lazy imports any time we see an import not at the beginning of the file. (Developers sometimes approximate lazy imports these days by importing in the body of a function, for example.)
5. More aggressive/opinionated lints - e.g. "make all imports lazy", with an unsafe autofix.
6. Conversely, _removing_ `lazy` if we can guarantee that the import will be evaluated (e.g. by a top-level statement in the module).

---

_Label `python315` added by @dylwil3 on 2025-11-06 20:22_

---

_Comment by @Avasam on 2025-11-10 12:44_

> 4. Suggesting lazy imports any time we see an import not at the beginning of the file. (Developers sometimes approximate lazy imports these days by importing in the body of a function, for example.)
> 5. More aggressive/opinionated lints - e.g. "make all imports lazy", with an unsafe autofix.

Somewhere in between, less aggressive: Suggesting lazy imports whenever a symbol *isn't* obviously used at the module root level (still imperfect, but less false-positives than "make all imports lazy")

In other words: "5. Make all imports lazy, except when detected to be used at module root".

Further refinement with code analysis could try to follow function calls, but that seems niche.

---
