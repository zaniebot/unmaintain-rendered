```yaml
number: 14886
title: "[WIP] Editing around comments"
type: pull_request
state: closed
author: dylwil3
labels:
  - fixes
  - do-not-merge
assignees: []
draft: true
base: main
head: delete-keep-triv
created_at: 2024-12-10T04:38:37Z
updated_at: 2025-12-10T18:01:06Z
url: https://github.com/astral-sh/ruff/pull/14886
synced_at: 2026-01-12T15:55:49Z
```

# [WIP] Editing around comments

---

_@dylwil3_

This is an attempt to introduce some helper functions for making edits without removing comments. The plan is to implement the new helper functions in a number of different rules, and see if the ecosystem check raises any alarms. If the approach starts to look reasonable, I will break this into separate PRs.

---------

In case anyone is interested, the rough idea is as follows:

1. First, handle deletions. If you are making a deletion edit in some range `[s0, e0]` and there are comment ranges in this interval, say `[s1,e1], [s2,e2], ..., [sn,en]`, then you could instead just delete each of the ranges `[s0,s1], [e1,s2], ..., [en,e0]`. There is some bookkeeping that needs to be done, however, around newlines and indentation (and probably more things I haven't thought of!), but that is the general idea.
2. Insertions are already okay.
3. Any range replacement can be written as a deletion following by an insertion. So use (1) and then insert. The downside of this approach is that it pushes all the comments to the top of the replacement. But it seems like this may be slightly better than deleting them. 

I think a more sophisticated approach can be developed on top of the above by first creating a diff that breaks up the range replacement into several replacements, and then applies (3) to each piece. But one thing at a time...

Finally, you probably shouldn't look at the implementation details at the moment - the code itself is ugly and probably not performant or memory-conscious, I'm just trying to get something logically correct first and then I'll try to make it pretty.

This may all be terribly misguided... we'll see...


---

_Label `do-not-merge` added by @dylwil3 on 2024-12-10 04:38_

---

_Comment by @github-actions[bot] on 2024-12-10 04:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @AlexWaygood on 2024-12-10 11:02_

---

_@AlexWaygood reviewed on 2024-12-10 13:14_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP013.py.snap`:279 on 2024-12-10 13:14_

This is a fantastic improvement, and we shouldn't let the perfect be the enemy of the good, so I'd happily merge a change to our fix infrastructure that preserved comments like this. However... I'd still mark this fix as unsafe if it moved comments like this. It would be very annoying if you e.g. had a TypedDict like this:

```py
Config = TypedDict("Config", {
    "x": int,  # this is the x field. It's an int because numbers are great.
    "y": str,  # this is the y field. It's a string because some things are string.
    "z": bytes,  # this is the z field. TODO: why is it bytes??
})
```

And Ruff then autofixed it to this:

```py
# this is the x field. It's an int because numbers are great.
# this is the y field. It's a string because some things are string.
# this is the z field. TODO: why is it bytes??
class Config(TypedDict):
    x: int
    y: str
    z: bytes
```

One way to think about it is: would you be happy for Ruff to automatically apply these fixes every time you pressed save in an editor, like you might with an autoformatter?

---

_Closed by @dylwil3 on 2025-12-10 18:01_

---
