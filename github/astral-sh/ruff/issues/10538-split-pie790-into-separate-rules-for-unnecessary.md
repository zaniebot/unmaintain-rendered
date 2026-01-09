---
number: 10538
title: "Split `PIE790` into separate rules for unnecessary `pass` vs `...`; I want to keep `...` in example code"
type: issue
state: open
author: Zac-HD
labels:
  - rule
assignees: []
created_at: 2024-03-23T20:14:05Z
updated_at: 2024-03-25T14:32:55Z
url: https://github.com/astral-sh/ruff/issues/10538
synced_at: 2026-01-07T13:12:15-06:00
---

# Split `PIE790` into separate rules for unnecessary `pass` vs `...`; I want to keep `...` in example code

---

_Issue opened by @Zac-HD on 2024-03-23 20:14_

The autofix to remove useless `pass` statements is great, but I currently disable the whole rule because it _also_ catches `...`.  These constructs are equivalent at execution time, but ellipses do in fact convey something to the reader, and therefore I want to keep them for example code.  Consider e.g.:

> The way to think about the recursive strategy is that you're repeatedly building up a series of strategies as follows:
> 
> ```python
> s1 = base
> s2 = one_of(s1, extend(s1))
> s3 = one_of(s2, extend(s2))
> ...
> ```

In other cases, it's useful to show where not-currently-relevant code has been omitted.  Using a comment is sometimes a reasonable substitute, but in already-commented code it's nice to have a distinguished marker which is _not_ an explanatory comment.

I'm aware of a variety of other uses in type-checking etc. which might also motivate splitting out this rule, but haven't looked into them.

---

_Label `rule` added by @charliermarsh on 2024-03-23 21:33_

---

_Comment by @zanieb on 2024-03-24 20:22_

Given all the problems we've had with ellipses in type checking contexts it might indeed be sensible to just split these into separate rules 🤔 

---

_Comment by @charliermarsh on 2024-03-24 20:23_

I'm fine with it. We could implement the related Pylint rule (https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/unnecessary-ellipsis.html) and remove ellipses from this rule (although we just added them here, it was originally in preview).

---

_Comment by @MichaReiser on 2024-03-25 07:32_

It may also be helpful to think about how we would categorize the two rules in the future. It would be a strong reason to split them if we would categorize them differently. 

I'm undecided on the categories but my initial thinking is:

* ellipsis: `restriction`. It generally seems fine to use ellipsis because they have a specific meaning (even though it has the same runtime semantics as `pass`)
* `pass`: `complexity`. The function definition is overly complex and it can be simplified by removing the `pass` statement without altering its semantics. 

CC: @AlexWaygood 

---

_Comment by @AlexWaygood on 2024-03-25 12:58_

I think it's reasonable to want to distinguish between `...` and `pass` for this rule, since they're conventionally used for different purposes even if they have identical semantics from Python's perspective when they're used as statements. However, while I don't really have a strong opinion here, I think I lean towards making the rule configurable rather than creating a separate rule for `...`s.

I know pyright treats `...` differently to `pass` in the context of function and class bodies. As far as I know, however, that's a pyright-specific thing that's never been formally enshrined in a typing spec of any sort. Mypy does not treat `...` differently to `pass` in these contexts, as far as I'm aware, and pyright's choice here has always seemed slightly odd to me. (But I'm not saying we shouldn't support pyright users. We should obviously make it easy for people to use the two tools at the same time; they're both great!)

So I see the motivation for removing `...` and `pass` as basically the same. In both cases, the rule is pushing you to simplify the code by removing statements that are, strictly speaking, redundant.

---

_Comment by @MichaReiser on 2024-03-25 13:57_

@AlexWaygood so what I understand from your description is that you would configure both rules with the same severity? Would the detection of `...` be enabled by default?

---

_Comment by @AlexWaygood on 2024-03-25 13:59_

Yeah, I would probably have them both in the same category and the same severity, and would probably have them both enabled by default. But as I say, no _strong_ opinion here.

---

_Comment by @zanieb on 2024-03-25 14:20_

Here's the relevant commentary from the Pyright author just for reference https://github.com/astral-sh/ruff/issues/8756

I sort of strongly prefer separate rules over configuration options.

Are all "useless statements" going to go into "complexity"?

---

_Comment by @AlexWaygood on 2024-03-25 14:26_

> Are all "useless statements" going to go into "complexity"?

I haven't yet decided whether "complexity" will be its own category in my proposal, or just a subcategory (and whatever I propose, we'll obviously discuss it internally before going ahead with anything 😄). But to me, it makes sense for all "useless statement" rules to be part of the same category, yeah

---

_Comment by @zanieb on 2024-03-25 14:27_

It feels like a bit of a weird home. I'd never turn on the complexity rules personally, but I would absolutely want useless statements to be detected. I'd say it's almost "suspicious" :) but of course a `pass` in an empty body is pedantic to remove and a `pass` in the middle of a function is quite suspicious.

---

_Comment by @AlexWaygood on 2024-03-25 14:32_

Yeah I think "style" might make more sense than "complexity" for "redundant statement" rules. (I only said above that I thought they should all be in the _same_ category ;). But let's not get _too_ into the weeds of rule recategorisation details here ;)

---
