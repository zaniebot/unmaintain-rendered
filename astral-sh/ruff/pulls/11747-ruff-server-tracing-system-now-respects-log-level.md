```yaml
number: 11747
title: "`ruff server`: Tracing system now respects log level and trace level, with options to log to a file"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/logging-v2
created_at: 2024-06-05T01:01:05Z
updated_at: 2024-06-11T18:29:48Z
url: https://github.com/astral-sh/ruff/pull/11747
synced_at: 2026-01-12T15:55:38Z
```

# `ruff server`: Tracing system now respects log level and trace level, with options to log to a file

---

_@snowsignal_

## Summary

Fixes #10968.
Fixes #11545.

The server's tracing system has been rewritten from the ground up. The server now has trace level and log level settings which restrict the tracing events and spans that get logged. 

* A `logLevel` setting has been added, which lets a user set the log level. By default, it is set to `"info"`.
* A `logFile` setting has also been added, which lets the user supply an optional file to send tracing output (it does not have to exist as a file yet). By default, if this is unset, tracing output will be sent to `stderr`.
* A `$/setTrace` handler has also been added, and we also set the trace level from the initialization options. For editors without direct support for tracing, the environment variable `RUFF_TRACE` can override the trace level.
* Small changes have been made to how we display tracing output. We no longer use `tracing-tree`, and instead use `tracing_subscriber::fmt::Layer` to format output. Thread names are now included in traces, and I've made some adjustment to thread worker names to be more useful.

## Test Plan

In VS Code, with `ruff.trace.server` set to its default value, no logs from Ruff should appear.

After changing `ruff.trace.server` to either `messages` or `verbose`, you should see log messages at `info` level or higher appear in Ruff's output:
<img width="1005" alt="Screenshot 2024-06-10 at 10 35 04 AM" src="https://github.com/astral-sh/ruff/assets/19577865/6050d107-9815-4bd2-96d0-e86f096a57f5">

In Helix, by default, no logs from Ruff should appear.

To set the trace level in Helix, you'll need to modify your language configuration as follows:
```toml
[language-server.ruff]
command = "/Users/jane/astral/ruff/target/debug/ruff"
args = ["server", "--preview"]
environment = { "RUFF_TRACE" = "messages" }
```

After doing this, logs of `info` level or higher should be visible in Helix:
<img width="1216" alt="Screenshot 2024-06-10 at 10 39 26 AM" src="https://github.com/astral-sh/ruff/assets/19577865/8ff88692-d3f7-4fd1-941e-86fb338fcdcc">

You can use `:log-open` to quickly open the Helix log file.

In Neovim, by default, no logs from Ruff should appear.

To set the trace level in Neovim, you'll need to modify your configuration as follows:
```lua
require('lspconfig').ruff.setup {
  cmd = {"/path/to/debug/executable", "server", "--preview"},
  cmd_env = { RUFF_TRACE = "messages" }
}
```

You should see logs appear in `:LspLog` that look like the following:
<img width="1490" alt="Screenshot 2024-06-11 at 11 24 01 AM" src="https://github.com/astral-sh/ruff/assets/19577865/576cd5fa-03cf-477a-b879-b29a9a1200ff">

You can adjust `logLevel` and `logFile` in `settings`:
```lua
require('lspconfig').ruff.setup {
  cmd = {"/path/to/debug/executable", "server", "--preview"},
  cmd_env = { RUFF_TRACE = "messages" },
  settings = {
    logLevel = "debug",
    logFile = "your/log/file/path/log.txt"
  }
}
```

The `logLevel` and `logFile` can also be set in Helix like so:
```toml
[language-server.ruff.config.settings]
logLevel = "debug"
logFile = "your/log/file/path/log.txt"
```

Even if this log file does not exist, it should now be created and written to after running the server:

<img width="1148" alt="Screenshot 2024-06-10 at 10 43 44 AM" src="https://github.com/astral-sh/ruff/assets/19577865/ab533cf7-d5ac-4178-97f1-e56da17450dd">


---

_Label `server` added by @snowsignal on 2024-06-05 01:01_

---

_Review requested from @MichaReiser by @snowsignal on 2024-06-05 01:01_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:11 on 2024-06-05 01:03_

~~The reason I went with `Messages` instead of `Off` as a default is so that `info` events are logged by default if the trace value is unset by the LSP client.~~

Nevermind, I misunderstood what TraceValue was for. It should be `Off` by default.

---

_@snowsignal reviewed on 2024-06-05 01:04_

---

_Comment by @github-actions[bot] on 2024-06-05 01:29_

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

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:40 on 2024-06-05 06:54_

The `TRACE_VALUE` value is never read, but only set. We should delete the code if we aren't using it anyway or implement the logic to use the value. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:25 on 2024-06-05 06:59_

