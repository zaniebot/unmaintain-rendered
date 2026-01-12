```yaml
number: 14493
title: Consider quotes inside format-specs when choosing the quotes for an f-string
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/f-string-expressions-format-spec
created_at: 2024-11-20T15:49:52Z
updated_at: 2024-11-22T12:45:32Z
url: https://github.com/astral-sh/ruff/pull/14493
synced_at: 2026-01-12T15:55:48Z
```

# Consider quotes inside format-specs when choosing the quotes for an f-string

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13935

## Test Plan

Added test


---

_Label `formatter` added by @MichaReiser on 2024-11-20 15:50_

---

_Label `preview` added by @MichaReiser on 2024-11-20 15:50_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-11-20 15:50_

---

_Comment by @github-actions[bot] on 2024-11-20 15:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-11-20 16:01_

Hmm, this now leads to invalid syntax for `"aaa " f'{1=: "abcd {'aa'}}'`

---

_Comment by @MichaReiser on 2024-11-20 16:29_

Okay, this is a pre-existing issue, but not one that gets fixed by this PR. 

```py
f'{1=: "abcd {'aa'} \'\'}'
```

We can't change the outer quotes because the format-spec quotes then become the "closing" quotes of the entire-fstring (which they shouldn't). That means we have to preserve the quotes if:

* an expression has a format spec that contains the current quote character
* the expression uses debug text

---

_Comment by @MichaReiser on 2024-11-20 17:29_

I'll do another review myself of the code changes with a fresh mind tomorrow morning but I think this is mostly correct.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:965 on 2024-11-20 17:36_

This is slightly too naive because it incorrectly considers escaped quotes. I think this is fine considering how rare this syntax is and it doesn't lead to invalid syntax, it only results in ruff not picking the preferred quotes when it could.

---

_@MichaReiser reviewed on 2024-11-20 17:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/normalize.rs`:965 on 2024-11-21 07:01_

They can be nested as well but as you mentioned (and I agree) the syntax itself is rare enough to not worry about it. For example, `f"{x:a{y:b{z}}}"`

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/normalize.rs`:941 on 2024-11-21 07:04_

The documentation seems to be a bit misleading as per the logic. I don't see it considering the debug expression.

I would also reword it as "Returns `true` if the `f_string` has any part that ..."

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/normalize.rs`:942 on 2024-11-21 07:04_

To be consistent, we're using this format everywhere although I see it's not being enforced and there are usages of both, I can change that in a different PR
```suggestion
    f_string: &FString,
```


---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/implicit.rs`:250 on 2024-11-21 07:13_

The only change here is the addition of this condition to return the quote that needs to be preserved? I'm curious to know why was this condition not present before this? It seems like it should've been present.

---

_@dhruvmanila reviewed on 2024-11-21 07:14_

---

_@MichaReiser reviewed on 2024-11-21 07:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:965 on 2024-11-21 07:16_

Hmm, let me have a look if that means we also need to test for the expression parts

---

_@MichaReiser reviewed on 2024-11-21 07:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:942 on 2024-11-21 07:21_

I prefer to keept it as is so that it's consistent with the function name, but I agree that we have both forms of spelling fstring. Although I don't think this is a big deal

---

_@MichaReiser reviewed on 2024-11-21 07:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/implicit.rs`:250 on 2024-11-21 07:24_

Not 100%. I would have to remind myself of how the pre 312 fstring formatting works again. It might also just be that we were missing a test. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:965 on 2024-11-21 10:17_

I added some more tests... this is tricky :laughing: 

---

_@MichaReiser reviewed on 2024-11-21 10:17_

---

_@dhruvmanila approved on 2024-11-21 11:12_

---

_Merged by @MichaReiser on 2024-11-22 12:43_

---

_Closed by @MichaReiser on 2024-11-22 12:43_

---

_Branch deleted on 2024-11-22 12:43_

---
