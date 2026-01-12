```yaml
number: 4632
title: "Add `uvx` alias for `uv tool run`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/uvx
created_at: 2024-06-28T17:59:31Z
updated_at: 2024-07-02T01:42:04Z
url: https://github.com/astral-sh/uv/pull/4632
synced_at: 2026-01-12T16:06:21Z
```

# Add `uvx` alias for `uv tool run`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/4476

Originally, this used the changes in #4642 to invoke `main()` from a `uvx` binary. This had the benefit of `uvx` being entirely standalone at the cost of doubling our artifact size. We think that's the incorrect trade-off. 

Instead, we assume `uvx` is always next to `uv` and create a tiny binary (<1MB) that invokes `uv` in a child process. This seems preferable to a `cargo-dist` alias because we have more control over it. This binary should "just work" for all of our cargo-dist distributions and installers, but we'll need to add a new entry point for our PyPI distribution. I'll probably tackle support there separately?

```
❯ ls -lah target/release/uv target/release/uvx
-rwxr-xr-x  1 zb  staff    31M Jun 28 23:23 target/release/uv
-rwxr-xr-x  1 zb  staff   452K Jun 28 23:22 target/release/uvx
```

This includes some small overhead:

```
❯ hyperfine --shell=none --warmup=100 './target/release/uv tool run --help' './target/release/uvx --help' --min-runs 2000
Benchmark 1: ./target/release/uv tool run --help
  Time (mean ± σ):       2.2 ms ±   0.1 ms    [User: 1.3 ms, System: 0.5 ms]
  Range (min … max):     2.0 ms …   4.0 ms    2000 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Benchmark 2: ./target/release/uvx --help
  Time (mean ± σ):       2.9 ms ±   0.1 ms    [User: 1.7 ms, System: 0.9 ms]
  Range (min … max):     2.8 ms …   4.2 ms    2000 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Summary
  ./target/release/uv tool run --help ran
    1.35 ± 0.09 times faster than ./target/release/uvx --help
```

