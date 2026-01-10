---
number: 10115
title: "`banned-api` (`TID251`) - support specifying quick fixes"
type: issue
state: open
author: DetachHead
labels:
  - fixes
  - needs-design
assignees: []
created_at: 2024-02-25T03:25:23Z
updated_at: 2025-01-14T07:40:01Z
url: https://github.com/astral-sh/ruff/issues/10115
synced_at: 2026-01-10T01:22:49Z
---

# `banned-api` (`TID251`) - support specifying quick fixes

---

_Issue opened by @DetachHead on 2024-02-25 03:25_

in some cases, `banned-api` rules suggest replacing an import with another import that has an equivalent type, in which case it's safe to have a quick fix that just replaces it:
```py
[tool.ruff.lint.flake8-tidy-imports.banned-api]
"typing.TypedDict".msg = "Use typing_extensions.TypedDict instead."
"typing.TypedDict".fix = "typing_extensions.TypedDict"
```

we could also make `msg` optional in cases where a fix is specified, which would just default to something like `"use {fix} instead"`

---

_Label `fixes` added by @MichaReiser on 2024-02-25 09:25_

---

_Comment by @markjm on 2025-01-09 19:13_

I think this is a great suggestion! @MichaReiser would you be willing to review a change for this feature if I worked on this?

---

_Comment by @MichaReiser on 2025-01-10 07:24_

Hi @markjm 

I think implementing this first requires some design. The case shown in the issue are somewhat straightforward because they only require changing the module from which the name is imported. But there are other cases where `banned-api` is useful. E.g.: The entire module is deprecated but there's no direct replacement for it (`cgi`), the specific name is deprecated, but the fix requires changing the imported name and updating all references and not just the module name, ... 

We should list the use cases, if a fix is supported, how it would be specified and what changes are necessary.

---

_Label `needs-design` added by @MichaReiser on 2025-01-10 07:24_

---

_Comment by @DetachHead on 2025-01-11 00:39_

> The case shown in the issue are somewhat straightforward because they only require changing the module from which the name is imported.

these cases are all i had in mind for this idea. i can't think of a way to implement fixes in more complicated cases where there's either no replacement or the replacement requires the usages to be updated. i think in those cases it should just not suggest a quick fix

---

_Comment by @markjm on 2025-01-14 01:58_

Thanks @MichaReiser ,

Given the configuration which would provide fixes is wholly in control of the individual crafting their config, I think it makes sense to empower the project maintainer to decide if they should specify a fixer like present above. I do not believe @DetachHead was suggesting that all banned-apis are required to have a fixer, just that you are allowed to specify one if you know, for your project, that the fixer is valid.

Some common cases we use this rule for are vendoring and shimming modules,

For example, if we want to use a 3p package but decide for some reason or another that we want to vendor the package, I may have a rule like

```
"old_package".fix = "third_party.old_package"
```

or if I want to shim some module, I may use

```
"some_module".fix = "shims.some_module"
```

I believe these to be reasonable use cases to this rule. As a random sample, in an extremely large internal codebase I work in, we have about 60 banned import rules, and 53 of them have a message that is simply "use XYZ instead"

TLDR, you are right that not all imports would have safe fixes, but given its project-supplied content, it seems reasonable to allow for maintainers to make that consideration themselves and supporting the functionality in the rule.


_Also:_
I think wanting this functionality also speaks to the power/speed of ruff as a refactoring tool. While yes, I do want this functionality as a persistent lint rules, I also think this functionality is useful to grow ruff's general refactoring functionality. For example, the reason this initially came up for me is because I wanted to do some large-scale import refactoring and realized this ruff rule, if available, would be the safest and fastest way to achieve it!





---

_Comment by @MichaReiser on 2025-01-14 07:40_

I'm not suggesting that all usages must have fixes. I'm only asking that we think through the design:

* What are the possible fixes for this rule, regardless of if we implement them. E.g. you only show examples where the module has the same name
* How does the fix get configured? 
* How do we ensure that adding support for more fixes will be possible in the future without requiring a breaking change?
* Extra points: Do we need a way for an extending configuration to remove a fix? 


---

_Referenced in [astral-sh/ruff#16709](../../astral-sh/ruff/issues/16709.md) on 2025-03-13 14:57_

---
