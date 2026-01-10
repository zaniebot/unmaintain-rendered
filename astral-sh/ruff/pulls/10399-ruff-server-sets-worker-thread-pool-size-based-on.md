```yaml
number: 10399
title: "`ruff server` sets worker thread pool size based on the user's available cores"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/configurable-threads
created_at: 2024-03-14T00:57:12Z
updated_at: 2024-03-18T21:14:39Z
url: https://github.com/astral-sh/ruff/pull/10399
synced_at: 2026-01-10T22:47:02Z
```

# `ruff server` sets worker thread pool size based on the user's available cores

---

_Pull request opened by @snowsignal on 2024-03-14 00:57_

## Summary

Fixes #10369.

## Test Plan

N/A


---

_Label `server` added by @snowsignal on 2024-03-14 00:57_

---

_Comment by @github-actions[bot] on 2024-03-14 01:10_

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

_@charliermarsh reviewed on 2024-03-14 15:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:505 on 2024-03-14 15:40_

Can you set the default in Clap directly?

---

_Review requested from @charliermarsh by @snowsignal on 2024-03-15 20:42_

---

_@charliermarsh reviewed on 2024-03-17 21:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:505 on 2024-03-17 21:31_

You can omit ` - the default is 4.` This is one of the benefits of encoding it in Clap -- it's already handled in a consistent way:

```
❯ cargo run -- server --help
   Compiling ruff_python_ast v0.0.0 (/Users/crmarsh/workspace/ruff/crates/ruff_python_ast)
   Compiling ruff v0.3.2 (/Users/crmarsh/workspace/ruff/crates/ruff)
   Compiling ruff_python_parser v0.0.0 (/Users/crmarsh/workspace/ruff/crates/ruff_python_parser)
   Compiling ruff_python_literal v0.0.0 (/Users/crmarsh/workspace/ruff/crates/ruff_python_literal)
   Compiling ruff_python_semantic v0.0.0 (/Users/crmarsh/workspace/ruff/crates/ruff_python_semantic)
   Compiling ruff_python_index v0.0.0 (/Users/crmarsh/workspace/ruff/crates/ruff_python_index)
   Compiling ruff_python_codegen v0.0.0 (/Users/crmarsh/workspace/ruff/crates/ruff_python_codegen)
   Compiling ruff_python_formatter v0.0.0 (/Users/crmarsh/workspace/ruff/crates/ruff_python_formatter)
   Compiling ruff_linter v0.3.2 (/Users/crmarsh/workspace/ruff/crates/ruff_linter)
   Compiling ruff_workspace v0.0.0 (/Users/crmarsh/workspace/ruff/crates/ruff_workspace)
   Compiling ruff_server v0.2.2 (/Users/crmarsh/workspace/ruff/crates/ruff_server)
    Finished dev [unoptimized + debuginfo] target(s) in 12.36s
     Running `target/debug/ruff server --help`
Run the language server

Usage: ruff server [OPTIONS]

Options:
      --preview            Enable preview mode; required for regular operation
      --threads <THREADS>  Set the number of background threads used for running tasks concurrently - the default is 4 [default: 4]
  -h, --help               Print help

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print diagnostics, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting diagnostics)

Global options:
      --config <CONFIG_OPTION>  Either a path to a TOML configuration file (`pyproject.toml` or `ruff.toml`), or a TOML `<KEY> = <VALUE>` pair (such as you might find in a `ruff.toml` configuration file) overriding a specific
                                configuration option. Overrides of individual settings using this option always take precedence over all configuration files, including configuration files that were also specified using `--config`
      --isolated                Ignore all configuration files
```

---

_@charliermarsh approved on 2024-03-17 21:31_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule.rs`:51 on 2024-03-18 07:29_

I think we should change `Pool::new` to take a `NonZeroUsize` (and pass it as such all the way to the `::new` call) because the `Pool` is non-functioning when created with `0` (it has 0 threads, and the bounded queue has a capacity of 0, dropping all messages).

Not related to this PR but I noticed it when looking at the `Pool::new` definition. 

```
        let (job_sender, job_receiver) = crossbeam::channel::bounded(threads);