I presume there may be some other downsides to a child process? The wrapper is a little awkward. We could consider `execv` but this is complicated across platforms. An example implementation of that over in [monotrail](https://github.com/konstin/poc-monotrail/blob/433af5aed90921e2c3f4b426b3ffffd83f04d3ff/crates/monotrail/src/monotrail.rs#L764-L799).


---

_Label `cli` added by @zanieb on 2024-06-28 17:59_

---

_Label `preview` added by @zanieb on 2024-06-28 17:59_

---

_@zanieb reviewed on 2024-06-28 18:02_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:853 on 2024-06-28 18:02_

This moved without change from `run` above because it's not safe to send `std::env` values across threads depending on the platform.

---

_Comment by @zanieb on 2024-06-28 18:44_

It turns out cargo-dist [supports aliases](https://opensource.axo.dev/cargo-dist/book/reference/config.html?highlight=alias#bin-aliases)! but I don't think that's actually what we want here, we have more control this way.

---

_Comment by @zanieb on 2024-06-28 19:09_

I think we'd follow this with some special-casing to the help menu to display `uvx` instead of `uv tool run` when used.

---

_Review requested from @charliermarsh by @zanieb on 2024-06-28 20:25_

---

_Marked ready for review by @zanieb on 2024-06-28 20:25_

---

_Comment by @charliermarsh on 2024-06-28 21:03_

This is great, but one early concern: will this double the size of the wheel, by shipping two separate binaries that are mostly identical?

---

_Comment by @zanieb on 2024-06-28 21:14_

@charliermarsh great question — ideally we'd make this binary teeeenie tiny. I'll investigate that but it sounds like I'll need to duplicate a lot more code?

---

_Comment by @zanieb on 2024-06-28 21:55_

Mm yeah each of these is going to be large... 

```
❯ ls -lah target/debug/uv target/debug/uvx
-rwxr-xr-x  1 zb  staff    80M Jun 28 16:53 target/debug/uv
-rwxr-xr-x  1 zb  staff    80M Jun 28 16:53 target/debug/uvx
```

Maybe a cargo-dist alias _is_ the way to go.

---

_Converted to draft by @zanieb on 2024-06-28 22:34_

---

_Comment by @sgammon on 2024-06-28 23:10_

@zanieb can't `uvx` assume it is always alongside `uv`, and dispatch through the main binary? Thus it would just be an alias for a longer `uv` command, and much smaller as a result?

_Edit:_ Or, potentially, an alias, and simply detect `uvx` as the first argument in `argv`

---

_Comment by @zanieb on 2024-06-29 03:45_

Yeah I think we're going to need to do something like that. I think the binary size increase in unacceptable for us and there's no reason for `uvx` to be standalone today. I think we have options like...

- A `cargo-dist` alias
   - We could need to roll our own alias in our PyPI distributions 
   - The way the alias works differs across platforms and distribution methods
   - The sys args are not reliable and differ across platforms, can we use them to know `uvx` was invoked reliably?
- A bespoke alias
   - We would need to figure out how to distribute this across platforms
- A new binary in the `uv` binary which just dispatches to a child process
   - Does this come have acceptable overhead?
   - Can it assume that it is always next to `uv` or do we need to support some discovery?

---

_Comment by @zanieb on 2024-06-29 04:17_

Alright, this is nice and spicy.

```
❯ ls -lah target/debug/uv target/debug/uvx
-rwxr-xr-x  1 zb  staff    81M Jun 28 23:16 target/debug/uv
-rwxr-xr-x  1 zb  staff   723K Jun 28 23:16 target/debug/uvx
❯ ./target/debug/uvx
Run a tool

Usage: uv tool run [OPTIONS] <COMMAND>
```

---

_@zanieb reviewed on 2024-06-29 04:32_

---

_Review comment by @zanieb on `crates/uv/src/bin/uvx.rs`:29 on 2024-06-29 04:32_

This feels a little dubious, but is probably fine in practice. The documentation suggests the status code is just truncated to a u8 on Unix anyway? Definitely open to alternative approaches here.

---

_@zanieb reviewed on 2024-06-29 04:33_

---

_Review comment by @zanieb on `crates/uv/src/bin/uvx.rs`:14 on 2024-06-29 04:33_

In the future, this might invoke a hidden `uv x` so we can special case the help menu and such?

---

_Marked ready for review by @zanieb on 2024-06-29 04:36_

---

_@charliermarsh approved on 2024-06-30 23:47_

How did you think about the tradeoffs between this approach and using a symlink?

I'm actually shocked that this is 456 KB, so big! I wonder if we can strip it? https://github.com/johnthagen/min-sized-rust?tab=readme-ov-file#strip-symbols-from-binary


---

_Comment by @konstin on 2024-07-01 07:56_

> The sys args are not reliable and differ across platforms, can we use them to know uvx was invoked reliably?

On unix definitely, a lot of binaries rely on moonlighting as different binaries, and i found it to also work on windows. Given that cargo-dist supports them, can we build on their experience?

---

_Comment by @zanieb on 2024-07-01 17:34_

cargo-dist uses various methods depending on the distribution target, not just a symbolic link. I worry about having a different implementation across platforms, the complexity of handling this as a distribution special-case, and the loss of control. Here we have a single consistent implementation that we can invoke via `cargo run` and tested in our standard suite. We have explicit control over the entry point (rather than inferring that it's been used based on the sys arguments).

I think that we can and probably should explore alternative approaches, but this seems like the lower-risk approach to start with.

---

_Comment by @zanieb on 2024-07-01 21:14_

I can't find an obvious way to strip just one of the binaries.

---

_Merged by @zanieb on 2024-07-02 01:42_

---

_Closed by @zanieb on 2024-07-02 01:42_

---

_Branch deleted on 2024-07-02 01:42_

---
