```yaml
number: 4865
title: F523 should not be marked as always-fixable
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-05T14:28:17Z
updated_at: 2023-06-06T06:06:03Z
url: https://github.com/astral-sh/ruff/issues/4865
synced_at: 2026-01-10T11:09:47Z
```

# F523 should not be marked as always-fixable

---

_Issue opened by @addisoncrump on 2023-06-05 14:28_

It is possible to split a format invocation across multiple lines:

```py
(''
.format(2))
```

This makes ruff a bit mad:

```
error: Failed to create fix for StringDotFormatExtraPositionalArguments: Failed to extract expression from source
fuzz/artifacts/ruff_fix_validity/minimized-from-f670b0a6b2f154ee6d66e9ad4f601cab0b3d4609:1:2: F523 `.format` call has unused arguments at position(s): 0
Found 1 error.
```

Discovered by #4822.

---

_Comment by @charliermarsh on 2023-06-05 15:49_

I believe this is a known issue in LibCST (that we filed): https://github.com/Instagram/LibCST/issues/846. Going to close since we're mostly just waiting for it to be resolved upstream.

---

_Closed by @charliermarsh on 2023-06-05 15:50_

---

_Label `bug` added by @charliermarsh on 2023-06-05 15:50_

---

_Comment by @addisoncrump on 2023-06-05 15:57_

Hm, okay. I will need to filter out this particular case in the fuzzers (to prevent the panic induced in code I didn't write :sweat_smile:).

---

_Comment by @charliermarsh on 2023-06-05 15:59_

Gahhh haha sorry! That error is caught and handled, right? It doesn't actually panic, does it? It's _supposed_ to be handled and logged to the user.

---

_Comment by @addisoncrump on 2023-06-05 16:31_

Yes, but when using the test suite functions, this panics (it is marked as always-fixable).

---

_Comment by @charliermarsh on 2023-06-05 16:34_

Oh, we should change it to "sometimes".

---

_Comment by @addisoncrump on 2023-06-05 16:43_

So, should we reopen with different title?

---

_Comment by @charliermarsh on 2023-06-05 16:44_

I'll just fix it real quick.

---

_Closed by @charliermarsh on 2023-06-05 16:55_

---

_Renamed from "F523: Newline between string and format invocation triggers extraction error" to "F523 should not be marked as AlwaysFixed" by @addisoncrump on 2023-06-05 17:09_

---

_Renamed from "F523 should not be marked as AlwaysFixed" to "F523 should not be marked as always-fixable" by @addisoncrump on 2023-06-06 06:05_

---
