```yaml
number: 16307
title: Fix backtick escaping for PowerShell
type: pull_request
state: merged
author: jborean93
labels:
  - bug
assignees: []
merged: true
base: main
head: pwsh-backtick
created_at: 2025-10-15T04:03:00Z
updated_at: 2025-10-21T10:20:37Z
url: https://github.com/astral-sh/uv/pull/16307
synced_at: 2026-01-10T06:36:16Z
```

# Fix backtick escaping for PowerShell

---

_Pull request opened by @jborean93 on 2025-10-15 04:03_

## Summary
Fixes the logic for escaping a double quoted string in PowerShell by not escaping a backslash but escaping the Unicode double quote variants that PowerShell treats the same as the ASCII double quotes.

<img width="685" height="99" alt="image" src="https://github.com/user-attachments/assets/ac1368c2-d915-4e49-b67f-ac71ee0f7d46" />

## Test Plan
There does not seem to be any tests for this. I'm fairly new to rust but happy to add some if someone can point me in the right direction.

---

_Review requested from @Gankra by @konstin on 2025-10-15 10:52_

---

_Comment by @andersk on 2025-10-17 07:25_

Shouldn’t `` ` `` itself be escaped as ``` `` ``` too?

---

_Comment by @jborean93 on 2025-10-17 08:52_

It probably should yes but it would mean that anyone that has a newline literal or unicode escape sequence would now not work. I think in the context of where this is used this is probably ok and should be escaped.

---

_Review comment by @Gankra on `crates/uv-shell/src/lib.rs`:334 on 2025-10-20 14:42_

```suggestion
            '"' | '`' | '\u{201C}' | '\u{201D}' | '\u{201E}' | '$' => escaped.push('`'),
```

---

_@Gankra approved on 2025-10-20 14:49_

Dug through some docs and tried some examples and this all looks correct.

---

_Comment by @Gankra on 2025-10-20 14:54_

Other redflags that came up in research: 

If these were single-quote strings then `'`,  `‘` and `’` would similarly be problematic, but in double-quote strings they're not (and this function *only* works for double-quote strings (the ` escape is only for double-quote strings), and the user of this function *needs* double quote strings to actually expand an env-var in the string).

If this were a completely unquoted string (`echo hello`) then `#` would be a concern as it would introduce a comment. However it does not inside quoted strings.

---

_Label `bug` added by @konstin on 2025-10-20 14:57_

---

_Comment by @jborean93 on 2025-10-20 18:43_

Yea single quotes are easier to escape but in this case we are relying that this will be used in double quotes as we use `$env:PATH` in a value appended to this string. Thanks for sending through the backtick suggestion as well.

---

_Closed by @konstin on 2025-10-20 18:58_

---

_Reopened by @konstin on 2025-10-20 18:58_

---

_Merged by @konstin on 2025-10-21 10:16_

---

_Closed by @konstin on 2025-10-21 10:16_

---

_Branch deleted on 2025-10-21 10:20_

---
