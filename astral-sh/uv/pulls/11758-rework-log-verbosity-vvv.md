```yaml
number: 11758
title: "rework log verbosity (`-vvv`)"
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: gankra/vvvvv
created_at: 2025-02-24T21:43:39Z
updated_at: 2025-02-28T23:49:29Z
url: https://github.com/astral-sh/uv/pull/11758
synced_at: 2026-01-12T16:09:59Z
```

# rework log verbosity (`-vvv`)

---

_@Gankra_

Reworks how log verbosity flags work.

* `<no argument>` is the same, equivalent to `RUST_LOG=off`
* `-v` is the same, equivalent to `RUST_LOG=uv=debug`
* `-vv` is now equivalent to `RUST_LOG=uv=trace` (previously it only enabled more log message context)
* `-vvv` is now equivalent to `RUST_LOG=trace` (previously it was equivalent to `-vv`)

The "more context" that `-vv` had has been moved to an orthogonal setting via an environment variable. Setting  `UV_LOG_CONTEXT=1` will add the extra context that `-vv` did.

In the future we may make these more granular as we try to use `info!/warn!` more.

Fixes #1569 

---

_Label `enhancement` added by @Gankra on 2025-02-24 21:43_

---

_Label `cli` added by @Gankra on 2025-02-24 21:43_

---

_@zanieb reviewed on 2025-02-24 21:48_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-24 21:48_

Curious, why this name? Should it be `UV_LOG_TREE` and/or `UV_LOG_DURATIONS`? Are these separable concepts?

---

_Review comment by @Gankra on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-24 21:51_

The two modes in the code make several random format changes, and the best I could come up with to describe them all was "more context". This extra granularity seemed a bit excessive?

---

_@Gankra reviewed on 2025-02-24 21:51_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-24 22:08_

I guess while we're touching it I'd rather have extra granularity with clear purpose than a catch-all flag that is mysterious. @konstin may have some thoughts, as he added that initial configuration.

Also cc @MichaReiser as I imagine we'll want a shared experience across the tools long-term.

---

_@zanieb reviewed on 2025-02-24 22:08_

---

_@MichaReiser reviewed on 2025-02-25 07:50_

---

_Review comment by @MichaReiser on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-25 07:50_

The behavior is already different:

* no args: info
* `-v`: info
* `-vv`: debug
* -vvv`: trace (but Red Knot / Ruff only). Also changes the output format
* Use `KNOT_LOG` for getting non ruff/Red Knot debug information

We also strip trace from release builds. 


Regarding the option. I don't have a strong opinion on its name. The main difference to me between the tree/flat layout and how we use it in Red Knot is that the tree layout not only logs messages but also includes the spans (with enter and exit events). 

I'd also consider this env var to be outside our versioning policy (unlike `UV_LOG`) so that we can iterate on naming design. E.g. should it be different env vars to switch between tree/flat but have a different env var that enables span logging for the flat layout? Or does the env var use some form of DSL?



---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-25 13:08_

My overall ideal for the log levels is:

* Default: You get information about your project only, concisely, so you can actually follow it.
* `-v`: You get information about uv and what actions it performs, e.g. uv version, current platform, steps such as lock/sync/install. This information is understandable after read the docs, you could turn this on e.g. by default on CI. 
* `-vv` (or `RUST_LOG=uv=debug`): You get debug messages. These are detailed information that expose some internals from uv, but usually written in a way that users can understand. It's the "user serviceable" level. You would run `uv ... -vv ...` and share the gist when creating a bug report 
* `RUST_LOG=...=trace` you want to get detailed, very verbose output for a specific area of uv. Requires understanding the uv codebase to interpret, targeted at developers.

I like the proposed changes in shifting us towards that, though I'd skip on exposing `RUST_LOG=trace` to the CLI, some non-uv crates tend to get really noisy at that level and we've barely ever needed that information.

---

_@konstin reviewed on 2025-02-25 13:08_

---

_@zanieb reviewed on 2025-02-25 14:32_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-25 14:32_

@MichaReiser 

> no args: info
> -v: info

So `-v` doesn't do anything?

> -vvv`: trace (but Red Knot / Ruff only). Also changes the output format

Why this decision?

> We also strip trace from release builds.

This seems dubious to me, we do need _our_ trace logs fairly often for debugging user issues. That said, we don't have an `info` layer :)

> we've barely ever needed that information

