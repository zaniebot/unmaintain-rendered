```yaml
number: 2495
title: "Handling of `type: ignore[code]` comments as blanket `type: ignore`"
type: issue
state: open
author: MichaReiser
labels:
  - suppression
assignees: []
created_at: 2026-01-14T16:06:15Z
updated_at: 2026-01-14T16:15:51Z
url: https://github.com/astral-sh/ty/issues/2495
synced_at: 2026-01-14T16:38:48Z
```

# Handling of `type: ignore[code]` comments as blanket `type: ignore`

---

_@MichaReiser_

> (Somewhat separately, I do wonder about our current behavior of interpreting any type: ignore with an unrecognized code as a blanket type: ignore. It seems unlikely to me that that will be the desired behavior for anyone -- but I'm also not sure what would be a better behavior.)
> First posted by @carljm in https://github.com/astral-sh/ty/issues/2494#issuecomment-3750267012



---

_Label `suppression` added by @MichaReiser on 2026-01-14 16:06_

---

_Renamed from "Handling of `type: ignore[code]` comments as blanked `type: ignore`" to "Handling of `type: ignore[code]` comments as blanket `type: ignore`" by @carljm on 2026-01-14 16:07_

---

_Comment by @MichaReiser on 2026-01-14 16:15_

I agree that it's somewhat questionable but it does match pyright's behavior. There's also the argument, but not a very strong one, that the PEP only [specifies](https://typing.python.org/en/latest/spec/directives.html#type-ignore-comments) that the comment starts with `type: ignore`. To my knowledge, `type: ignore[code]` is a mypy invention. 

I think there's an argument to always ignore `type: ignore[]` comments, since they're obviously meant for mypy, not for ty. That might be annoying in the LSP use case when you use ty with a mypy project that requires `type: ignore[code]`. 

I'm leaning towards leaving it as is. The user wants to suppress a type-check diagnostic on that line. Unfortunately, ty doesn't understand which specific one it is, but ty should honor the user's intent to suppress a diagnostic on that line.

---