I think implementing this shouldn't be too hard

* Store the `LogLevel` instead of the `TRACE_VALUE` 
* Change `LogFilter` to read the `TRACE_VALUE` variable instead of the local `filter` to determine the level

You could also consider making `LogLevel` inherit from `:u8` and adding a `TryFrom<u8> for LogLevel`. That way, you could use a `AtomicU8` for `TRACE_VALUE`

---

_@MichaReiser requested changes on 2024-06-05 07:00_

Looks good. I think we should address the TODO before merging this and extend the test plan with a case that shows that changing the verbosity in the extension settings changes the output.

---

_@snowsignal reviewed on 2024-06-05 18:22_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:40 on 2024-06-05 18:22_

The PR made a note that this would be used in the future:

> This PR also introduces a $/setTrace handler, which will be used in the future by our tracing span subscriber.

I wanted to at least add a handler for this so that we don't get a warning about a nonexistent handler every time the trace level changes.

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:25 on 2024-06-05 18:24_

I actually explored having the log level be set by the trace value (you can see it in the commit history). I ultimately decided against this because the trace value sent by the LSP is [only meant for `$/logTrace`, not `window/logMessage`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#traceValue).

---

_@snowsignal reviewed on 2024-06-05 18:24_

---

_Comment by @codspeed-hq[bot] on 2024-06-05 20:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/server/logging-v2)

### Merging #11747 will **improve performances by 13.17%**

<sub>Comparing <code>jane/server/logging-v2</code> (f5d35dd) with <code>main</code> (521a358)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `jane/server/logging-v2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 917.3 µs | 810.5 µs | +13.17% |


---

_Review requested from @MichaReiser by @snowsignal on 2024-06-05 22:18_

---

_@snowsignal reviewed on 2024-06-05 23:30_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:25 on 2024-06-05 23:30_

I've introduced a `logLevel` setting to client settings to fix this.

---

_@MichaReiser reviewed on 2024-06-06 06:00_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:25 on 2024-06-06 06:00_

Oh I didn't realize that difference when reviewing yesterday. What's the reason we don't use `$/logTrace` because it seems like the right fit from the spec

> $/logTrace should be used for systematic trace reporting. For single debugging messages, the server should send [window/logMessage](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#window_logMessage) notifications. [source](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#logTrace)

---

_@MichaReiser reviewed on 2024-06-06 06:06_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:25 on 2024-06-06 06:06_

Okay, I think I'm starting to understand what this PR does. I think the PR title needs updating. 

I try to summarize my understanding to make sure it is correct. 

This PR only logs `trace::debug`, `trace::warn` but it omits all `trace::debug_span` etc. Is this correct?

What's the motivation for making this distinction instead of just logging all tracing events with `$/logTrace` for now. That seems simpler because it doesn't require additional filtering or yet another setting (which I'm worried that it just gets hard to understand for users, especially because `TRACE_VALUE: message` exists). Is it because you need a way to format a span to a string? 

---

_@snowsignal reviewed on 2024-06-07 00:15_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:25 on 2024-06-07 00:15_

@MichaReiser My plan was for tracing events to go through `window/logMessage` and then later implement a additional tracing layer that sends all tracing events / messages to `$/logTrace` as a follow-up.

The reason I'm doing this is to separate the log level from the trace level.

---

_Renamed from "`ruff server`: Use `window/logMessage` for tracing events" to "`ruff server`: Tracing system now respects log level and trace level, with options to log to a file" by @snowsignal on 2024-06-09 23:05_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:131 on 2024-06-10 06:38_

Nit: `tracing-subscriber` has a `BoxMakeWriter` that is just that. You can call it with `BoxMakeWriter::new(std::io::stderr)`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:147 on 2024-06-10 06:40_

I would find some module level documentation helpful that explains the different ways on how tracing can be configured. It seem we now support an env var, `TRACE_VALUE`, a file, and the PR description mentions a `logLevel` setting, but it's not clear to me where that one is set.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:133 on 2024-06-10 06:43_

It seems that we now only support `Off` vs `Message` and `Verbose`. What do you think of mapping `Message` to `Info`, and `Verbose` to `Debug` or `Trace`?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:125 on 2024-06-10 06:45_

It seems we need to inform tracing  when the log level changes

> If the maximum level the Filter will enable can change over the course of its lifetime, it is free to return a different value from multiple invocations of this method. However, note that changes in the maximum level will only be reflected after the callsite Interest cache is rebuilt, by calling the tracing_core::callsite::rebuild_interest_cache function. Therefore, if the Filter will change the value returned by this method, it is responsible for ensuring that [rebuild_interest_cache`]rebuild is called after the value of the max level changes.

