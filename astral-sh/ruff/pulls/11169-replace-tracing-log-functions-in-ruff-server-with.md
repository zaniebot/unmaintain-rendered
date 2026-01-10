```yaml
number: 11169
title: "Replace `tracing` log functions in `ruff server` with LSP-compliant logging functions"
type: pull_request
state: closed
author: snowsignal
labels:
  - server
assignees: []
base: main
head: jane/server/logging
created_at: 2024-04-26T23:30:13Z
updated_at: 2025-02-20T09:00:15Z
url: https://github.com/astral-sh/ruff/pull/11169
synced_at: 2026-01-10T19:57:22Z
```

# Replace `tracing` log functions in `ruff server` with LSP-compliant logging functions

---

_Pull request opened by @snowsignal on 2024-04-26 23:30_

## Summary

Closes https://github.com/astral-sh/ruff/issues/10968.

At the moment, the log level is always set to `Info`. Log level configuration will be implemented in a follow-up.

## Test Plan

Add a series of logs, one for each level, to the definition of `Server::new`, after the logger is initialized:
![Screenshot 2024-04-26 at 4 16 25 PM](https://github.com/astral-sh/ruff/assets/19577865/36a75077-bc04-4d7e-86ea-da230c565c91)

You should see the following logs in the output of the VS Code extension:
<img width="621" alt="Screenshot 2024-04-26 at 4 21 23 PM" src="https://github.com/astral-sh/ruff/assets/19577865/d27ee582-5c3d-4105-9a7b-ef41e0dd1517">



---

_Label `server` added by @snowsignal on 2024-04-26 23:30_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-26 23:30_

---

_Comment by @github-actions[bot] on 2024-04-26 23:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2024-04-27 01:37_

Is there an alternate design whereby we wire up `tracing` to emit to the LSP client? I.e., some kind of `tracing` configuration that would let us continue to use `tracing::info!` and friends, but _also_ emit to the client?

---

_Comment by @MichaReiser on 2024-04-27 07:06_

@charliermarsh I think that should be possible by setting up our own subscriber that then forwards messages to the LSP.

I think the problem this approach is that it only works for ruff_server, but we will use tracing in runes too and we don't have the luxury to change all tracing calls there. 

I think we need to explore an alternative approach that captures all tracing messages (subscriber) and forwards them correctly OR setup an alternative subscriber that writes e.g. to a file, although I would prefer seeing the tracing messages in vs code.

---

_Comment by @charliermarsh on 2024-04-27 11:55_

Yeah I would prefer trying to integrate this into tracing too (for those reasons, plus it's the API I would've naturally reached for if I went to write code in the LSP as a future editor / author). @snowsignal can you give it a shot?

---

_Comment by @snowsignal on 2024-04-27 23:35_

@charliermarsh Sure, I'll see what I can do! 

---

_Review requested from @charliermarsh by @snowsignal on 2024-04-28 03:05_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:49 on 2024-05-01 08:54_

We should ideally respect the log level configured in the VS Code settings or does VS code take care of filtering out log messages?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:39 on 2024-05-01 08:55_

Can we use a debug level instead of dropping these messages?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/server.rs`:60 on 2024-05-01 08:58_

I think we still want to have some form of filtering of log messages to avoid that tracing logs from third party libraries (trace and debug) show up in our LSP logs (they can be very noisy). 

The old implementation limited third-party logs to `info` level or higher for this. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:20 on 2024-05-01 09:00_

I wonder what will happen now where `ClientSender` is a weakref. I suspect that we won't see the `Main loop stopped` message anymore because the logging sender is shut down by then. 

I think we should unset our tracing subscriber before dropping `Connection` and restore the old subscriber to ensure these messages don't get lost. 

---

_@MichaReiser reviewed on 2024-05-01 09:00_

---

_@snowsignal reviewed on 2024-05-02 03:24_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:49 on 2024-05-02 03:24_

We don't have a log level configured in the VS Code settings at the moment. All we have is `ruff.trace.server`, but that's more a verbosity setting than something that controls the log level.

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:20 on 2024-05-02 03:25_

> I think we should unset our tracing subscriber before dropping `Connection` and restore the old subscriber to ensure these messages don't get lost.

Good idea!

---

_@snowsignal reviewed on 2024-05-02 03:25_

---

_Removed from milestone `Ruff Server: Beta` by @snowsignal on 2024-05-22 09:47_

---

_Comment by @snowsignal on 2024-06-04 23:30_

This PR is really outdated, especially with the major refactors we made to server initialization. I'll re-implement this feature in a new PR that's based off of the latest `main`.

---

_Closed by @snowsignal on 2024-06-04 23:30_

---

_Branch deleted on 2025-02-20 09:00_

---
