```yaml
number: 15017
title: "Add an experimental `uv format` command"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/format-ruff
created_at: 2025-08-01T19:05:04Z
updated_at: 2025-08-26T19:42:59Z
url: https://github.com/astral-sh/uv/pull/15017
synced_at: 2026-01-12T16:11:32Z
```

# Add an experimental `uv format` command

---

_@zanieb_

As a frontend to Ruff's formatter.

There are some interesting choices here, some of which may just be temporary:

1. We pin a default version of Ruff, so `uv format` is stable for a given uv version
2. We install Ruff from GitHub instead of PyPI, which means we don't need a Python interpreter or environment
3. We do not read the Ruff version from the dependency tree

See https://github.com/astral-sh/ruff/pull/19665 for a prototype of the LSP integration.



---

_Marked ready for review by @zanieb on 2025-08-07 20:53_

---

_Review requested from @konstin by @zanieb on 2025-08-18 21:47_

---

_Comment by @zanieb on 2025-08-18 21:48_

@konstin can you review please? I want to build on the abstractions added here.

---

_Review comment by @konstin on `docs/reference/cli.md`:1844 on 2025-08-19 08:34_

Why does it say `[COMMAND]` here?

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:34 on 2025-08-19 08:38_

```suggestion
            Binary::Ruff => Version::new([0, 12, 5]),
```

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:52 on 2025-08-19 08:40_

I wouldn't couple our GitHub release URLs to the PEP-controlled source dist extension.

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:73 on 2025-08-19 08:43_

Do we need docstrings here if the `#[error()]` says the same?

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:78 on 2025-08-19 08:45_

We should use the actual types instead of converting to string eagerly

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:96 on 2025-08-19 08:46_

We should avoid anyhow

```suggestion
        source: uv_extract::Error,
```

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:281 on 2025-08-19 08:49_

These need to use `fs_err` instead of `tokio`

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:149 on 2025-08-19 09:10_

Why is this implementation different from the one in the distribution database?

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:287 on 2025-08-19 09:13_

```suggestion
/// Cast platform types to the cargo dist target triple format used in the GitHub release page.
```

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:188 on 2025-08-19 09:15_

```suggestion
    if cache_entry.path().exists() {
```

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:201 on 2025-08-19 09:16_

`tokio::fs` does not attach the path to the error message, we need to use `fs_err::tokio` instead. 

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:204 on 2025-08-19 09:18_

Why are we using the parent of the cache dir?

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:348 on 2025-08-19 09:22_

Do we need a retry loop for stream errors?

---

_Review comment by @konstin on `crates/uv/src/commands/project/format.rs`:55 on 2025-08-19 09:24_

Why is the reporter optional if we always have a reporter?

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:249 on 2025-08-19 09:25_

Given that we know that we always have a reporter, we can remove the second branch

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:109 on 2025-08-19 09:27_

We should show consistent context (name, version, url) for each branch. We're creating basically all the errors in `bin_install`, so we can also attach this context at the callsite of `bin_install`

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:64 on 2025-08-19 09:28_

Is this error reachable / can we avoid this error variant?

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:280 on 2025-08-19 10:22_

We should respect the user's umask and only add the executable bit, as we do in https://github.com/astral-sh/uv/blob/2677e85df952d2ec6908380dd1c3d6a4f52b5e5a/crates/uv-extract/src/stream.rs#L207-L214

---

_Review comment by @konstin on `crates/uv/tests/it/format.rs`:242 on 2025-08-19 10:37_

Why are we requesting Python 3.11 here?

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:1007 on 2025-08-19 10:39_

Maybe "Format Python code"? It can also format jupyter notebooks, and I think "code formatter" is the more familiar term. On the Ruff side, the docstring is:

> Run the Ruff formatter on the given files or directories

---

_Review comment by @konstin on `crates/uv/src/commands/project/format.rs`:38 on 2025-08-19 10:47_

Can we have an `Option<Version>` in clap directly?

---

_Review comment by @konstin on `crates/uv/src/commands/project/format.rs`:74 on 2025-08-19 10:55_

```suggestion
        command.args(args.iter());
```

---

_Review comment by @konstin on `crates/uv/src/commands/reporters.rs`:844 on 2025-08-19 11:00_

What does the `Some(1)` here do?

---

_@konstin reviewed on 2025-08-19 11:03_

---

_@zanieb reviewed on 2025-08-19 13:54_

---

_Review comment by @zanieb on `docs/reference/cli.md`:1844 on 2025-08-19 13:54_

Because we take arbitrary options. I'll fix that, thanks!

---

_Review comment by @zanieb on `crates/uv-bin-install/src/lib.rs`:52 on 2025-08-19 13:54_

I didn't want to but this is the type I needed for the extraction. I can look into it again though. It seemed minor.

---

_@zanieb reviewed on 2025-08-19 13:54_

---

_@zanieb reviewed on 2025-08-19 13:55_

---

_Review comment by @zanieb on `crates/uv-bin-install/src/lib.rs`:73 on 2025-08-19 13:55_

Naw that's AI slop — I'll clean it up. Thanks!

---

_@zanieb reviewed on 2025-08-19 14:47_

---

_Review comment by @zanieb on `crates/uv-bin-install/src/lib.rs`:287 on 2025-08-19 14:47_

I'm sort of trying to abstract over the cargo dist / GitHub release part here. It's "binary ..." because this is the "binary install" crate. I expect this will change as I explore things like CUDA targets.

---