[source](https://docs.rs/tracing-subscriber/latest/tracing_subscriber/layer/trait.Filter.html#method.max_level_hint)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:76 on 2024-06-10 06:46_

What's the motivation of having two filters rather than just one?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/message.rs`:41 on 2024-06-10 06:48_

Did you change this to `eprintln` because it might happen before tracing is set up? I think it would still be nice if the message also goes to tracing to ensure the error is captured in a logfile if `logFile` is set

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:77 on 2024-06-10 06:49_

How about creating a `GlobalSettings` struct that includes a `client_settings` fields attributed with `#[flatten]`?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:87 on 2024-06-10 06:50_

Is it required to restart the server when changing the `logFile` or `logLevel` setting? 

---

_@MichaReiser reviewed on 2024-06-10 06:51_

The code changes look good to me. I left a few questions and are waiting for a test plan that shows a few traces. 

I think some documentation on how the tracing setup would be helpful, especially because it now supports environment variables, tracing level, logLevel, log file etc. 

---

_@snowsignal reviewed on 2024-06-10 13:43_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:133 on 2024-06-10 13:43_

The difference between `messages` and `verbose` is the amount of information that gets logged, not the level of logging. Since our tracing system doesn't have separated 'verbose' information attached to its traces, I think it makes sense to treat them the same. `logLevel` should be the source of truth for the traces that get shown, IMO.

---

_@snowsignal reviewed on 2024-06-10 13:45_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:125 on 2024-06-10 13:45_

The log level doesn't change over the lifecycle of the server, though. It's the responsibility of the client to reboot the server with new settings when the client settings change.

---

_@snowsignal reviewed on 2024-06-10 13:46_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:76 on 2024-06-10 13:46_

They're filtering by different things, and I'd rather keep them separated for modularity.

---

_@snowsignal reviewed on 2024-06-10 13:46_

---

_Review comment by @snowsignal on `crates/ruff_server/src/message.rs`:41 on 2024-06-10 13:46_

Good idea!

---

_@snowsignal reviewed on 2024-06-10 13:47_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:87 on 2024-06-10 13:47_

Yes, that's how its set up right now.

---

_@MichaReiser reviewed on 2024-06-10 14:32_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:76 on 2024-06-10 14:32_

Could you add a documentation explaining what they're filtering by then so that a future me understand the motivation of keeping them different and doesn't merge them together.

---

_@snowsignal reviewed on 2024-06-10 17:16_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:131 on 2024-06-10 17:16_

Unfortunately, this is a different return type, so it wouldn't work here. We need to consolidate the log file / stderr writer into a single type here, otherwise the tracing layer would have a different type and we'd need to build it in two completely distinct branches.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:131 on 2024-06-10 17:25_

It seems to work fine for rust analyzer:

```
    let writer = match log_file {
        Some(file) => BoxMakeWriter::new(Arc::new(file)),
        None => BoxMakeWriter::new(std::io::stderr),
    };
```

---

_@MichaReiser reviewed on 2024-06-10 17:25_

---

_@MichaReiser reviewed on 2024-06-10 17:26_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:133 on 2024-06-10 17:26_

That makes sense. Maybe put this answer somewhere into the documentation.

---

_Comment by @MichaReiser on 2024-06-10 17:26_

This looks good to me. Happy to approve tomorrow morning when the Test plan's filled out. Thanks for doing another iteration on this!

---

_@snowsignal reviewed on 2024-06-10 17:28_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:131 on 2024-06-10 17:28_

Oh, I see what you mean! Nevermind 😄 

---

_Review requested from @MichaReiser by @snowsignal on 2024-06-11 04:13_

---

_@MichaReiser approved on 2024-06-11 06:23_

Nice! Thank's for taking the time to iterate on the design

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/trace.rs`:16 on 2024-06-11 06:54_

It might be useful to provide this information in the server docs either in the global docs (`crates/ruff_server/README.md`) or editor specific docs (`crates/ruff_server/docs/setup/*.md`).

---

_@dhruvmanila reviewed on 2024-06-11 06:54_

---

_Comment by @dhruvmanila on 2024-06-11 07:07_

This isn't blocking the PR but can you provide similar examples for Neovim. I'm able to set the `RUFF_TRACE` environment variable via the `cmd_env` key but not able to figure out where the `logLevel` / `logFile` should go and how should they affect the messages displayed.

---

_@snowsignal reviewed on 2024-06-11 14:24_

---

_Review comment by @snowsignal on `crates/ruff_server/src/trace.rs`:16 on 2024-06-11 14:24_

I'll update the docs in a follow-up.

---

_Comment by @snowsignal on 2024-06-11 18:29_

@dhruvmanila I've updated the PR description with a Neovim testing guide 😄 

---

_Merged by @snowsignal on 2024-06-11 18:29_

---

_Closed by @snowsignal on 2024-06-11 18:29_

---

_Branch deleted on 2024-06-11 18:29_

---
