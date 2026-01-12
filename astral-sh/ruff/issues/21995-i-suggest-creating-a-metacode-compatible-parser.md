```yaml
number: 21995
title: I suggest creating a metacode-compatible parser
type: issue
state: open
author: pomponchik
labels:
  - question
  - needs-info
assignees: []
created_at: 2025-12-16T03:10:46Z
updated_at: 2025-12-18T22:35:52Z
url: https://github.com/astral-sh/ruff/issues/21995
synced_at: 2026-01-12T15:54:58Z
```

# I suggest creating a metacode-compatible parser

---

_@pomponchik_

### Question

Hi!

I recently created a comment parser that can be used in any linters and formatters: [`metacode`](https://github.com/pomponchik/metacode). I suggest this as a convenient standard, since different tools are now forced to implement such parsers from scratch. And I suggest that ruff, as the flagship of all python linters, support this standard.

Given that Ruff is not written in Python, I suggest implementing a separate [grammar](https://github.com/pomponchik/metacode?tab=readme-ov-file#what-about-other-languages)-based mini-parser using Rust.

### Version

_No response_

---

_Label `question` added by @pomponchik on 2025-12-16 03:10_

---

_Comment by @MichaReiser on 2025-12-16 07:23_

Can you say more about what the benefits are for ruff? We don't need a "can parse all pragma comments" parser, we only need to parse the few comments supported by Ruff. Or are there cases where Ruff's parser doesn't follow your meta grammar?

---

_Label `needs-info` added by @AlexWaygood on 2025-12-18 22:35_

---
