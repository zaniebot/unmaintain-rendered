```yaml
number: 18673
title: "Fix `\\r` and `\\r\\n` handling in t- and f-string debug texts"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/debug-text-newline
created_at: 2025-06-14T05:52:22Z
updated_at: 2025-06-15T05:53:08Z
url: https://github.com/astral-sh/ruff/pull/18673
synced_at: 2026-01-10T18:45:04Z
```

# Fix `\r` and `\r\n` handling in t- and f-string debug texts

---

_Pull request opened by @MichaReiser on 2025-06-14 05:52_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/18667

The `Printer` only supports `\n`. We, therefore, need to normalize the newlines in debug texts during formatting. 

Normalizing the newlines doesn't change the Python semantics because Python normalizes all newlines to `\n` 



## Test Plan

Added snapshot test


---

_Label `bug` added by @MichaReiser on 2025-06-14 05:52_

---

_Label `formatter` added by @MichaReiser on 2025-06-14 05:52_

---

_Comment by @github-actions[bot] on 2025-06-14 05:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@ntBre approved on 2025-06-14 15:32_

Nice! This makes sense to me.

---

_Comment by @MichaReiser on 2025-06-15 05:53_

I was a bit confused why this would lead to duplicate lines but I think I understand it now:

1. We infer `\r\n` as the used line ending because it's the first we see 
2. The [printer replaces every `\n` with `\r\n`](https://github.com/astral-sh/ruff/blob/793ff9bdbc2ad4909f45594726ee0d2ff166130c/crates/ruff_formatter/src/printer/mod.rs#L820-L831) which results in `\r\r\n` after the first formatting pass 

Now we have two line breaks. The printer will now infer `\r` as the line ending, and replace the `\n` with `\r`, now we have three line endings. The editor might also decide to convert the `\r` to `\r\n`, in which case this goes on forever

---

_Merged by @MichaReiser on 2025-06-15 05:53_

---

_Closed by @MichaReiser on 2025-06-15 05:53_

---

_Branch deleted on 2025-06-15 05:53_

---
