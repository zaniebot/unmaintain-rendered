---
number: 13935
title: "f-string formatting: Consider format specs when choosing the preferred quotes"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
  - preview
assignees: []
created_at: 2024-10-26T08:05:03Z
updated_at: 2024-11-22T12:43:54Z
url: https://github.com/astral-sh/ruff/issues/13935
synced_at: 2026-01-07T13:12:16-06:00
---

# f-string formatting: Consider format specs when choosing the preferred quotes

---

_Issue opened by @MichaReiser on 2024-10-26 08:05_

The new preview style formats

```python
"" f'{1:""}'
```

as 

```python
f"{1:\"\"}"
```

This is correct but results in unnecessary escapes. I think we should account for the quotes in format-specs when choosing the preferred quotes for an f-string. 

This can be changed here https://github.com/astral-sh/ruff/blob/113ce840a690cd59acbaf7d40d70b4352df681fe/crates/ruff_python_formatter/src/string/normalize.rs#L264-L295

CC: @dhruvmanila 

---

_Label `formatter` added by @MichaReiser on 2024-10-26 08:05_

---

_Label `help wanted` added by @MichaReiser on 2024-10-26 08:05_

---

_Label `preview` added by @MichaReiser on 2024-10-26 08:05_

---

_Comment by @dhruvmanila on 2024-10-28 07:29_

I don't think we can do that because it would change the value of the format spec itself: https://play.ruff.rs/baf27789-023c-4803-aa8d-e933cc616f0f (refer to the AST).

Note that those quotes are not the surrounding quotes of a literal element but the quotes itself are _part_ of it.

---

_Comment by @dhruvmanila on 2024-10-28 07:30_

Oh wait, I think I misunderstood. Do you mean we should account for the possible quotes inside a format spec, thus choosing single quote for this specific example?

---

_Comment by @MichaReiser on 2024-10-28 07:35_

> Oh wait, I think I misunderstood. Do you mean we should account for the possible quotes inside a format spec, thus choosing single quote for this specific example?

Yes. We should use single quotes for the outer f-string because it avoids escaping the format-spec (unless the f-string contains more single quotes outside the format spec)

---

_Referenced in [astral-sh/ruff#14493](../../astral-sh/ruff/pulls/14493.md) on 2024-11-20 15:49_

---

_Closed by @MichaReiser on 2024-11-22 12:43_

---