I don't know if I agree with this, I need these pretty often for debugging network problems. That said, it is noisy and ideally we'd improve our logging coverage instead.

---

_@MichaReiser reviewed on 2025-02-25 14:38_

---

_Review comment by @MichaReiser on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-25 14:38_

> So -v doesn't do anything?

It enables `info`. We only show warnings by default. The ideal Red Knot output is only diagnostics or a simple "Done" if there are no diagnostics. We use the `info` to explain what python version we inferred etc but that doesn't seem useful to show by default.

> Why this decision?

Because we don't support `UV_LOG_CONTEXT` and we've found the tree layout useful. But it has come up in the past that we want a dedicated way to switch between how the logs are represented that's independent from the verbosity

> This seems dubious to me, we do need our trace logs fairly often for debugging user issues. That said, we don't have an info layer :)

Yeah, we'll see if we need it for Red Knot, but the trace level logs are extremely detailed, and we want to avoid the perf cost.

A side note. We haven't fully figured this out on our side. But that's where we're currently at.

---

_@zanieb reviewed on 2025-02-25 14:49_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-25 14:49_

> It enables info. We only show warnings by default. The ideal Red Knot output is only diagnostics or a simple "Done" if there are no diagnostics

This makes sense. I think you wrote the wrong thing in the initial post.

> Because we don't support UV_LOG_CONTEXT and we've found the tree layout useful. But it has come up in the past that we want a dedicated way to switch between how the logs are represented that's independent from the verbosity

This is helpful. We don't support `UV_LOG_CONTEXT` either today, it's toggled by the verbosity right now. I think it makes sense for us to move towards a separate option. Do you have opinions on how that's done or named? i.e., comments on my suggestions at the top?

---

_@MichaReiser reviewed on 2025-02-25 16:13_

---

_Review comment by @MichaReiser on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-25 16:13_

> This makes sense. I think you wrote the wrong thing in the initial post.

Oh right

> Do you have opinions on how that's done or named? i.e., comments on my suggestions at the top?

Not really beyond what I mentioned in my original reply. But @sharkdp has worked with it more than I did, maybe he has some more thoughts?

---

_@sharkdp reviewed on 2025-02-26 09:23_

---

_Review comment by @sharkdp on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-26 09:23_

Not sure I understand the full picture here. I found the tree-like output very useful when working on red knot, mostly because type inference is a deeply recursive and it's often useful to see the full call stack. I'm not sure if I would find it equally useful in other code bases, but I don't really mind the auto-change in format when going from `-vv` to `-vvv`. I never felt the need to see tracing logs without the tree-like format. So I guess I would recommend to go with just one "control parameter" first, and add a second later, if needed?

---

_@zanieb reviewed on 2025-02-26 15:21_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-26 15:21_

I look at the tracing logs a lot and never in the tree format —  but that's because `-vvv` doesn't enable tracing here 🤷‍♀️. I don't have strong feelings here. It's easy to iterate on this (the formatting) too so we should bias towards just merging something.

---

_@zanieb reviewed on 2025-02-26 15:24_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-26 15:24_

I think Konsti has a fair point about `-vvv`, but I don't know what else we'd do with that level until we have a proper `info` layer and it seems fine to just have it be extremely verbose unless we start getting lots of user reports with too much logs. Since most users don't even share output with `-v`, I'm not too worried.

---

_@zanieb approved on 2025-02-26 15:24_

---

_@zanieb reviewed on 2025-02-26 15:25_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:551 on 2025-02-26 15:25_

```suggestion
    /// Add additional context and structure to log messages.
    /// If logging is not enabled, e.g., with `RUST_LOG` or `-v`, this has no effect.
```

---

_@zanieb approved on 2025-02-26 15:25_

---

_@konstin reviewed on 2025-02-26 15:40_

---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:552 on 2025-02-26 15:40_

Personally, I've never used the tree output, I think @charliermarsh found it helpful?

---

_@konstin reviewed on 2025-02-26 15:42_

---

_Review comment by @konstin on `crates/uv/src/logging.rs`:186 on 2025-02-26 15:42_

Sometimes, I sometimes find timestamps very helpful (it's direct performance information), but it's probably not worth adding a flag for it.

---

_Merged by @Gankra on 2025-02-28 23:49_

---

_Closed by @Gankra on 2025-02-28 23:49_

---

_Branch deleted on 2025-02-28 23:49_

---
