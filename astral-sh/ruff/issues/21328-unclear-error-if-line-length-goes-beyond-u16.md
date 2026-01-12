```yaml
number: 21328
title: Unclear error if line-length goes beyond u16 boundaries
type: issue
state: closed
author: chirizxc
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2025-11-07T22:44:37Z
updated_at: 2025-11-10T18:29:37Z
url: https://github.com/astral-sh/ruff/issues/21328
synced_at: 2026-01-12T15:54:57Z
```

# Unclear error if line-length goes beyond u16 boundaries

---

_@chirizxc_

### Summary
Refs https://github.com/astral-sh/ruff/issues/20823

ruff.toml:
```toml
line-length = 99_999
```
```
❯ ruff check
ruff failed                                                                                                                                           
  Cause: Failed to load configuration `...`
  Cause: Failed to parse ...
  Cause: TOML parse error at line 3, column 15
  |
3 | line-length = 99_999
  |               ^^^^^^
invalid value: integer `99999`, expected u16
```

From [TOML spec](https://toml.io/en/v1.0.0#integer):
> Arbitrary 64-bit signed integers (from −2^63 to 2^63−1) should be accepted and handled losslessly. If an integer cannot be represented losslessly, an error must be thrown.

```shell
❯ bat ruff.toml                                                                                                                                       
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: ruff.toml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ target-version = "py310"
   2   │ 
   3 ~ │ line-length = 65535
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ ruff check
ruff failed                                                                                                                                           
  Cause: Failed to load configuration `...`
  Cause: Failed to parse ...
  Cause: TOML parse error at line 3, column 15
  |
3 | line-length = 65535
  |               ^^^^^
line-length must be between 1 and 320 (got 65535)
```
and
```shell
❯ bat ruff.toml                                                                                                                                       
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: ruff.toml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ target-version = "py310"
   2   │ 
   3 ~ │ line-length = 65536
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ ruff check
ruff failed                                                                                                                                           
  Cause: Failed to load configuration `...`
  Cause: Failed to parse ...
  Cause: TOML parse error at line 3, column 15
  |
3 | line-length = 65536
  |               ^^^^^
invalid value: integer `65536`, expected u16
```
### Version

`ruff 0.14.4 (c7ff9826d 2025-11-06)`

---

_Label `good first issue` added by @MichaReiser on 2025-11-07 22:58_

---

_Label `configuration` added by @MichaReiser on 2025-11-07 22:58_

---

_Closed by @MichaReiser on 2025-11-10 18:29_

---
