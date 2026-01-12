```yaml
number: 2443
title: "Feature: warn when first-party module has the same name as third-party or builtin module"
type: issue
state: open
author: AndBoyS
labels:
  - lint
assignees: []
created_at: 2026-01-11T11:20:21Z
updated_at: 2026-01-11T17:01:46Z
url: https://github.com/astral-sh/ty/issues/2443
synced_at: 2026-01-12T15:54:26Z
```

# Feature: warn when first-party module has the same name as third-party or builtin module

---

_@AndBoyS_

When you have typing.py module defined in your code, ty will get confused (since typing is builtin). It can be difficult to debug the issue, so it would be nice to have a warning when this kind of shadowing occures

---

_Label `lint` added by @AlexWaygood on 2026-01-11 11:34_

---

_Comment by @AlexWaygood on 2026-01-11 14:25_

This does seem like a reasonable thing to offer a diagnostic for, since it's likely that we'll have confusing behaviour if a user does this accidentally. It might be noisy on some codebases, but I guess they can just suppress the diagnostic. It will probably only be useful if it's enabled by default.

One interesting question is: _how_ users would suppress this diagnostic? Would the diagnostic be issued on the first line of the incorrectly named file? If so, I guess they could just put a `ty: ignore[<error code>]` comment on the first line of the file.

---

_Comment by @MichaReiser on 2026-01-11 17:00_

Ideally it would be emitted without a range because there's no need to render a code frame. But yes, that might require some special casing on the suppression side or add support for file level ty:ignore

---

_Comment by @MichaReiser on 2026-01-11 17:01_

Ruff has a rule for this also

https://docs.astral.sh/ruff/rules/stdlib-module-shadowing/

---
