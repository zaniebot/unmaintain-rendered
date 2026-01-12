```yaml
number: 642
title: Crash when triggering completions in unreachable match cases
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
  - fatal
  - completions
assignees: []
created_at: 2025-06-12T14:05:21Z
updated_at: 2025-10-03T12:20:03Z
url: https://github.com/astral-sh/ty/issues/642
synced_at: 2026-01-12T15:54:23Z
```

# Crash when triggering completions in unreachable match cases

---

_@sharkdp_

~~I tried to trigger this in the corpus tests and the LSP server, but didn't manage to do so, so reporting as a playground problem for now:~~

Start with https://play.ty.dev/089aad00-e47d-4076-bc45-c9c9f4293f81 and then type ` if` after the `1` in the match case:
```py
match 0:
    case 1:  # try to add ` if …` after the `1` here
        pass
```
This crashes with "entered unreachable code: If we have at least one declaration, the scope-start should not be definitely visible". 

This seems to be triggered by auto-completions. When starting with https://play.ty.dev/e0c377ca-5e64-4478-86a2-8c6f11f31ee8
```py
match 0:
    case 1 i:
        pass
```
there is no crash. But when I explicitly trigger the autocompletions using ctrl-space after the `i`, I get the same crash.

---

_Label `bug` added by @sharkdp on 2025-06-12 14:05_

---

_Label `fatal` added by @sharkdp on 2025-06-12 14:05_

---

_Assigned to @sharkdp by @sharkdp on 2025-06-12 14:05_

---

_Comment by @sharkdp on 2025-06-12 14:33_

Oh, this seems to reproduce in the IDE tests as well:
```rs
let test = cursor_test(
    r#"
    match 0:
        case 1 i<CURSOR>:
            pass
    "#,
);
```

---

_Renamed from "Playground crashes when adding guard in unreachable match case" to "Crash when triggering completions in unreachable match cases" by @sharkdp on 2025-06-12 14:34_

---

_Label `server` added by @AlexWaygood on 2025-06-12 14:52_

---

_Closed by @sharkdp on 2025-06-17 07:24_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:20_

---
