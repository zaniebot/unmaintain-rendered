```yaml
number: 325
title: "server: Add link to rule documentation"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-05-12T10:40:13Z
updated_at: 2025-05-17T17:28:01Z
url: https://github.com/astral-sh/ty/issues/325
synced_at: 2026-01-10T02:34:09Z
```

# server: Add link to rule documentation

---

_Issue opened by @MichaReiser on 2025-05-12 10:40_

We should add a `codeDescription` for all diagnostics with a `LintName` with an URL pointing to the corresponding anchor in https://github.com/astral-sh/ty/blob/main/docs/rules.md#call-non-callable



---

_Label `help wanted` added by @MichaReiser on 2025-05-12 10:40_

---

_Label `server` added by @MichaReiser on 2025-05-12 10:40_

---

_Comment by @kiran-4444 on 2025-05-14 06:47_

This makes things pretty comfortable, I'd like to take this up.

---

_Comment by @MichaReiser on 2025-05-14 06:49_

You can use `https://ty.dev/rules` instead of using the full github url

---

_Comment by @kiran-4444 on 2025-05-14 11:33_

> You can use `https://ty.dev/rules` instead of using the full github url

Understood. Can you assign this to me so that I won't lose track of it? Thanks!

---

_Assigned to @kiran-4444 by @MichaReiser on 2025-05-14 11:55_

---

_Comment by @kiran-4444 on 2025-05-16 06:22_

How do I test these changes? I did:

```bash
cargo run --bin ty server
```

How to I get messages from this and see if my changes are getting returned? 

---

_Comment by @MichaReiser on 2025-05-16 06:33_

You need to setup your [editor to use the ty server](https://github.com/astral-sh/ty/tree/main/docs#editor-integration). The easiest is to use ty's vs code extension but I don't know if that's the editor you use. 

If you use ty's VS code extension. Change the `ty.path` setting so that it points to the `<path_to_ruff_repo>/target/debug/ty` and then build ty with `cargo build --bin ty`

---

_Closed by @MichaReiser on 2025-05-17 17:28_

---
