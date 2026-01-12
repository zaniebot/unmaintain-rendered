```yaml
number: 22491
title: Delete bad and good.txt
type: pull_request
state: closed
author: MichaReiser
labels:
  - testing
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: micha/bye-bye-bad-txt
created_at: 2026-01-10T13:05:05Z
updated_at: 2026-01-10T13:21:00Z
url: https://github.com/astral-sh/ruff/pull/22491
synced_at: 2026-01-12T15:57:50Z
```

# Delete bad and good.txt

---

_@MichaReiser_

## Summary

The projects now pass locally. 

I'm sure this will be more annoying than just deleting the files :)

## Test Plan

<!-- How was it tested? -->


---

_Label `testing` added by @MichaReiser on 2026-01-10 13:05_

---

_Comment by @astral-sh-bot[bot] on 2026-01-10 13:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ecosystem-analyzer` added by @MichaReiser on 2026-01-10 13:08_

---

_Comment by @MichaReiser on 2026-01-10 13:20_

Oh no. Looks like `steam.py` still panics in CI :(

---

_Closed by @MichaReiser on 2026-01-10 13:21_

---
