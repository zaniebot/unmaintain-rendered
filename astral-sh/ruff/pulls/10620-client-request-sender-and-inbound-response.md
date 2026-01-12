```yaml
number: 10620
title: "Client request sender and inbound response handling for `ruff server`"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/response-handling
created_at: 2024-03-26T18:23:22Z
updated_at: 2024-03-27T19:09:44Z
url: https://github.com/astral-sh/ruff/pull/10620
synced_at: 2026-01-12T15:55:32Z
```

# Client request sender and inbound response handling for `ruff server`

---

_@snowsignal_

## Summary

Fixes #10618.

This PR introduces a proper API for sending requests to the client and handling any response sent back. Dynamic capability registration now uses this new API, fixing an issue where a much more simplistic response handler silently flushes a code action request that needed a response.

## Test Plan

#10618 can no longer be reproduced. No errors about unhandled responses should appear in the extension output, and you should see this new log when the server starts:
```
<DATE> <TIME> [info] <DURATION> INFO ruff_server::server Configuration file watcher successfully registered
```


---

_Label `server` added by @snowsignal on 2024-03-26 18:23_

---

_Review requested from @charliermarsh by @snowsignal on 2024-03-26 18:27_

---

_Comment by @github-actions[bot] on 2024-03-26 18:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-03-26 18:42_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server.rs`:107 on 2024-03-26 18:42_

Is the right way to think of this that:

- We might have some responses on `connection.receiver` already (but they can be buffered).
- We kick off a "blocking" request to register capabilities, and wait for the client to respond.
- Once the client responds, we move to to processing messages on `connection.receiver`.

---

_@charliermarsh reviewed on 2024-03-26 18:43_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server.rs`:117 on 2024-03-26 18:43_

I may not have enough context on this piece. Why is this now not an error?

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:117 on 2024-03-26 18:45_

It was an error before because we didn't have any reason to receive a response from the server. Now that we have an interface to send the client requests, we do want to listen for responses.

---

_@snowsignal reviewed on 2024-03-26 18:45_

---

_@snowsignal reviewed on 2024-03-26 18:50_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:107 on 2024-03-26 18:50_

You're on the right track, but we don't actually wait for the client to respond here. We kick off a request and then immediately go into the event loop. Responses are handled as part of that event loop.

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server.rs`:107 on 2024-03-26 19:16_

Ahh ok makes sense. Is it important that the dynamic capabilities response is handled "first", or it's ok if they're out-of-order? (Do we enforce the order in any way?)

---

_@charliermarsh approved on 2024-03-26 19:16_

---

_@snowsignal reviewed on 2024-03-26 19:36_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:107 on 2024-03-26 19:36_

> (Do we enforce the order in any way?)

We don't enforce the order - we rely on `lsp_server::Connection` to give us messages in the order the client sent them. The LSP specification requires that [requests on both sides are processed in the order that they're received](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#messageOrdering), but it doesn't say that all incoming requests need to be handled before responses, or vice-versa.

> Is it important that the dynamic capabilities response is handled "first", or it's ok if they're out-of-order?

In this specific instance, it's absolutely fine to have this happen out-of-order because the response handler doesn't do anything besides log a success. Technically, the client could wait an arbitrary amount before sending this response if it wanted to (though that would be rude).

I think this scenario would also work in the general case:
```
1. client sends request A to server
2. server sends request B to client
3. server sends response A to client
4. client sends response B response to server
```

The response handler for `response B` would be run with a potentially different state then when `request B` was sent occurred if `request A` was a local task that modified the state, but the response handler shouldn't have any guarantees on execution order to begin with.

---

_Merged by @snowsignal on 2024-03-26 20:53_

---

_Closed by @snowsignal on 2024-03-26 20:53_

---

_Branch deleted on 2024-03-26 20:53_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:174 on 2024-03-27 08:05_

Nit: This is not something we need to immediately follow up. It's more an observation and maybe we have ideas on how we could improve it later on. 

What would be nice is if it wouldn't be necessary to specify the `Request` type when calling the function, and instead could let Rust infer it. I think the issue here is that we pass the parameters but there's no way to get to the request from the parameters (only the other way around). 
I don't have a solution to it, but thought it might be worth raising because maybe you have a great idea on how this could be improved.

---

_@MichaReiser reviewed on 2024-03-27 08:06_

Nice! It removes the need for the atomic request counter and we get full request/response support.

---

_@snowsignal reviewed on 2024-03-27 19:09_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:174 on 2024-03-27 19:09_

Unfortunately I think this is just a limitation of Rust's type resolver, and I understand why. It would definitely be nice if the compiler could infer the type from the `Params` + `Result` type, but it would fail in a scenario where multiple requests had the same `Params` and `Result` type.

I'm fine with being explicit about the request type, to be honest. We do something similar when creating tasks for API endpoints. We *could* make the request a function argument and that would remove the need to specify the generic parameter, but I prefer putting it in angle brackets 😄 

---
