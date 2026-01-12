```yaml
number: 8663
title: "isort: Add support for the ``from-first`` setting"
type: pull_request
state: merged
author: jelmer
labels:
  - isort
assignees: []
merged: true
base: main
head: isort-from-first
created_at: 2023-11-13T19:41:40Z
updated_at: 2025-03-27T15:34:53Z
url: https://github.com/astral-sh/ruff/pull/8663
synced_at: 2026-01-12T15:55:26Z
```

# isort: Add support for the ``from-first`` setting

---

_@jelmer_

# Summary

This setting behaves similarly to the ``from_first`` setting in isort upstream, and sorts "from X import Y" type imports before straight imports.

Like the other PR I added, happy to refactor if this is better in another form.

Fixes #8662 

# Test plan

I've added a unit test, and ran this on a large codebase that relies on this setting in isort to verify it doesn't have unexpected side effects.

---

_Comment by @github-actions[bot] on 2023-11-13 20:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @jelmer on 2023-11-13 20:27_

---

_Label `isort` added by @charliermarsh on 2023-11-14 00:17_

---

_@charliermarsh reviewed on 2023-11-14 18:12_

Could we instead explore incorporating this into `ModuleKey` and `MemberKey`? We carved those out recently to make the sorting easier to modify.

---

_Comment by @jelmer on 2023-11-14 20:31_

> Could we instead explore incorporating this into `ModuleKey` and `MemberKey`? We carved those out recently to make the sorting easier to modify.

How to do that doesn't seem obvious to me - I'd be grateful for some feedback.

from_first applies in isort even if force_within_sections is not enabled; see https://github.com/PyCQA/isort/blob/c574c4c190ceddb66b10330ae811a95306f07b74/isort/output.py#L90

I could add something like ModuleKey, e.g. ImportTypeKey?

Alternatively perhaps a ``preferred_import_type: Option<bool>`` (set to Some(true) for ImportFrom when first_from is enabled, and Some(true) for Imports if it is not enabled) to ModuleKey, and make ``from_first`` imply ``force_sort_within_sections``. That's diverging slightly from isort, but I'm not sure if that's a big deal?

---

_Comment by @charliermarsh on 2023-11-14 21:32_

@bluthej - Curious if you have any thoughts on how `from-first` could be rolled into the new sort key design?

---

_Comment by @bluthej on 2023-11-14 21:53_

I guess I would have done something along those lines, the only thing is I don't see why we need a `Box` in there... I'll take a closer look when I get a chance!

The alternative would be to add an item in `ModuleKey` that should be positioned appropriately to either have straight and from imports one way, the other way or mixed (with `force-sort-within-sections`). 

FWI I just submitted a quick PR that moves some of this logic into `order.rd`. This was only a small refactor to make things a bit more tidy but I thought I'd mention it just in case :) 

---

_Comment by @jelmer on 2023-11-14 22:17_

> I guess I would have done something along those lines, the only thing is I don't see why we need a `Box` in there... I'll take a closer look when I get a chance!
> 
> The alternative would be to add an item in `ModuleKey` that should be positioned appropriately to either have straight and from imports one way, the other way or mixed (with `force-sort-within-sections`).
> 
> FWI I just submitted a quick PR that moves some of this logic into `order.rd`. This was only a small refactor to make things a bit more tidy but I thought I'd mention it just in case :)

I had trouble coercing rust into thinking that the two iterators were the same type - the only way I managed to was with a box. That's probably just my ignorance on the rust side..

---

_Review requested from @charliermarsh by @jelmer on 2023-11-16 11:11_

---

_Comment by @jelmer on 2023-11-16 11:13_

@charliermarsh PTAL; now updated after the recent refactoring. It no longer uses box, and mimicks the behaviour in isort.

---

_Comment by @bluthej on 2023-11-18 13:53_

@charliermarsh I had a closer look at the changes (sorry for the delay, I've been kind of swamped). I personally think this implementation is the way to go. In my mind, the items in `ModuleKey` are useful for when we really have to do the comparison on a per import basis, whereas here we are ordering two broad classes of imports (namely straight and from imports). We would be doing way more comparisons than by just doing one `if` statement to order the blocks, as proposed here. Plus it's not like there's a "dynamic range" of import types for this setting, there's just straight and from imports.

@jelmer I had a feeling `collect` would help get rid of the `Box`es ;)

My only comment is that there does not seem to be any interaction between this setting and `force-sort-within-sections`. When `force-sort-within-sections = true` I don't think we should be placing blank lines between straight and from imports. Plus I just tested it on the latest Ruff release and setting both options currently has an odd behavior, namely it will only place a blank line before the first from import that appears after a straight import!
If you both agree, I think it would be nice to fix it in this PR, but I'd be happy to submit a separate PR after this one if you prefer :)

---

_Comment by @charliermarsh on 2023-11-18 23:13_

> If you both agree, I think it would be nice to fix it in this PR, but I'd be happy to submit a separate PR after this one if you prefer :)

I agree. Do you have a sense of how to make that change? It may just be easier to put up a separate PR on top of this one and I can merge them in order.

---

_Comment by @bluthej on 2023-11-19 09:32_

The most straightforward way to fix it would be to add `&& !settings.force_sort_within_sections` in the if statements that add the lines. Of course there should be a test for that! 
@jelmer would you like to take care of it or should I make a PR on top of yours? 

But I don't know what the general policy is regarding incompatible settings in Ruff, do you usually make one setting silently override the other or do you throw an error and notify the user that their config is inconsistent? 

---

_Comment by @jelmer on 2023-11-20 09:56_

@bluthej My setup actually relies on both ``force-sort-within-sections`` and ``from-first`` together in our isort config, with only a single empty line appearing before the first from import - although it might only make sense if you only use no_sections.

The ``force_alphabetical_sort`` setting in isort specifically sets these options together:

https://github.com/PyCQA/isort/blob/b67a6a595803710f6af52865d82d996f264224c6/isort/settings.py#L278

---

_Comment by @bluthej on 2023-11-20 18:56_

@jelmer I'm sorry my message was totally confusing, what I really meant to say was that there does not seem to be any interaction between `force_sort_within_sections` and `lines_between_types`, which is loosely related to `from_first` because you had to handle the insertion of blank lines. That's something I hadn't noticed prior to looking at your PR, that's why I was mentioning it.

---

_Comment by @bluthej on 2023-11-20 18:58_

But that makes me think this PR is fine, I'll open a separate issue for that.

---

_Comment by @jelmer on 2023-11-21 11:19_

> @jelmer I'm sorry my message was totally confusing, what I really meant to say was that there does not seem to be any interaction between `force_sort_within_sections` and `lines_between_types`, which is loosely related to `from_first` because you had to handle the insertion of blank lines. That's something I hadn't noticed prior to looking at your PR, that's why I was mentioning it.

Ah, sorry, I did indeed misunderstand. That makes sense to me though - both your comments on force_sort_within_sections and lines_between_types interactions, and doing that as a separate change.

---

_@charliermarsh approved on 2023-11-21 23:31_

Thanks!

---

_Merged by @charliermarsh on 2023-11-21 23:36_

---

_Closed by @charliermarsh on 2023-11-21 23:36_

---

_Branch deleted on 2025-03-27 15:34_

---
