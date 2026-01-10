```yaml
number: 16187
title: Fix unstable formatting of trailing end-of-line comments of parenthesized attribute values
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/fix-instable-trailing-attribute-value-formatting
created_at: 2025-02-16T15:26:38Z
updated_at: 2025-02-18T07:43:53Z
url: https://github.com/astral-sh/ruff/pull/16187
synced_at: 2026-01-10T19:57:22Z
```

# Fix unstable formatting of trailing end-of-line comments of parenthesized attribute values

---

_Pull request opened by @MichaReiser on 2025-02-16 15:26_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/16151

This fixes a formatter instability where it incorrectly moved a trailing end-of-line comment 
of a parenthesized attribute value was moved to the next line:

```py
result = (
    (await query_the_thing(mypy_doesnt_understand))  # type: ignore[x]
    .foo()
    .bar()
)
```

Got reformatted to 

```py
result = (
    (await query_the_thing(mypy_doesnt_understand))
      # type: ignore[x]
    .foo()
    .bar()
)
```

and then finally to:

```py
result = (
    (await query_the_thing(mypy_doesnt_understand))
    # type: ignore[x]
    .foo()
    .bar()
)
```

Not only is it an instability, but it also moves the `type: ignore` comment away from the node it was attributing. 

This PR fixes both by preserving the input formatting. Both those examples are correct formattings now:

```py
result = (
    (await query_the_thing(mypy_doesnt_understand))  # type: ignore[x]
    .foo()
    .bar()
)


```py
result = (
    (await query_the_thing(mypy_doesnt_understand))  # type: ignore[x]
    .foo()
    .bar()
)

result = (
    (
        await query_the_thing(mypy_doesnt_understand)  # type: ignore[x]
    )  # other
    .foo()
    .bar()
)
```

I don't expect this to change any stable formatting because the formatter always moved trailing end-of-line comments and made them leading own-line comments of the `.` token

## Test Plan

Added tests, ecosystem check

---

_Comment by @github-actions[bot] on 2025-02-16 15:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `bug` added by @MichaReiser on 2025-02-16 15:35_

---

_Label `formatter` added by @MichaReiser on 2025-02-16 15:35_

---

_Renamed from "Fix instable formatting of trailing end-of-line comments of parenthesized attribute values" to "Fix unstable formatting of trailing end-of-line comments of parenthesized attribute values" by @AlexWaygood on 2025-02-16 22:07_

---

_Comment by @huonw on 2025-02-16 23:08_

Thanks for fixing this! (BTW, I think the code examples in the PR description aren't quite the right ones.) 😄 

---

_Comment by @MichaReiser on 2025-02-17 13:58_

@huonw thanks for making me aware. I was in the middle of writing the summary and then realized that the fix wasn't ideal. So I decided to push what I had.

---

_@MichaReiser reviewed on 2025-02-17 14:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1396 on 2025-02-17 14:00_

We want to fall through to the next case if the comment is after the closing parentheses

---

_Marked ready for review by @MichaReiser on 2025-02-17 14:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/attribute.py`:171 on 2025-02-18 07:24_

What's the significance of this test case? It seems to be working fine on `main`

---

_@dhruvmanila approved on 2025-02-18 07:25_

I'm a bit unfamiliar with fluent chain layout so it might good to get a second pair of eyes on this (Konsti?) but otherwise looks good.

---

_@MichaReiser reviewed on 2025-02-18 07:43_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/attribute.py`:171 on 2025-02-18 07:43_

I mainly wanted to make sure that I don't break this case.

---

_Comment by @MichaReiser on 2025-02-18 07:43_

> I'm a bit unfamiliar with fluent chain layout so it might good to get a second pair of eyes on this (Konsti?) but otherwise looks good.

Yeah. I don't think anyone is very familiar with this specific code at the moment :)

---

_Merged by @MichaReiser on 2025-02-18 07:43_

---

_Closed by @MichaReiser on 2025-02-18 07:43_

---

_Branch deleted on 2025-02-18 07:43_

---
