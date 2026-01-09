---
number: 15251
title: Revisit adding all PEP585 names to UP006
type: issue
state: open
author: MichaReiser
labels:
  - rule
assignees: []
created_at: 2025-01-04T11:09:35Z
updated_at: 2025-01-04T11:13:51Z
url: https://github.com/astral-sh/ruff/issues/15251
synced_at: 2026-01-07T13:12:16-06:00
---

# Revisit adding all PEP585 names to UP006

---

_Issue opened by @MichaReiser on 2025-01-04 11:09_

https://github.com/astral-sh/ruff/pull/5454 added all PEP-585 names to UP006 but this caused multiple issues which is why I decided to revert it in https://github.com/astral-sh/ruff/pull/15250

We should revisit if we want this change and how we resolve possible conflicts, as summarized by a contributor:

> I have raised a few issues with this change:
> 
> * https://github.com/astral-sh/ruff/issues/15246
> 
>   Even if the detection+fix ability is better now that these are implemented in `UP006`, something needs to be done about the fact that the newly added warnings are all duplicates of warnings and fixes that already exist in `UP035`.
> 
>   If this implementation becomes a little more polished, it probably should completely replace the implementation that's currently in `UP035`, there's really no reason to still keep that inferior implementation other than its battle-tested status. Although maybe for historic consistency, even if the *implementation* is unified towards `UP006`, the set of items that it triggers for should remain split between `UP006` (covering only builtins such as `list`) and `UP035` (covering others) like it's always been.
> 
> * https://github.com/astral-sh/ruff/issues/15245
> 
>   The fixer has bugs where it sometimes fails to kick in, and in corner cases kicks in with strange results.
>   
> For now I'd like to see this pull request reverted and relanded later after a more detailed discussion, because it has ongoing bugs and possibly introduces undesirable/unexpected changes to what the rules are supposed to cover.
> 
> _Originally posted by @oprypin in https://github.com/astral-sh/ruff/issues/5454#issuecomment-2569976304_
>             

---

_Label `rule` added by @MichaReiser on 2025-01-04 11:11_

---

_Referenced in [astral-sh/ruff#20659](../../astral-sh/ruff/pulls/20659.md) on 2025-10-02 20:55_

---
