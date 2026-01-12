```yaml
number: 18885
title: "Rule request: `typing_extensions` import detected outside of `TYPE_CHECKING` block"
type: issue
state: closed
author: PerchunPak
labels:
  - question
assignees: []
created_at: 2025-06-23T09:22:10Z
updated_at: 2025-06-23T11:44:28Z
url: https://github.com/astral-sh/ruff/issues/18885
synced_at: 2026-01-12T15:54:56Z
```

# Rule request: `typing_extensions` import detected outside of `TYPE_CHECKING` block

---

_@PerchunPak_

### Summary

It would be nice to add such a rule disabled by default. My use case is that I am a maintainer of a library, and we have decided that types cannot be a runtime dependency. So our users usually don't have it installed. It will help us prevent such bugs as https://github.com/py-mine/mcstatus/issues/1017

---

_Comment by @MichaReiser on 2025-06-23 11:08_

I think you can already use [`banned-module-level-imports`](https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_banned-module-level-imports) to accomplish this, see https://play.ruff.rs/435e3b87-6316-4ddf-9685-bd5262f3f274



---

_Label `question` added by @MichaReiser on 2025-06-23 11:09_

---

_Comment by @PerchunPak on 2025-06-23 11:42_

> I think you can already use [`banned-module-level-imports`](https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_banned-module-level-imports) to accomplish this, see [play.ruff.rs/435e3b87-6316-4ddf-9685-bd5262f3f274](https://play.ruff.rs/435e3b87-6316-4ddf-9685-bd5262f3f274)

Indeed, thanks! This is resolved, unless you think its worth adding a rule with a more specific error message and a wiki page with "Why is this wrong"

---

_Closed by @MichaReiser on 2025-06-23 11:44_

---
