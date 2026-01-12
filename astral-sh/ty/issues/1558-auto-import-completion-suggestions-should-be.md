```yaml
number: 1558
title: "Auto-import completion suggestions should be ranked below other suggestions if the symbol is `@type_check_only`"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - typing semantics
  - completions
assignees: []
created_at: 2025-11-14T16:34:25Z
updated_at: 2025-11-17T16:35:36Z
url: https://github.com/astral-sh/ty/issues/1558
synced_at: 2026-01-12T15:54:25Z
```

# Auto-import completion suggestions should be ranked below other suggestions if the symbol is `@type_check_only`

---

_@AlexWaygood_

https://github.com/astral-sh/ruff/pull/20910 implemented a feature where `@type_check_only` symbols are ranked below other symbols in completion suggestions. However, this was implemented only for symbols in the same file -- and this has limited usefulness, since generally if your cursor in a `.py` file, most symbols in your file are not going to be marked as `@type_check_only`. It's a feature that's used much more often in stubs. Ideally, we would also rank auto-import suggestions below other auto-import suggestions if the relevant symbols are `@type_check_only`.

This is a bit of a hard problem, however, as we cannot eagerly infer the type of all auto-import suggestions. Possibly we can lazily infer the types of these symbols, but even that might be too much of a performance hit. If we can't use the type checker at all for this information, we may have to evaluate whether implementing this feature is worth the complexity.

---

_Label `typing semantics` added by @AlexWaygood on 2025-11-14 16:34_

---

_Label `completions` added by @AlexWaygood on 2025-11-14 16:34_

---

_Label `server` added by @AlexWaygood on 2025-11-14 17:03_

---

_Comment by @BurntSushi on 2025-11-17 16:35_

> If we can't use the type checker at all for this information, we may have to evaluate whether implementing this feature is worth the complexity.

If [a lot of uses are trivial](https://github.com/search?q=repo%3Apython%2Ftypeshed%20type_check_only&type=code), then it shouldn't be too hard for the auto-import symbol scanner to pick it up in most cases. The failure mode, I think, is that there are some cases where we don't correctly recognize it (either false positive or false negative) and thus rank that symbol higher (false negative) or lower (false positive) than we otherwise would. That might be an acceptable trade-off here.

---