_@zanieb reviewed on 2025-08-19 14:49_

---

_Review comment by @zanieb on `crates/uv-bin-install/src/lib.rs`:188 on 2025-08-19 14:49_

I sort of always prefer `try_exists` per

```
/// [`Path::exists()`] only checks whether or not a path was both found and readable. By
/// contrast, `try_exists` will return `Ok(true)` or `Ok(false)`, respectively, if the path
/// was _verified_ to exist or not exist. If its existence can neither be confirmed nor
/// denied, it will propagate an `Err(_)` instead. This can be the case if e.g. listing
/// permission is denied on one of the parent directories.
```

I think it's just more explicit. But `exists` does seem fine here too.

---

_@zanieb reviewed on 2025-08-19 14:52_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1007 on 2025-08-19 14:52_

I specifically didn't want

> given files or directories

because uv should be handling that selection for you.

I also didn't want to say "The Ruff formatter".

I guess I'd consider notebooks a form of Python files?

I can look at rephrasing this though.

---

_@konstin reviewed on 2025-08-19 15:01_

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:287 on 2025-08-19 15:01_

I'm looking to clarify that the string we get back is a rust-like, cargo dist / GitHub releases target triple, to tell them apart from all the other platform target strings we have.

---

_@konstin reviewed on 2025-08-19 15:12_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:1007 on 2025-08-19 15:12_

Can you add something around this to the docstring to clarify how `uv format` conceptually differs from `ruff format`?

---

_@konstin reviewed on 2025-08-19 15:12_

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:188 on 2025-08-19 15:12_

I got confused reading this as I was expecting `try_exist` to use the additional error information but then didn't.

---

_@zanieb reviewed on 2025-08-19 15:54_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1007 on 2025-08-19 15:54_

It actually doesn't differ at this time, other than version handling. I'll see what makes sense to say.

---

_@konstin reviewed on 2025-08-19 16:29_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:1007 on 2025-08-19 16:29_

We can also start with something simpler and update the docstring as we diverge.

---

_@zanieb reviewed on 2025-08-19 21:06_

---

_Review comment by @zanieb on `crates/uv-bin-install/src/lib.rs`:64 on 2025-08-19 21:06_

I don't mind being defensive here since the platform can be an arbitrary string. I think this will evolve as there are more `Binary` variants and it'll be clearer if we need it.

---

_@zanieb reviewed on 2025-08-19 21:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/reporters.rs`:844 on 2025-08-19 21:34_

This is just a copy of https://github.com/astral-sh/uv/blob/5de39b5d09119ed9a9c163c1b250bb15d87f4e33/crates/uv/src/commands/reporters.rs#L638-L652 without support for multiple items yet.

I guess we shouldn't use a `MultiProgress`? I don't really mind using it for consistency though.

---

_@zanieb reviewed on 2025-08-19 21:37_

---

_Review comment by @zanieb on `crates/uv-bin-install/src/lib.rs`:348 on 2025-08-19 21:37_

Good call.

---

_Label `preview` added by @zanieb on 2025-08-20 14:45_

---

_Comment by @zanieb on 2025-08-20 14:46_

I believe I've addressed all the feedback.

---

_Review requested from @konstin by @zanieb on 2025-08-20 16:02_

---

_@konstin reviewed on 2025-08-20 18:40_

---

_Review comment by @konstin on `crates/uv/src/commands/reporters.rs`:844 on 2025-08-20 18:40_

Non-blocker here (an aesthetic problem if it shows up all, pre-existing code), but setting the length to `1` instead of `None` seems off, shouldn't we either set a real value or nothing?

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:208 on 2025-08-20 18:48_

```suggestion
    // Add executable bit
    #[cfg(unix)]
```

---

_@zanieb reviewed on 2025-08-20 18:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/reporters.rs`:844 on 2025-08-20 18:54_

Yeah I.. don't really know much about what's going on here. We can open an issue to dig into it.

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:319 on 2025-08-20 19:31_

We need to attach retries here if any happened in the middleware, through the same pattern as https://github.com/astral-sh/uv/pull/13897/files#diff-a327e08abfc6c0f1a83a8c92ad294c17f1b410f5133b1e81ccbdc76008c1f8b0R572-R575

---

_@konstin approved on 2025-08-20 19:51_

We need to fix the retries thing, everything else looks good

---

_Merged by @zanieb on 2025-08-21 11:33_

---

_Closed by @zanieb on 2025-08-21 11:33_

---

_Branch deleted on 2025-08-21 11:33_

---

_@JelleZijlstra reviewed on 2025-08-21 20:01_

---

_Review comment by @JelleZijlstra on `docs/reference/cli.md`:1849 on 2025-08-21 20:01_

Did you mean to write something else here? Maybe the "By default," part should be removed.

---

_@zanieb reviewed on 2025-08-21 20:56_

---

_Review comment by @zanieb on `docs/reference/cli.md`:1849 on 2025-08-21 20:56_

Oops, I was changing the sentence and missed that. Thank you!

---

_Comment by @mgaitan on 2025-08-22 19:24_

how is it different to uvx ruff format ?  seems a bit weird to be honest. 

---

_Comment by @shawnoster on 2025-08-26 19:42_

I know this is merged but not a huge fan of this. Is the `uv` roadmap for it to become yet another bloated tool. Calling two tools is not difficult and adheres to the the Unix tradition of a tool doing one thing and one thing well. Just one more thing to maintain in a place that doesn't need it.

Do as you do but architecturally this is a bad choice.

---
