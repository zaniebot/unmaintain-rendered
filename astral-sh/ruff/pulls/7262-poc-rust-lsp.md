```yaml
number: 7262
title: "POC: Rust LSP"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: rust-based-lsp
created_at: 2023-09-10T18:08:47Z
updated_at: 2024-05-08T12:41:47Z
url: https://github.com/astral-sh/ruff/pull/7262
synced_at: 2026-01-10T22:05:26Z
```

# POC: Rust LSP

---

_Pull request opened by @MichaReiser on 2023-09-10 18:08_

# Rust LSP 

This PR adds a Rust-based LSP to ruff. You can start it by running `ruff lsp`. It receives messages over `stdin` and writes responses over `stdout`. `stderr` is used for logging. 

## Getting started

1. Create a debug build of ruff. 
2. Open the VS Code extension in VS Code (`editors/vscode`)
3. Run `nom install` in `editors/vscode`
4. Install the recommended extensions
4. Launch the extension (`F5`) or *Run and Debug* / *Run Extension*
5. Open the *User Settings* and configure the path to the ruff binary: `"ruff.lspBin": "<path>/target/debug/ruff"`
6. Open a Python file

## Advantages

* Keeps all open files in memory to enable multi-file analysis (access to unsaved changes) eventually
* Avoids the overhead of starting a new process, discovering the configuration, etc., on every keystroke by using a long-running server instead
* One less dependency (ruff-lsp)
* Removes the need to expose information in our JSON output or CLI only for the VS code extension.
* Removes the Python dependency from the VS Code extension (including the extension depending on the Python extension), removing concerns about the minimal supported Python version.

## Development tools
* You can debug the VS code extension inside VS Code or by opening the developer tools: Cmd+Shift+P: *Developer: Toggle Developer Tools* in the VS Code instance running the extension.
* You can set the `ruff.trace.server` setting to `verbose` to get `trace` level `tracing` in the `Output: Ruff` panel and see all messages between client and server in the new `Output: Ruff Trace` panel.

## Implemented functionality
* UTF-8, UTF-16, and UTF-32 position encoding
* Incremental document updates. The LSP stores all open documents in memory
* Multi-workspace support
* Linting with code fixes
* Formatting: Only transmits changed lines instead of the whole file
* Configuration caching: Cache the workspace configurations. Reloads them on change.

## Challenges and open questions

