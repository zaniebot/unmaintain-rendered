```yaml
number: 17443
title: "[`pyupgrade`] Add fix safety section to docs (`UP030`)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc_fix_safety_for_format_literals
created_at: 2025-04-17T08:23:09Z
updated_at: 2025-04-21T18:14:58Z
url: https://github.com/astral-sh/ruff/pull/17443
synced_at: 2026-01-12T15:56:02Z
```

# [`pyupgrade`] Add fix safety section to docs (`UP030`)

---

_@Kalmaegi_



## Summary

add fix safety section to format_literals, for #15584 

---

_Renamed from "add fix safety section to docs" to "add fix safety section to format_literals docs" by @Kalmaegi on 2025-04-17 08:37_

---

_Label `documentation` added by @ntBre on 2025-04-18 02:48_

---

_Comment by @github-actions[bot] on 2025-04-18 02:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @ntBre by @ntBre on 2025-04-18 14:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/format_literals.rs`:44 on 2025-04-18 14:29_

I don't think this second part is true, the implementation will swap the order of the `format` arguments if the string indices are out of order. You can see this when applying the quick fix here, for example: https://play.ruff.rs/d02b59f6-abb0-4a57-b5f7-e3b79597caf6

I'm actually not even sure about the comment part. I  played with this on the playground too, and even in cases like this:

```py
"{1}, {0}".format( # comment 1
    "Hello", # comment 2 
    "World", # comment 3
    )
```

I couldn't get it to delete any comments.

Ah, I think I found a reason it can be unsafe:

```py
def print2(arg):
    print(arg)
    return arg

"{1}, {0}".format( # comment 1
    print2("Hello"), # comment 2 
    print2("World"), # comment 3
    )
```

If arguments to `format` have side effects (printing in this case), and the format string indices are in the wrong order, the autofix will reorder the arguments, changing the behavior of the program. 


---

_@ntBre reviewed on 2025-04-18 14:30_

Thanks! This was quite a tricky one, but I think I came up with a scenario where the rule is definitely unsafe. What do you think?

---

_@Kalmaegi reviewed on 2025-04-18 15:16_

---

_Review comment by @Kalmaegi on `crates/ruff_linter/src/rules/pyupgrade/rules/format_literals.rs`:44 on 2025-04-18 15:16_

I'm a bit embarrassed I didn't test this thoroughly. This rule doesn’t delete or modify comments.
Not sure if we need to adjust comment reordering logic. Currently, swapping arguments leaves comments misplaced. What do you think of this warning:

```
This fix is unsafe: because:

-Comments attached to arguments are not moved, which can cause comments to mismatch the actual arguments.
-If arguments have side effects (e.g., print), reordering may change program behavior.
```

---

_@ntBre reviewed on 2025-04-18 16:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/format_literals.rs`:44 on 2025-04-18 16:57_

No need to be embarrassed! I think fix safety is pretty tricky in general, and I'm guessing the rules you're working on don't have existing documentation because it's hard to tell why they're unsafe :slightly_smiling_face: 

Good catch on the comment swapping too! I think that looks good, I would just remove the colon after `unsafe:` and add spaces after the `-` at  the start of the bulleted list items (but markdown might not mind that anyway).

---

_Comment by @Kalmaegi on 2025-04-19 06:43_

I tested it again, and currently, sometimes the annotations are swapped, while other times they are not. There might be errors in cases where swapping occurs, but I think these are rare edge cases.
before: 
```
def take_two(x: int, y: int) -> int:
    return x + y

def take_one(x: int) -> int:
    return x + 1

"{1}, {0}".format( # comment 1
    take_two(1, # comment 2
    2), # comment 3 
    take_one(1),
)
```

after:
```
def take_two(x: int, y: int) -> int:
    return x + y

def take_one(x: int) -> int:
    return x + 1

"{}, {}".format( # comment 1
    take_one(1), # comment 3 
take_two(1,  # comment 2
    2), 
)
```

---

_@ntBre approved on 2025-04-21 18:14_

Very nice, thanks!

---

_Renamed from "add fix safety section to format_literals docs" to "[`pyupgrade`] Add fix safety section to docs (`UP030`)" by @ntBre on 2025-04-21 18:14_

---

_Merged by @ntBre on 2025-04-21 18:14_

---

_Closed by @ntBre on 2025-04-21 18:14_

---
