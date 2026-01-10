```yaml
number: 1451
title: "Go-to definition works for unimported symbols if they are imported in `builtins.pyi`"
type: issue
state: closed
author: MatthewMckee4
labels:
  - bug
  - server
assignees: []
created_at: 2025-10-29T13:24:15Z
updated_at: 2025-10-29T18:39:37Z
url: https://github.com/astral-sh/ty/issues/1451
synced_at: 2026-01-10T02:06:25Z
```

# Go-to definition works for unimported symbols if they are imported in `builtins.pyi`

---

_Issue opened by @MatthewMckee4 on 2025-10-29 13:24_

### Summary

A small example here https://play.ty.dev/823bed81-e07f-4446-9be8-51ec51f6c38c.

We can ctrl click and f12 on `TracebackType`, but we can't on `bar`. This feels unintuitive.

Perhaps this is the intended outcome?

### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-10-29 13:27_

---

_Label `server` added by @MichaReiser on 2025-10-29 13:27_

---

_Comment by @MichaReiser on 2025-10-29 13:28_

Thanks. I'm surprised you can click on `TracebackType` as there's no definition for it. We should probably fix this (unless the symbol is available for some reasons unknown to me)

---

_Comment by @sharkdp on 2025-10-29 13:54_

Maybe this only works because `TracebackType` is imported in `builtins`?

https://github.com/python/typeshed/blob/3c5531d49213cab3a1f0e172259c9c7650c2d6a6/stdlib/builtins.pyi#L36

---

_Comment by @carljm on 2025-10-29 15:39_

Yes, `GeneratorType` for example does not goto-def when not imported, and is not imported in `builtins.pyi`. So I think this is a case where the builtins-lookup logic for goto-def does not implement the re-import convention for stub files, and it needs to do so, in order to avoid wrongly treating many names as builtins.

---

_Renamed from "Go-to definition works for unimported symbols" to "Go-to definition works for unimported symbols if they are imported in `builtins.pyi`" by @carljm on 2025-10-29 15:40_

---

_Closed by @MichaReiser on 2025-10-29 18:39_

---
