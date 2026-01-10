```yaml
number: 11291
title: "Fix `ruff server` hanging after Neovim closes"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: jane/server/neovim-shutdown
created_at: 2024-05-05T17:08:50Z
updated_at: 2024-05-06T02:07:28Z
url: https://github.com/astral-sh/ruff/pull/11291
synced_at: 2026-01-10T22:37:02Z
```

# Fix `ruff server` hanging after Neovim closes

---

_Pull request opened by @snowsignal on 2024-05-05 17:08_

## Summary

A follow-up to https://github.com/astral-sh/ruff/pull/11222. `ruff server` stalls during shutdown with Neovim because after it receives an exit notification and closes the I/O thread, it attempts to log a success message to `stderr`. Removing this log statement fixes this issue.

## Test Plan

Track the instances of `ruff` in the OS task manager as you open and close Neovim. A new instance should appear when Neovim starts and it should disappear once Neovim is closed.


---

_Label `bug` added by @snowsignal on 2024-05-05 17:08_

---

_Label `server` added by @snowsignal on 2024-05-05 17:08_

---

_@charliermarsh approved on 2024-05-05 17:11_

Do you think this would work as expected if we were sending messages via the LSP protocol? Or same issue?

---

_Comment by @snowsignal on 2024-05-05 17:15_

> Do you think this would work as expected if we were sending messages via the LSP protocol? Or same issue?

That wouldn't work because the I/O with the client has shut down at the point where we log this. However, it would at least fail instantly instead of just waiting forever.

---

_Merged by @snowsignal on 2024-05-05 17:15_

---

_Closed by @snowsignal on 2024-05-05 17:15_

---

_Branch deleted on 2024-05-05 17:15_

---

_Comment by @github-actions[bot] on 2024-05-05 17:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-05-05 18:42_

How can we avoid this in the future? It seems error prone to rely on tracing not being used after the main loop ends (it may even be a dependency that writes a trace, maybe even a third party dependency 

---

_Comment by @charliermarsh on 2024-05-05 18:51_

Any ideas? I don’t understand the root of the problem well-enough.

---

_Comment by @MichaReiser on 2024-05-05 18:54_

Not understanding the root cause is what makes me wary of the fix. It's a good fix to unblock users but it seems fragile to me long term. 



---

_Comment by @snowsignal on 2024-05-05 20:33_

I can do some further investigation into why the tracing log was blocking.

---

_Comment by @ShIRannx on 2024-05-06 02:07_

the `ruff server` is not the subprocess of nvim. is this relevant to it?

---
