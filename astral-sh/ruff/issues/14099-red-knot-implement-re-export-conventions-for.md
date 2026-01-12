```yaml
number: 14099
title: "[red-knot] implement re-export conventions for imports"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-11-05T00:12:48Z
updated_at: 2025-02-14T09:47:53Z
url: https://github.com/astral-sh/ruff/issues/14099
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] implement re-export conventions for imports

---

_@carljm_

See https://typing.readthedocs.io/en/latest/spec/distributing.html#import-conventions

A normal `from foo import bar` in module `a` should not allow another module to do `from a import bar`; an explicit `from foo import bar as bar` should be required to specify a re-export.

In the current typing spec, these rules are worded as applicable to all modules. Mypy does apply them in all modules, pyright applies them only in type stubs.

I have a vague memory that these originated as stub-only conventions, but I haven't been able to dig up confirmation.

We need to decide if we will apply these conventions in all modules, or only in stubs, and then implement them accordingly.

In particular this is problematic at the moment because we don't apply these conventions in `builtins.pyi`, meaning we understand all symbols imported into `builtins.pyi` (e.g. various `typing` module symbols) as builtin names.


---

_Label `red-knot` added by @carljm on 2024-11-05 00:12_

---

_Comment by @AlexWaygood on 2024-11-05 07:54_

As you say, it's very important that we apply these semantics in stub files.

For runtime files, my instinct is to say that we should copy the runtime semantics exactly, but have an additional disabled-by-default error code (let's call it `no-implicit-reexport`) where we warn if you try to import something that's only implicitly re-exported from a module. (I actually believe that's what mypy does?) I think that should work fine for most cases — the only problem I can see is imports inside `if TYPE_CHECKING` blocks, which won't exist at runtime but which we will assume to exist if we're running without the `no-implicit-reexport` error code enabled. I'm not sure the unsoundness there is significant, though — I can't remember it ever coming up in the mypy issue tracker.

---

_Comment by @charliermarsh on 2024-11-05 13:25_

I could look into this.

---

_Comment by @carljm on 2024-11-05 15:49_

> For runtime files, my instinct is to say that we should copy the runtime semantics exactly, but have an additional disabled-by-default error code (let's call it no-implicit-reexport) where we warn if you try to import something that's only implicitly re-exported from a module. (I actually believe that's what mypy does?)

This makes sense to me. I definitely think that if we see an import of a not-explicitly-re-exported import, we should still infer the correct type that we are able to observe from the runtime semantics, even if we also issue a warning. I think the only difference between what you suggest here and what mypy does (according to my local testing) is that it appears to me the warning is enabled by default in mypy, not disabled by default. I get the warning without using a mypy config file or any additional CLI flags.

One additional wrinkle to throw in -- I got clarification at https://github.com/microsoft/pyright/discussions/9385 that pyright makes a distinction I hadn't realized here: it only applies these import rules to non-stub modules if they are third-party modules, not first-party ones. The idea is that these conventions are how you clarify the public API of a typed library, but they don't apply within a codebase. I can see the rationale there, and I can also see how the structure of the specification suggests that: the "import conventions" section is within the page about distributing typed libraries. So I think I would weakly favor making the same distinction?

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 20:31_

---

_Comment by @hauntsaninja on 2024-11-09 23:42_

Mypy's behaviour is:
- Always have `--no-implicit-reexport`-like behaviour enabled for stub files (this comes from PEP 484)
- `--no-implicit-reexport` applies to source, first party and third party, and is part of `--strict` 
- Mypy has per-module configuration options that let you silence this if you use some third party package that doesn't use `__all__` or `X as X` reexports

Random thoughts:
- Agreed on never paying double jeopardy, so always infer a type when you issue a warning. mypy and pyright didn't use to do this and both had to change
- The check is a little pedantic in that a) when badness actually strikes later you'll usually eventually get a type check error, b) it doesn't match runtime semantics. But it's useful and I have seen it correctly flag things that would later prove to be issues or simplify weird imports (I've even seen cases where it actually lets you get rid of some of your dependency tree)
- Pyright's first vs third party distinction is interesting. I guess it solves the problem where a third party module doesn't want to do `__all__` or re-export (for a while pallets projects held out). With mypy, you need to add a per-module config to solve that problem.
  Edit: this isn't right, I got pyright's behaviour backwards / pyright also draws a distinction between py.typed third party and non-py.typed third party.
- But I think this check's "preventing later badness" is much more useful with third party code than with first party code, since it's much more possible to have version skew with third party code between type check time and use time. So it's useful to have a way to get the strict behaviour everywhere, as with mypy
- One thing not discussed is an `__init__.py` heuristic, see https://github.com/python/mypy/issues/10198. If red-knot wants to encourage this check, e.g. have it on by default for source files, I'd definitely recommend treating `__init__.py` specially (i.e. if there are zero explicit reexports in `__init__.py` treat everything as reexported). I suspect this would be enough to obviate the need for first vs third party distinction
- One other related behaviour to think through is whether you should fail open or closed on `__all__` patterns that red-knot doesn't recognise

---

_Comment by @carljm on 2024-11-09 23:46_

Thanks, that's useful!

One note on pyright's first vs third party distinction; I think it works in the opposite way from what you're suggesting. Pyright applies strict re-export rules to imports from third party libraries, but not to imports from your own first-party code. The idea is that this allow library authors more control over their exported api surface. 

---

_Comment by @MichaReiser on 2024-12-12 12:36_

@charliermarsh are you still planning to work on this?

---

_Comment by @charliermarsh on 2024-12-12 13:21_

Unfortunately dropped the ball here. I’ll re-assign if I pick it back up, but others are welcome to take it over.

---

_Unassigned @charliermarsh by @charliermarsh on 2024-12-12 13:21_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-12-16 11:36_

---

_Removed from milestone `Red Knot 2024` by @carljm on 2025-01-09 17:43_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 17:43_

---

_Closed by @dhruvmanila on 2025-02-14 09:47_

---

_Closed by @dhruvmanila on 2025-02-14 09:47_

---
