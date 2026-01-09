---
number: 11099
title: Add parser support for PEP 696
type: issue
state: closed
author: JelleZijlstra
labels:
  - parser
  - python313
assignees: []
created_at: 2024-04-23T05:12:50Z
updated_at: 2024-04-26T07:47:31Z
url: https://github.com/astral-sh/ruff/issues/11099
synced_at: 2026-01-07T13:12:15-06:00
---

# Add parser support for PEP 696

---

_Issue opened by @JelleZijlstra on 2024-04-23 05:12_

Ruff's parser does not support the new syntax introduced by [PEP 696](https://peps.python.org/pep-0696/):

```
def f[T=int](): pass
```

I noticed because I'm adding this syntax to CPython and Ruff started failing in CI :).

I saw that you recently switched to a handwritten parser. I'm interested in taking a look at that parser, so I'd like to try to implement this myself.

---

_Comment by @dhruvmanila on 2024-04-23 06:56_

Thank you for notifying us about the new syntax!

> I saw that you recently switched to a handwritten parser. I'm interested in taking a look at that parser, so I'd like to try to implement this myself.

Yes, of course! Although the parser crate (`ruff_python_parser`) lacks a bit of public-facing documentation, specifically a README and an updated CONTRIBUTING guidelines, it shouldn't black you as I can assist with the implementation. That said, I'll be working on the documentation this week.

I'll need to go through the PEP to list the work items needed to complete this, but for reference, there's the [PEP 701](https://github.com/astral-sh/ruff/issues/6502) issue and [PEP 695](https://github.com/astral-sh/ruff/issues/5062) issue.

At a high level, this will impact all of the major areas: the AST, parser, linter, and formatter. If you're solely interested in updating the AST and parser, that's perfectly fine; either I or someone else from the team can handle implementing the changes in the linter and formatter.


---

_Label `parser` added by @dhruvmanila on 2024-04-23 06:56_

---

_Label `python313` added by @dhruvmanila on 2024-04-23 06:57_

---

_Comment by @JelleZijlstra on 2024-04-23 13:04_

Thanks! I'll take a look myself first and let you know if I run into any issues. I expect the PEP 695 issue will provide a good reference as mechanically the new syntax is very similar to the bound syntax in PEP 695.

---

_Referenced in [astral-sh/ruff#11120](../../astral-sh/ruff/pulls/11120.md) on 2024-04-24 01:16_

---

_Comment by @JelleZijlstra on 2024-04-24 01:23_

#11120 implements support in all the places I could find in the Ruff codebase. Rust's ADTs were very nice here; the compiler basically told me almost all the places I had to update. There's a test failure that I'll look into soon.

At runtime this sort of thing is an error:

```
>>> type X[T = int, U] = str
  File "<stdin>-0", line 1
SyntaxError: non-default type parameter 'U' follows default type parameter
```

Since Ruff doesn't detect a few similar cases in the existing syntax (#11118, #11119), I left these error checks out of the PEP 696 support too. It might make sense to add a new lint rule checking for these and similar issues, but that can wait for a different PR.

---

_Comment by @dhruvmanila on 2024-04-24 05:38_

> At runtime this sort of thing is an error:
> 
> ```
> >>> type X[T = int, U] = str
>   File "<stdin>-0", line 1
> SyntaxError: non-default type parameter 'U' follows default type parameter
> ```

We do detect this specific case for function parameters. For reference:

https://github.com/astral-sh/ruff/blob/f5c7a62aa65decb9e286bd65ba17f1a3bd1f91e6/crates/ruff_python_parser/src/parser/statement.rs#L3055-L3070



---

_Closed by @MichaReiser on 2024-04-26 07:47_

---