```

We should revisit the bounded queue's size and how a queue overflow is handled (currently panics). I'm saying that because using `--threads=1` created a bounded queue of size 1, and the server panics as soon as we queue the second message, which doesn't seem unlikely. I prefer a fixed message count that we allow to accumulate regardless of the thread count.

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:507 on 2024-03-18 07:36_

This is related to the summary review feedback. The documentation isn't 100% accurate OR requires very specific knowledge of the LSP implementation. The LSP creates one more (background) thread for formatting.  This is what makes me think that it might be best to hide away the thread count from users. 

---

_@MichaReiser reviewed on 2024-03-18 07:36_

The option seems very low-level and difficult to understand for users. As a user, I wouldn't know what a good default is. Should I use the number of CPU cores? If so, why does Ruff not use that by default? 

I prefer to improve Ruff's default thread count detection (e.g., don't use more than the number of CPUs) and only expose this CLI option if we have a very specific use case that it enables. 



---

_Comment by @snowsignal on 2024-03-18 17:48_

@MichaReiser I agree that we could do better than hard-coding `4` as the thread count. Do you think `clamp(1, num_cpus, 4)` could work as a default? I still think we should give users the capability of setting a worker thread count themselves (in case they need more workers running or if the default puts too high of a demand on their computer). We should probably also be clear that this doesn't reflect the actual number of threads used by the program.

---

_@snowsignal reviewed on 2024-03-18 17:50_

---

_Review comment by @snowsignal on `crates/ruff/src/args.rs`:507 on 2024-03-18 17:50_

Maybe 'worker thread count' would be a better way to describe this. `rust-analyzer` has a [similar setting](https://rust-analyzer.github.io/manual.html#rust-analyzer.numThreads).

---

_@snowsignal reviewed on 2024-03-18 18:00_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/schedule.rs`:51 on 2024-03-18 18:00_

I agree with this, though I have a question:
> We should revisit the bounded queue's size and how a queue overflow is handled (currently panics).

Is this true? The [documentation](https://docs.rs/crossbeam/latest/crossbeam/channel/fn.bounded.html) says that `send`-ing a message when the queue is at capacity will block the thread until space becomes available.

---

_Review requested from @MichaReiser by @snowsignal on 2024-03-18 20:28_

---

_@MichaReiser reviewed on 2024-03-18 20:41_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule.rs`:51 on 2024-03-18 20:41_

Oh good point. I misunderstood the API. I guess a too small bounded queue is less of a concern in that case (do you know if it would be possible to lead to dead-locks when setting the count to 1 and the server sends a notification that requires a response?)

---

_Comment by @MichaReiser on 2024-03-18 20:44_

> Do you think clamp(1, num_cpus, 4) could work as a default?

That sounds like a good default to me

> I still think we should give users the capability of setting a worker thread count themselves (in case they need more workers running or if the default puts too high of a demand on their computer).

I defer to you, but I would prefer waiting to add the option until we have a concrete use case (users asking for it). My concern with preemptively adding it is that it possibly hides problems with Ruff or the LSP that need fixing rather than having users tinker with thread counts.

* If Ruff require more than 4 threads to be responsive, than there's probably something wrong with Ruff or the LSP. Are we waiting too long for a lock? Are we re-computing too much data? Or something else? Rather than having users increase the thread count, we should investigate why Ruff is slow.
* Reducing the thread count is a more appealing use case, but using `clamp(1, numcpus)` largely addresses this without leaking the thread pool abstraction or require manual configuration. 

It's worth noting that Ruff today (as far as I understand) only uses 1 thread.



---

_@MichaReiser approved on 2024-03-18 20:47_

---

_Comment by @snowsignal on 2024-03-18 20:56_

@MichaReiser I think you've convinced me that we should not make this configurable for now. I've removed the command-line argument and kept the rest of the changes. Adding the argument back will be easy in the future, if we need it.

---

_Renamed from "Introduce `--threads` argument to `ruff server` for setting thread count" to "`ruff server` sets worker thread pool size based on the user's available cores" by @snowsignal on 2024-03-18 20:57_

---

_@snowsignal reviewed on 2024-03-18 21:06_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/schedule.rs`:51 on 2024-03-18 21:06_

> (do you know if it would be possible to lead to dead-locks when setting the count to 1 and the server sends a notification that requires a response?)

No, because any tasks active in the thread pool are not allowed to await server connections. They can _send_ requests/notifications to the server, but they can't receive messages. The sender is one half of a zero-sized channel onto a dedicated I/O thread, and that shouldn't block unless the I/O between the server and client itself starts blocking.

---

_Merged by @snowsignal on 2024-03-18 21:07_

---

_Closed by @snowsignal on 2024-03-18 21:07_

---

_Branch deleted on 2024-03-18 21:07_

---

_Comment by @MichaReiser on 2024-03-18 21:11_

Okay, I think there's one use case where a similar flag could be useful, but I don't think we need it today. That is when someone uses the LSP as our official Ruff API (instead of calling into the CLI). It could then be useful to let Ruff go as fast as possible (use up to numcpu threads). However, I don't know if I, as a user, would want to specify the thread count or instead set a `--detached` option that tells Ruff: Hey, you run out of an editor, go as fast as you can and don't bother about UI responsiveness. The advantage of a semantic option is that we could e.g. use a thread pool without priorities (less overhead). The downside is that Ruff might compete for computing resources with the calling script. It may need something of both. But let's leave that for another day.

---