### Tower-LSP or LSP-server
The most known crates for building an LSP server with rust are [tower-lsp](https://crates.io/crates/tower-lsp) and [lsp-server](https://crates.io/crates/lsp-server/0.7.4). This prototype uses tower-lsp, but I think it's worth considering lsp-server. 

#### tower-lsp

* A framework in the spirit of *You don't call us, we call you*. It controls the main loop and the messaging for you. 
* Based on `async`. Uses `tokio` by default 
* Provides a trait with methods for the most common operations. Giving you a sense of a typed API ;) 
* Handles some invariants for you: Disallowing multiple `initialize` messages. Not accepting messages after `shutdown`. 
* Long [standing issue](https://github.com/ebkalderon/tower-lsp/issues/284) about concurrent handler execution (race conditions)

#### lsp-server

* Used by rust-analyzer
* A library in the sense that it doesn't control the main loop. It only handles the message dispatching between client and server but leaves the scheduling to the user. 
* Being in control of the main loop allows more advanced scheduling. E.g. [rust-analzyer](https://github.com/rust-lang/rust-analyzer/blob/b3f45745ea0502e664711f130f9d01f87b99fd27/crates/rust-analyzer/src/main_loop.rs#L708-L787) does:
	* Uses the main thread to mutate the global plugin state. 
	* Uses a high-priority thread for on-type requests (format on type)
	* Uses a dedicated formatter thread
	* Use higher priority threads for latency-sensitive analysis
	* Use lower-priority threads for everything else. 
* Doesn't use `async` Rust, which is less useful for code analysis because most tasks are CPU and not IO bound.
* Hypothesis: Controlling the scheduling may help to avoid thread-safe structures if it is known that some data is only ever accessed from the main thread. 

[Example](https://github.com/rust-lang/rust-analyzer/blob/master/lib/lsp-server/examples/goto_def.rs)

### VS Code Extension migration

This prototype doesn't explore how to support the old Python-based LSP and the ruff-integrated LSP in a single extension. It's further unclear how the new LSP would support the `args` setting (The setting is already problematic today because `format` and `check` don't support the same arguments).

A possible approach could be that the extension detects the version of the ruff binary and either spawns `ruff-lsp` (Python LSP) or `ruff lsp` (Rust based LSP)

### Ruff binary discovery
This prototype doesn't explore how to discover the `ruff` binary for the current project (and using the bundled version for untrusted workspaces). It defaults to `ruff.lspBin`.

### Higher level `lint` and `format` abstractions

The `ruff_workspace` crate provides most functionality needed by the LSP, but not with the right granularity and laziness. This either results in code duplication between the CLI and the LSP or unnecessary work:

* The LSP resolves the settings for each open workspace by using `PyprojectConfig::resolve` and `python_files_in_path`.
	* `python_files_in_path` resolves all settings **and** python files included in the project. However, the LSP only needs the settings. Building up a list of all files in the project is wasted work.
	* Ideally, the `Setting` for a path would be resolved lazily rather than eagerly. 
* The LSP should only lint and format files that are part of the project (included and not excluded). There's no existing API to check whether a file is part of the Ruff project (the CLI uses the files returned by `python_files_in_path`)
* The detection of a file's source type would need to be replicated in the LSP.

To sum it up. We lack high-level APIs for lazy linting or formatting of a file to avoid duplicating concerns performed by the CLI.

### Concurrency

> Note: I'm new to writing concurrent Rust code. 

The fact that tower-lsp uses tokio forces the implementation to use thread-safe (`Send` and `Sync`) data structure for the session state. This makes accessing and mutating the configuration awkward and introduces much complexity. It would be nice if we could minimize the need for thread-safe data structures and locking.

However, this problem isn't only caused by tower-lsp. I see us facing similar issues when implementing multi-file analysis where we suddenly need a way to snapshot the project state and run the analysis on the snapshot (to avoid new user modifications causing inconsistent states). 


## Notes

It isn't necessary to integrate the VS code extension into this repository. I added it to this repository to have a single PR that includes all changes (from plugin to ruff)

---

_Closed by @MichaReiser on 2023-09-12 05:49_

---

_Reopened by @MichaReiser on 2023-09-12 05:49_

---

_Renamed from "Avoid allocating in lex_decimal" to "POC: Rust LSP" by @MichaReiser on 2023-09-12 05:49_

---

_Comment by @MichaReiser on 2023-09-12 05:50_

Current dependencies on/for this PR:
* main
  * **PR #7262** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7262" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7262?utm_source=stack-comment).

---

_Comment by @codspeed-hq[bot] on 2023-09-25 13:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/rust-based-lsp)

### Merging #7262 will **improve performances by 2.9%**

<sub>Comparing <code>rust-based-lsp</code> (a670128) with <code>main</code> (8028de8)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `rust-based-lsp` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 18.6 ms | 18.1 ms | +2.9% |


---

_Closed by @MichaReiser on 2023-12-14 10:50_

---

_Comment by @micahscopes on 2024-01-29 14:59_

This is a very nice writeup.  What did you end up deciding on?

---

_Comment by @MichaReiser on 2024-01-29 15:05_

> This is a very nice writeup. What did you end up deciding on?

Thanks. We have yet to decide. We hope that @InquisitivePenguin can work and this soon. 

Are you working on something similar? 

---

_Comment by @micahscopes on 2024-01-29 18:56_

@MichaReiser Yes, I'm working on a language server for [Fe language](https://fe-lang.org/).  I recently prototyped a switch from lsp-server to tower-lsp and I've documented some of my thoughts so far here: https://github.com/ethereum/fe/pull/979

Long story short, it seems that there's no way of making concurrency problems magically simple.  Waving `async/await` around at them isn't going to make them go away.  I'm tempted to try stream extensions for this because of my existing familiarity with javascript "FRP" libraries and for their flexibility and expressiveness.  In that case though the choice between tower-lsp and lsp-server probably doesn't matter too much.  Tower-lsp has that nice simple API for implementing LSP features and is simple to set up.

We are using salsa2022 in our compiler libraries, which alleviates some of the pressure of getting this stuff perfect.  A lot of computations will be reused as long as inputs haven't changed.

---

_Comment by @MichaReiser on 2024-01-30 06:57_

@micahscopes Thanks for sharing your findings and the pull request. It will help us evaluating tower-lsp and lsp-server. 

> Long story short, it seems that there's no way of making concurrency problems magically simple. 

I didn't expect that. There's an inherent complexity that the LSP protocol allows multiple in-flight requests. I'm mainly wondering if there's an opportunity to avoid some complexity introduced by async and await (and async runtimes) because the LSP-server can give us more fine-grained control of when we want to run something on different threads to avoid concurrency-safe data structures potentially. The reason why I feel that async (or at least tokio) might not be the right fit is because most of our operations aren't IO bound and, therefore, shouldn't run in a regular tokio worker. 

That said. I'm sure your PR will help me understand why the trade of more fine grained control doesn't outweigh the first-class language integration that you get with async await.

---

_Comment by @micahscopes on 2024-01-30 19:55_

> I didn't expect that. There's an inherent complexity that the LSP protocol allows multiple in-flight requests. 

Exactly, regardless of lsp-server vs tower-lsp, it's still necessary to face this complexity!  Today I'm feeling curious about using tower-lsp along with separate thread(s) for long running tasks.

> I'm mainly wondering if there's an opportunity to avoid some complexity introduced by async and await (and async runtimes) because the LSP-server can give us more fine-grained control of when we want to run something on different threads to avoid concurrency-safe data structures potentially

Using async/await wouldn't necessarily preclude that kind of fine-grained control, right?  For example, if you wanted some state to be on a thread and didn't want to share it, you could still use channels to communicate to/from tower-lsp handlers, couldn't you?  I.e. the channel endpoints would be the only "shared state" you allow in the tower-lsp object.

But I hear your concern about the complexity of async/await/executors.  Is it possible to create something maintainable and easy to fix if things go wrong?  Does async/await still provide advantages even when using explicit channels and threads?

By the way, you've probably seen this already but I found this to be very helpful: https://tokio.rs/tokio/tutorial/shared-state.

---

_Comment by @morgante on 2024-01-31 04:10_

In developing the Grit [language server](https://docs.grit.io/language/overview) we faced some similar challenges with the async language server.

We ended up moving all long-running or compute-intensive work to a separate thread which we communicate with via channels. So far this is working nicely, but you do need to do a bit of work to make sure state messages aren't sent to the client.

---

_Comment by @MichaReiser on 2024-01-31 08:22_

Hy @morgante :wave: Oh that's interesting. Do I understand it correctly that you used the `async`/`await` only as the LSP facade (to handle async IO operations) but use your own scheduler (a separate thread for now) to do the work? 

That sounds very clever, making use of the async-await ergonomics where it matters but avoid it "infecting" all the downstream infrastructure and keeping some control over scheduling.

---

_Comment by @morgante on 2024-01-31 08:30_

Exactly, with the caveat that we do still keep some very fast things (ex. looking up available patterns) in the main LSP thread.

---

_Comment by @micahscopes on 2024-01-31 15:13_

@morgante thank you for sharing your experience, it's encouraging to hear that you've made this work. Async/await appeals to me in that it simplifies keeping track of pending requests and related responses; no need for extra data structures to keep track of pending request IDs or any of that.

> So far this is working nicely, but you do need to do a bit of work to make sure state messages aren't sent to the client

Can you elaborate on the part about preventing state messages from being sent to the client? I don't understand.

---

_Comment by @morgante on 2024-02-15 07:34_

> Can you elaborate on the part about preventing state messages from being sent to the client? I don't understand.

Depending on how long async execution takes, documents will often have changed before the results are sent to the client. The `version` parameter can help with this, but it's even better to dedupe expensive in-progress work if you can and not even bother sending a diagnostic notification for an old version.

---

_Branch deleted on 2024-05-08 12:41_

---
