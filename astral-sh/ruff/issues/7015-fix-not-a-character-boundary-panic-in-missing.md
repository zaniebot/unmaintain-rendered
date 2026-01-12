```yaml
number: 7015
title: "Fix \"not a character boundary\" panic in `missing_copyright_notice`"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2023-08-31T06:50:32Z
updated_at: 2023-09-02T07:54:29Z
url: https://github.com/astral-sh/ruff/issues/7015
synced_at: 2026-01-12T15:54:46Z
```

# Fix "not a character boundary" panic in `missing_copyright_notice`

---

_@MichaReiser_

The missing copyright notice only looks at the 1024 first bytes. However, this can go wrong for unicode text if the 1024 byte is not a character boundary (because it is a multibyte character).

https://github.com/astral-sh/ruff/blob/2cf00fee96eedaf061d1c8578d2d479eeb65658a/crates/ruff/src/rules/flake8_copyright/rules/missing_copyright_notice.rs#L36-L40

Fix: Use `is_char_boundary` to find a valid char boundary close to 1024.

```
locator.contents().is_char_boundary(1024)
```

---

_Label `bug` added by @MichaReiser on 2023-08-31 06:50_

---

_Label `good first issue` added by @MichaReiser on 2023-08-31 06:50_

---

_Label `help wanted` added by @MichaReiser on 2023-08-31 06:50_

---

_Renamed from "Fix potential "not a character boundary" panic in `missing_copyright_notice`" to "Fix "not a character boundary" panic in `missing_copyright_notice`" by @MichaReiser on 2023-08-31 06:50_

---

_Comment by @WindowGenerator on 2023-08-31 09:39_

@MichaReiser Hello. Can I solve this bug?

---

_Assigned to @WindowGenerator by @MichaReiser on 2023-08-31 10:04_

---

_Comment by @MichaReiser on 2023-08-31 16:12_

@WindowGenerator sure. #7012 or #7027 both contain a code snipped that triggers the error and you may want to use as a test case.

---

_Comment by @qarmin on 2023-09-02 05:58_

This is fixed and can be closed

---

_Closed by @MichaReiser on 2023-09-02 07:54_

---
