```yaml
number: 15232
title: "Improve logging system using `logLevel`, avoid trace value"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/server-logs
created_at: 2025-01-03T04:05:32Z
updated_at: 2025-01-09T04:36:00Z
url: https://github.com/astral-sh/ruff/pull/15232
synced_at: 2026-01-12T15:55:50Z
```

# Improve logging system using `logLevel`, avoid trace value

---

_@dhruvmanila_

**Changelog:** This PR improves observability in the server by removing the need of using the ["trace" value](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#traceValue) as a means to turn on or off logging. Now, the server logging is solely controlled using the [`logLevel` server setting](https://docs.astral.sh/ruff/editors/settings/#loglevel) which defaults to "info". This avoids the confusion where the user is notified about going through the log messages but there aren't any.

## Summary

Refer to the VS Code PR (https://github.com/astral-sh/ruff-vscode/pull/659) for details on the change.

This PR changes the following:

1. Add tracing span for both request (request id and method name) and notification (method name) handler
2. Remove the `RUFF_TRACE` environment variable. This was being used to turn on / off logging for the server
3. Similarly, remove reading the `trace` value from the initialization options
4. Remove handling the `$/setTrace` notification
5. Remove the specialized `TraceLogWriter` used for Zed and VS Code (https://github.com/astral-sh/ruff/pull/12564)

Regarding the (5) for the Zed editor, the reason that was implemented was because there was no way of looking at the stderr messages in the editor which has been changed. Now, it captures the stderr as part of the "Server Logs". (https://github.com/zed-industries/zed/blob/82492d74a8d0350cba66671c27e282a928fd4c85/crates/language_tools/src/lsp_log.rs#L548-L552)

### Question

Regarding (1), I think having just a simple trace level message should be good for now as the spans are not hierarchical. This could be tackled with astral-sh/ty#90. The difference between the two:

<details><summary>Using <code>tracing::trace</code></summary>
<p>

```
   0.019243416s DEBUG ThreadId(08) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/playground/ruff/.vscode
   0.026398750s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/playground/ruff
   0.026802125s TRACE ruff:main ruff_server::server::api: Received notification "textDocument/didOpen"
   0.026930666s TRACE ruff:main ruff_server::server::api: Received notification "textDocument/didOpen"
   0.026962333s TRACE ruff:main ruff_server::server::api: Received request "textDocument/diagnostic" (1)
   0.027042875s TRACE ruff:main ruff_server::server::api: Received request "textDocument/diagnostic" (2)
   0.027097500s TRACE ruff:main ruff_server::server::api: Received request "textDocument/codeAction" (3)
   0.027107458s DEBUG ruff:worker:0 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
   0.027123541s DEBUG ruff:worker:3 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/organize_imports.py
   0.027514875s  INFO     ruff:main ruff_server::server: Configuration file watcher successfully registered
   0.285689833s TRACE     ruff:main ruff_server::server::api: Received request "textDocument/codeAction" (4)
  45.741101666s TRACE     ruff:main ruff_server::server::api: Received notification "textDocument/didClose"
  47.108745500s TRACE     ruff:main ruff_server::server::api: Received notification "textDocument/didOpen"
  47.109802041s TRACE     ruff:main ruff_server::server::api: Received request "textDocument/diagnostic" (5)
  47.109926958s TRACE     ruff:main ruff_server::server::api: Received request "textDocument/codeAction" (6)
  47.110027791s DEBUG ruff:worker:6 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
  51.863679125s TRACE     ruff:main ruff_server::server::api: Received request "textDocument/hover" (7)
```

</p>
</details> 

<details><summary>Using <code>tracing::trace_span</code></summary>
<p>

Only logging the enter event:

```
   0.018638750s DEBUG ThreadId(11) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/playground/ruff/.vscode
   0.025895791s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/playground/ruff
   0.026378791s TRACE ruff:main notification{method="textDocument/didOpen"}: ruff_server::server::api: enter
   0.026531208s TRACE ruff:main notification{method="textDocument/didOpen"}: ruff_server::server::api: enter
   0.026567583s TRACE ruff:main request{id=1 method="textDocument/diagnostic"}: ruff_server::server::api: enter
   0.026652541s TRACE ruff:main request{id=2 method="textDocument/diagnostic"}: ruff_server::server::api: enter
   0.026711041s DEBUG ruff:worker:2 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/organize_imports.py
   0.026729166s DEBUG ruff:worker:1 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
   0.027023083s  INFO     ruff:main ruff_server::server: Configuration file watcher successfully registered
   5.197554750s TRACE     ruff:main notification{method="textDocument/didClose"}: ruff_server::server::api: enter
   6.534458000s TRACE     ruff:main notification{method="textDocument/didOpen"}: ruff_server::server::api: enter
   6.535027958s TRACE     ruff:main request{id=3 method="textDocument/diagnostic"}: ruff_server::server::api: enter
   6.535271166s DEBUG ruff:worker:3 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/organize_imports.py
   6.544240583s TRACE     ruff:main request{id=4 method="textDocument/codeAction"}: ruff_server::server::api: enter
   7.049692458s TRACE     ruff:main request{id=5 method="textDocument/codeAction"}: ruff_server::server::api: enter
   7.508142541s TRACE     ruff:main request{id=6 method="textDocument/hover"}: ruff_server::server::api: enter
   7.872421958s TRACE     ruff:main request{id=7 method="textDocument/hover"}: ruff_server::server::api: enter
   8.024498583s TRACE     ruff:main request{id=8 method="textDocument/codeAction"}: ruff_server::server::api: enter
  13.895063666s TRACE     ruff:main request{id=9 method="textDocument/codeAction"}: ruff_server::server::api: enter
  14.774706083s TRACE     ruff:main request{id=10 method="textDocument/hover"}: ruff_server::server::api: enter
  16.058918958s TRACE     ruff:main notification{method="textDocument/didChange"}: ruff_server::server::api: enter
  16.060562208s TRACE     ruff:main request{id=11 method="textDocument/diagnostic"}: ruff_server::server::api: enter
  16.061109083s DEBUG ruff:worker:8 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
  21.561742875s TRACE     ruff:main notification{method="textDocument/didChange"}: ruff_server::server::api: enter
  21.563573791s TRACE     ruff:main request{id=12 method="textDocument/diagnostic"}: ruff_server::server::api: enter
  21.564206750s DEBUG ruff:worker:4 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
  21.826691375s TRACE     ruff:main request{id=13 method="textDocument/codeAction"}: ruff_server::server::api: enter
  22.091080125s TRACE     ruff:main request{id=14 method="textDocument/codeAction"}: ruff_server::server::api: enter
```

</p>
</details> 


**Todo**

- [x] Update documentation (I'll be adding a troubleshooting section under "Editors" as a follow-up which is for all editors)
- [x] Check for backwards compatibility. I don't think this should break backwards compatibility as it's mainly targeted towards improving the debugging experience.

~**Before I go on to updating the documentation, I'd appreciate initial review on the chosen approach.**~

resolves: astral-sh/ruff#14959 

## Test Plan

Refer to the test plan in https://github.com/astral-sh/ruff-vscode/pull/659.

Example logs at `debug` level:

```
   0.010770083s DEBUG ThreadId(15) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/playground/ruff/.vscode
   0.018101916s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/playground/ruff
   0.018559916s DEBUG ruff:worker:4 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
   0.018992375s  INFO     ruff:main ruff_server::server: Configuration file watcher successfully registered
  23.408802375s DEBUG ruff:worker:11 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
  24.329127416s DEBUG  ruff:worker:6 ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
```

Example logs at `trace` level:

```
   0.010296375s DEBUG ThreadId(13) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/playground/ruff/.vscode
   0.017422583s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/playground/ruff
   0.018034458s TRACE ruff:main notification{method="textDocument/didOpen"}: ruff_server::server::api: enter
   0.018199708s TRACE ruff:worker:0 request{id=1 method="textDocument/diagnostic"}: ruff_server::server::api: enter
   0.018251167s DEBUG ruff:worker:0 request{id=1 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
   0.018528708s  INFO     ruff:main ruff_server::server: Configuration file watcher successfully registered
   1.611798417s TRACE ruff:worker:1 request{id=2 method="textDocument/codeAction"}: ruff_server::server::api: enter
   1.861757542s TRACE ruff:worker:4 request{id=3 method="textDocument/codeAction"}: ruff_server::server::api: enter
   7.027361792s TRACE ruff:worker:2 request{id=4 method="textDocument/codeAction"}: ruff_server::server::api: enter
   7.851361500s TRACE ruff:worker:5 request{id=5 method="textDocument/codeAction"}: ruff_server::server::api: enter
   7.901690875s TRACE     ruff:main notification{method="textDocument/didChange"}: ruff_server::server::api: enter
   7.903063167s TRACE ruff:worker:10 request{id=6 method="textDocument/diagnostic"}: ruff_server::server::api: enter
   7.903183500s DEBUG ruff:worker:10 request{id=6 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
   8.702385292s TRACE      ruff:main notification{method="textDocument/didChange"}: ruff_server::server::api: enter
   8.704106625s TRACE  ruff:worker:3 request{id=7 method="textDocument/diagnostic"}: ruff_server::server::api: enter
   8.704304875s DEBUG  ruff:worker:3 request{id=7 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/play.py
   8.966853458s TRACE  ruff:worker:9 request{id=8 method="textDocument/codeAction"}: ruff_server::server::api: enter
   9.229622792s TRACE  ruff:worker:6 request{id=9 method="textDocument/codeAction"}: ruff_server::server::api: enter
  10.513111583s TRACE  ruff:worker:7 request{id=10 method="textDocument/codeAction"}: ruff_server::server::api: enter
```

---

_Label `server` added by @dhruvmanila on 2025-01-03 04:05_

---

_Comment by @github-actions[bot] on 2025-01-03 04:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[WIP] Improve server logging" to "Improve language server logging" by @dhruvmanila on 2025-01-03 10:03_

---

_Renamed from "Improve language server logging" to "Use stderr for server log messages" by @dhruvmanila on 2025-01-03 12:22_

---

_Marked ready for review by @dhruvmanila on 2025-01-03 12:29_

---

_Review requested from @carljm by @dhruvmanila on 2025-01-03 12:29_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-03 12:29_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-01-03 12:29_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-01-03 12:29_

---

_Renamed from "Use stderr for server log messages" to "Improve logging system using `logLevel`, avoid trace value" by @dhruvmanila on 2025-01-07 09:58_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/logging.rs`:84 on 2025-01-07 13:11_

Should this default to `Info` or is the `Default` implementation unused?

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/logging.rs`:62 on 2025-01-07 13:13_

IMO, the ideal setup for Red Knot would be if we'd reuse the tracing setup from the CLI for as much as possible instead of introducing a slightly different tracing setup. But that's something we can follow up separately

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/logging.rs`:23 on 2025-01-07 13:14_

I think this can be removed, now that we no longer use `logMessage` for logging

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api.rs`:29 on 2025-01-07 13:17_

I don't think this works as expected because the `span` is dropped as soon as the `request` method ends. You should be able to see this by adding other tracing logs in a request handler. Those messages should be nested under the `request` level. 

Instead, what we have to do is to create the `span` here, but only call `entered` in the thread that process the request OR defer creating the span to the thread that processes the request. 

The same applies to notification. 

Does this also cover response handlers?

---

_@MichaReiser reviewed on 2025-01-07 13:18_

---

_@dhruvmanila reviewed on 2025-01-07 14:10_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/logging.rs`:84 on 2025-01-07 14:10_

Good catch, I didn't see that. Yes, the default is unused but I think we should use the default implementation. I'll update the code.

---

_@dhruvmanila reviewed on 2025-01-07 14:13_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/logging.rs`:62 on 2025-01-07 14:13_

Yes, #12744 

---

_@dhruvmanila reviewed on 2025-01-08 04:13_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api.rs`:29 on 2025-01-08 04:13_

Thanks, I've updated them.

No it doesn't cover response handler, I think we decided not to add them for now as the results are large.

---

_@MichaReiser reviewed on 2025-01-08 07:19_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api.rs`:29 on 2025-01-08 07:19_

> No it doesn't cover response handler, I think we decided not to add them for now as the results are large.

Can you tell me more about what you mean by "the results are large"? What I mean is that we may want to add a tracing span to 

https://github.com/astral-sh/ruff/blob/b63c2e126b928fab4180eda5d9132552beeee7fe/crates/ruff_server/src/server/client.rs#L110-L143

---

_@MichaReiser approved on 2025-01-08 07:19_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api.rs`:29 on 2025-01-08 12:32_

I see, yeah I think it makes sense to add it there as well.

---

_@dhruvmanila reviewed on 2025-01-08 12:32_

---

_@dhruvmanila reviewed on 2025-01-08 12:46_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api.rs`:29 on 2025-01-08 12:46_

I'm actually thinking to avoid this for now as currently the output isn't hierarchical so I'll prefer to add this after we merge it with red knot logging.

---

_Merged by @dhruvmanila on 2025-01-08 12:48_

---

_Closed by @dhruvmanila on 2025-01-08 12:48_

---

_Branch deleted on 2025-01-08 12:48_

---
