```yaml
number: 7293
title: "Update `Stylist` quote detection to use the new f-string tokens"
type: issue
state: closed
author: dhruvmanila
labels:
  - core
  - python312
assignees: []
created_at: 2023-09-12T09:43:04Z
updated_at: 2023-09-15T07:32:24Z
url: https://github.com/astral-sh/ruff/issues/7293
synced_at: 2026-01-10T11:09:49Z
```

# Update `Stylist` quote detection to use the new f-string tokens

---

_Issue opened by @dhruvmanila on 2023-09-12 09:43_

Update the [`detect_quote`](https://github.com/astral-sh/ruff/blob/a4a4616f0a49c6b1ff7da93a71308d897d27d21f/crates/ruff_python_codegen/src/stylist.rs#L52-L52) function to either use the `String` or `FStringStart` token to detect the quote (single or double).

This should probably be enough as the `FStringStart` token contains the prefix and quotes for the f-string:

```diff
    let quote_range = tokens.iter().flatten().find_map(|(t, range)| match t {
        Tok::String {
            triple_quoted: false,
            ..
-        } => Some(*range),
+        }
+        | Tok::FStringStart => Some(*range),
        _ => None,
    });
```

---

_Label `rule` added by @dhruvmanila on 2023-09-12 09:46_

---

_Label `python312` added by @dhruvmanila on 2023-09-12 09:46_

---

_Label `rule` removed by @dhruvmanila on 2023-09-12 09:47_

---

_Label `core` added by @dhruvmanila on 2023-09-12 09:47_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-13 04:57_

---

_Closed by @dhruvmanila on 2023-09-15 07:32_

---
