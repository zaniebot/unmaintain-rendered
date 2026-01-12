```yaml
number: 17276
title: "[red-knot] Incorrect syntax error persistent in playground"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - playground
  - ty
assignees: []
created_at: 2025-04-07T14:05:42Z
updated_at: 2025-04-07T16:47:00Z
url: https://github.com/astral-sh/ruff/issues/17276
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] Incorrect syntax error persistent in playground

---

_@sharkdp_

This is something that happened to me a few times. It's always the same situation. I add a return type annotation by typing ` -> …` after a function definition, and then I'm left with a syntax error that shouldn't be there:

![Image](https://github.com/user-attachments/assets/65eb4de3-2ca1-419b-ab07-07c82db6a647)

How to reproduce:
- Go to https://playknot.ruff.rs/ffc63887-bdf3-4471-a4be-097a2f047e2b
- Add an *incorrect* return annotation (by typing), for example: ` -> str`
- Observe incorrect syntax error `Expected ':', found '-' (invalid-syntax)`

Seems to be related to there being *another* diagnostic as well. If you type `-> int`, it works fine. If you do random other changes to that file, the error often goes away. For example, if you select all text, cut it, and paste it back in, the error is gone.

---

_Label `playground` added by @sharkdp on 2025-04-07 14:05_

---

_Label `red-knot` added by @sharkdp on 2025-04-07 14:05_

---

_Label `bug` added by @AlexWaygood on 2025-04-07 14:57_

---

_Comment by @MichaReiser on 2025-04-07 16:18_

It's very specific about adding `-` when typing the return type. The problem is that the parser generates 6(!) diagnostics, all starting at the very same position

```json
[
  {
    "id": "invalid-syntax",
    "message": "Expected ':', found '-'",
    "severity": 2,
    "range": {
      "__wbg_ptr": 4924664
    },
    "textRange": {
      "__wbg_ptr": 4924816
    }
  },
  {
    "id": "invalid-syntax",
    "message": "Invalid annotated assignment target",
    "severity": 2,
    "range": {
      "__wbg_ptr": 4924840
    },
    "textRange": {
      "__wbg_ptr": 4924872
    }
  },
  {
    "id": "invalid-syntax",
    "message": "Expected an expression",
    "severity": 2,
    "range": {
      "__wbg_ptr": 4924896
    },
    "textRange": {
      "__wbg_ptr": 4924928
    }
  },
  {
    "id": "invalid-syntax",
    "message": "Expected an expression",
    "severity": 2,
    "range": {
      "__wbg_ptr": 4924952
    },
    "textRange": {
      "__wbg_ptr": 4924984
    }
  },
  {
    "id": "invalid-syntax",
    "message": "Unexpected indentation",
    "severity": 2,
    "range": {
      "__wbg_ptr": 4925008
    },
    "textRange": {
      "__wbg_ptr": 4925040
    }
  },
  {
    "id": "invalid-syntax",
    "message": "Expected a statement",
    "severity": 2,
    "range": {
      "__wbg_ptr": 4925064
    },
    "textRange": {
      "__wbg_ptr": 4925096
    }
  }
]
```

This seems like something we should improve in the parser but, obviously, this shouldn't break the playground. 

The reason this breaks the playground is because I use `{startLine}:{startColumn}-{id}` as a unique identifier, which obviously isn't as unique as I thought... 

---

_Closed by @MichaReiser on 2025-04-07 16:39_

---

_Closed by @MichaReiser on 2025-04-07 16:39_

---

_Added to milestone `Red Knot Alpha` by @AlexWaygood on 2025-04-07 16:47_

---
