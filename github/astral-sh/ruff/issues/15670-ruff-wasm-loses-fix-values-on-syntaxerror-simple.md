---
number: 15670
title: "ruff_wasm loses 'fix' values on 'SyntaxError: Simple statements must be separated by newlines or semicolons' error"
type: issue
state: closed
author: lkociko
labels:
  - question
assignees: []
created_at: 2025-01-22T11:20:20Z
updated_at: 2025-01-23T08:51:58Z
url: https://github.com/astral-sh/ruff/issues/15670
synced_at: 2026-01-07T13:12:16-06:00
---

# ruff_wasm loses 'fix' values on 'SyntaxError: Simple statements must be separated by newlines or semicolons' error

---

_Issue opened by @lkociko on 2025-01-22 11:20_

### Description

**Version:** WASM 0.9.2

I am using `ruff_wasm` inside my React application. The suggested fixes become undefined, and the reported issues are inconsistent.

Example:
If I have 2 issues, errors W291 & F841:

```json
[
  {
    "code": "F841",
    "message": "Local variable `notUsedVariable` is assigned to but never used",
    "location": {
      "row": 17,
      "column": 5
    },
    "end_location": {
      "row": 17,
      "column": 20
    },
    "fix": {
      "message": "Remove assignment to unused variable `notUsedVariable`",
      "edits": [
        {
          "location": {
            "row": 17,
            "column": 1
          },
          "end_location": {
            "row": 18,
            "column": 1
          }
        }
      ]
    }
  },
  {
    "code": "W291",
    "message": "Trailing whitespace",
    "location": {
      "row": 7,
      "column": 52
    },
    "end_location": {
      "row": 7,
      "column": 53
    },
    "fix": {
      "message": "Remove trailing whitespace",
      "edits": [
        {
          "location": {
            "row": 7,
            "column": 52
          },
          "end_location": {
            "row": 7,
            "column": 53
          }
        }
      ]
    }
  }
]
```

When I introduce an error `SyntaxError: Simple statements must be separated by newlines or semicolons`, for example `return "Something"a`, now the issues are 2 (**the F841 error disappeared**), and **the fixes are now undefined**.

```json
[
  {
    "code": "W291",
    "message": "Trailing whitespace",
    "location": {
      "row": 7,
      "column": 52
    },
    "end_location": {
      "row": 7,
      "column": 53
    }
  },
  {
    "message": "SyntaxError: Simple statements must be separated by newlines or semicolons",
    "location": {
      "row": 18,
      "column": 35
    },
    "end_location": {
      "row": 18,
      "column": 36
    }
  }
]
```


---

_Comment by @MichaReiser on 2025-01-22 13:36_

This behavior matches Ruff's normal behavior. Most most lint rules don't run if there's a syntax error and Ruff doesn't provide fixes for syntax errors. 

So this is working as expected. We intent to support linting files with syntax errors in the future but it isn't a priority for now.



---

_Closed by @MichaReiser on 2025-01-22 13:36_

---

_Label `question` added by @MichaReiser on 2025-01-22 13:36_

---

_Comment by @lkociko on 2025-01-23 08:40_

Hi @MichaReiser, thank you for your reply. 

We are currently using the initial ruff_wasm version (16 months old), and it behaves differently

When syntax error is introduced, the W291 warning maintains the fix value
```
[
    {
        "code": "E999",
        "message": "SyntaxError: Unexpected token 'a'",
        "location": {
            "row": 18,
            "column": 35
        },
        "end_location": {
            "row": 18,
            "column": 36
        }
    },
    {
        "code": "W291",
        "message": "Trailing whitespace",
        "location": {
            "row": 7,
            "column": 52
        },
        "end_location": {
            "row": 7,
            "column": 53
        },
        "fix": {
            "message": "Remove trailing whitespace",
            "edits": [
                {
                    "location": {
                        "row": 7,
                        "column": 52
                    },
                    "end_location": {
                        "row": 7,
                        "column": 53
                    }
                }
            ]
        }
    }
]
```
So to clarify, this the expected behaviour for version 0.9.2, right?

---

_Comment by @MichaReiser on 2025-01-23 08:51_

Yes, this was changed with https://github.com/astral-sh/ruff/pull/12134

Overall, changes to when Ruff emits fixes is something we might decide to change over time (is not part of our semver API stability)

---
