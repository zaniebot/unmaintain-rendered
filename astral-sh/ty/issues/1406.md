```yaml
number: 1406
title: LSP hang/crash on certain kinds of syntax error?
type: issue
state: closed
author: DanCardin
labels:
  - bug
  - server
assignees: []
created_at: 2025-10-20T16:35:06Z
updated_at: 2025-10-21T08:51:10Z
url: https://github.com/astral-sh/ty/issues/1406
synced_at: 2026-01-10T02:06:25Z
```

# LSP hang/crash on certain kinds of syntax error?

---

_Issue opened by @DanCardin on 2025-10-20 16:35_

### Summary

The act of going from:
```
def foo(self, key, value):
    return
```

to

```
def foo(self, **key, value):
    return
```

by typing the `**` hangs neovim for me. I'm not sure how to take neovim out of the loop here, but given that this kind of instant hang is, so far, unique to `ty` i'm hoping it's straightforward to reproduce whether or not neovim is involved.

I dont see this problem on alpha 14, but do for every version afterward.

### Version

ty 0.0.1-alpha.15 (0369a3598 2025-07-18) onward

---

_Label `server` added by @AlexWaygood on 2025-10-20 16:37_

---

_Label `bug` added by @sharkdp on 2025-10-21 06:32_

---

_Comment by @sharkdp on 2025-10-21 06:33_

Thank you for reporting this. I can confirm the error. Neovim's LSP logs show this:

```
ERROR An error occurred with request ID 37: request handler panicked at crates/ty_ide/src/semantic_tokens.rs:234:9:\nTokens must be added in file order: previous token ends at Some(21), new token starts at 16\nrun with `RUST_BACKTRACE=1` environment variable to display a backtrace\n\n"
```

Which points to this debug assertion:

https://github.com/astral-sh/ruff/blob/24d0f65d6237eb956553f0a8075030113f065aa1/crates/ty_ide/src/semantic_tokens.rs#L233-L241

---

_Comment by @AlexWaygood on 2025-10-21 06:43_

> Which points to this debug assertion:

That's interesting — I assume OP is using a release build, in which case debug assertions probably won't trigger for them. Is it possible that this is a separate bug?

---

_Comment by @MichaReiser on 2025-10-21 06:46_

> Thank you for reporting this. I can confirm the error. Neovim's LSP logs show this:
> 
> ```
> ERROR An error occurred with request ID 37: request handler panicked at crates/ty_ide/src/semantic_tokens.rs:234:9:\nTokens must be added in file order: previous token ends at Some(21), new token starts at 16\nrun with `RUST_BACKTRACE=1` environment variable to display a backtrace\n\n"
> ```
> 
> Which points to this debug assertion:
> 
> [astral-sh/ruff@`24d0f65`/crates/ty_ide/src/semantic_tokens.rs#L233-L241](https://github.com/astral-sh/ruff/blob/24d0f65d6237eb956553f0a8075030113f065aa1/crates/ty_ide/src/semantic_tokens.rs#L233-L241)

Could you enable `RUST_BACKTRACE=1` with a debug build? It would help narrow down where the tokens are added out of order.

Edit: I'm adding a test now

---

_Comment by @MichaReiser on 2025-10-21 06:49_

> That's interesting — I assume OP is using a release build, in which case debug assertions probably won't trigger for them. Is it possible that this is a separate bug?

The LSP uses a compact encoding for semantic tokens where offests are encoded relative to the previous token. I'm not sure if it supports negative offsets (as tokens are expected to be in source order) and I could see clients get stuck if a relative offset happens to be negative.

---

_Closed by @MichaReiser on 2025-10-21 08:51_

---
