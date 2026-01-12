```yaml
number: 2105
title: Manual type hints should override inferred type
type: issue
state: closed
author: mattangus
labels: []
assignees: []
created_at: 2025-12-19T09:08:38Z
updated_at: 2025-12-19T09:11:05Z
url: https://github.com/astral-sh/ty/issues/2105
synced_at: 2026-01-12T15:54:26Z
```

# Manual type hints should override inferred type

---

_@mattangus_

### Summary

When the type is not know by `ty` the user should be able to override the type with type hints. This is useful for example when using a package that has poor type hinting. This snippet relies on the fact that `ty` currently doesn't infer return types, but the point still stands for any `Unknown` type.

```
def fn():
    return 1

r: int = fn()

reveal_type(r) # Revealed type: `Unknown`
```

Here is the [ty playground](https://play.ty.dev/cd3436b6-1c9d-4695-b2d2-0481f5935f36) and [mypy playground](https://mypy-play.net/?mypy=latest&python=3.12&gist=aaf539f6c00140df4733470f55bb06e7) for this snippet.

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-12-19 09:11_

Thanks, please see #136

---

_Closed by @AlexWaygood on 2025-12-19 09:11_

---
