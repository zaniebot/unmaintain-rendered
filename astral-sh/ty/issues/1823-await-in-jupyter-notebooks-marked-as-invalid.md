```yaml
number: 1823
title: await in jupyter notebooks marked as invalid-syntax
type: issue
state: open
author: bvolkmer
labels:
  - needs-info
  - notebook
assignees: []
created_at: 2025-12-09T11:31:07Z
updated_at: 2025-12-14T16:00:24Z
url: https://github.com/astral-sh/ty/issues/1823
synced_at: 2026-01-12T15:54:25Z
```

# await in jupyter notebooks marked as invalid-syntax

---

_@bvolkmer_

### Summary

`await` statements are flagged as `error[invalid-syntax]: `await` outside of an asynchronous function` even though it is allowed in Jupyter cells.

### Version

ty 0.0.1-alpha.32

---

_Comment by @sharkdp on 2025-12-09 11:43_

FYI @ntBre 

---

_Comment by @dhruvmanila on 2025-12-09 11:58_

Related: https://github.com/astral-sh/ruff/issues/17395

---

_Label `notebook` added by @dhruvmanila on 2025-12-09 11:58_

---

_Comment by @MichaReiser on 2025-12-09 12:05_

The easiest for now is probably to just skip this check based on the parsing mode

---

_Comment by @bvolkmer on 2025-12-09 12:34_

Also, `invalid-syntax` is not a rule that can be ignored in the config:
```
warning[unknown-rule]: Unknown rule `invalid-syntax`
 --> ty.toml:2:1
  |
1 | [rules]
2 | invalid-syntax = "ignore"
  | ^^^^^^^^^^^^^^
  |
```
Edit: fixed underscore typo, still applies

---

_Assigned to @ntBre by @ntBre on 2025-12-09 12:50_

---

_Comment by @MichaReiser on 2025-12-09 13:07_

> Also, invalid-syntax is not a rule that can be ignored in the config:

This is intentional as these should, except when a bug in ty or ruff, always be fixed.

---

_Comment by @ntBre on 2025-12-09 13:35_

Could you share an example of your code? There should already be an exception to the rule for await in the module scope/at the top level of a cell:

https://github.com/astral-sh/ruff/blob/e8540d9b085980bef22cc8e1d4c6e75531cf140c/crates/ruff_python_parser/src/semantic_errors.rs#L930-L934

In a notebook with this code:

```py
import asyncio

await asyncio.sleep(1)

if True:
    await asyncio.sleep(1)

def foo():
    await asyncio.sleep(1)
```

I only see an error for the last case, which seems correct to me.

```shell
❯ ty check crates/ruff_linter/resources/test/fixtures/pylint/await_outside_async.ipynb
error[invalid-syntax]: `await` outside of an asynchronous function
 --> crates/ruff_linter/resources/test/fixtures/pylint/await_outside_async.ipynb:cell 1:9:5
  |
8 | def foo():
9 |     await asyncio.sleep(1)  # # [await-outside-async]
  |     ^^^^^^^^^^^^^^^^^^^^^^
  |

Found 1 diagnostic
```

---

_Label `needs-info` added by @AlexWaygood on 2025-12-14 16:00_

---
