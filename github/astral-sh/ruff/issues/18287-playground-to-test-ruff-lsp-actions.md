---
number: 18287
title: Playground to test ruff lsp actions?
type: issue
state: closed
author: kaddkaka
labels:
  - question
assignees: []
created_at: 2025-05-24T16:04:06Z
updated_at: 2025-05-24T16:27:39Z
url: https://github.com/astral-sh/ruff/issues/18287
synced_at: 2026-01-07T13:12:16-06:00
---

# Playground to test ruff lsp actions?

---

_Issue opened by @kaddkaka on 2025-05-24 16:04_

### Question

Hi, is it possible to test Ruff's LSP actions in the playground (https://play.ruff.rs/) or somewhere else?

I have a quite old version of ruff on my company workstation, and it would simplify to send in issues if I could easily try out the behavior in newest version.

### Version

_No response_

---

_Label `question` added by @kaddkaka on 2025-05-24 16:04_

---

_Comment by @MichaReiser on 2025-05-24 16:09_

Can you tell me more what you mean by lsp actions?

The playground shows diagnostics, you can apply fixes and see tge formatted code

---

_Comment by @kaddkaka on 2025-05-24 16:16_

Locally with ruff 0.11.3, and this code:

```py
def a(max_width):
    if max_width < 32:    # <-- this line
        # Bananas
        max_width = 32
```

On the marked line I see these diagnostics:
```
Diagnostics:
1. Ruff: Replace `if` statement with `max_width = max(max_width, 32)` [PLR1730]
2. pylint: [consider-using-max-builtin] Consider using 'max_width = max(max_width, 32)' instead of unnecessary if block [R1731]
```

On the same line I have these actions available:
```
Code actions:
1: Ruff (PLR1730): Replace with `max_width = max(max_width, 32)` [ruff]
2: Ruff (PLR1730): Disable for this line [ruff]
3: Ruff: Fix all auto-fixable problems [ruff]
4: Ruff: Organize imports [ruff]
```

I would like to try code action 1 in the playground. Because when I do that I get this broken result:
```py
def a(max_width):
    max_width = max(max_width, 32)
```

the comment is gone! 😮 

---

_Comment by @kaddkaka on 2025-05-24 16:18_

When I paste the above code into the playground I don't see any diagnostics or where/how to apply fixes.

---

_Comment by @MichaReiser on 2025-05-24 16:20_

The playground supports this but it doesn't know about your configuration. You can switch to the settings by clicking on the gear icon. Then paste your toml configuration 

---

_Comment by @kaddkaka on 2025-05-24 16:24_

Thanks 👍 The problematic behavior statys in 0.11.11, I'll make an issue

---

_Closed by @kaddkaka on 2025-05-24 16:27_

---
