```yaml
number: 17088
title: "Add `--compile-bytecode` to `uv python install` and `uv python upgrade` to compile the standard library"
type: pull_request
state: merged
author: EliteTK
labels:
  - enhancement
assignees: []
merged: true
base: main
head: tk/py-install-bytecomp
created_at: 2025-12-11T16:37:16Z
updated_at: 2026-01-12T09:31:36Z
url: https://github.com/astral-sh/uv/pull/17088
synced_at: 2026-01-12T09:57:06Z
```

# Add `--compile-bytecode` to `uv python install` and `uv python upgrade` to compile the standard library

---

_Pull request opened by @EliteTK on 2025-12-11 16:37_

## Summary

Implement #16408.

Currently doesn't avoid recompiling the bytecode when it is already compiled, which I initially thought could be fine but then realised that it would be annoying if you have the environment variable set.

Covers upgrades, installs, reinstalls, and compiling existing installs. But not certain about the UX.

pyodide is weird and currently gets skipped by the heuristic I came up with.

## Test Plan

Styling of the status report was manually tested, there is a new test for testing the actual functionality.


---

_Review requested from @konstin by @EliteTK on 2025-12-11 16:37_

---

_Comment by @codspeed-hq[bot] on 2025-12-11 16:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/tk%2Fpy-install-bytecomp?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #17088 will **not alter performance**

<sub>Comparing <code>tk/py-install-bytecomp</code> (cdf398c) with <code>main</code> (936da00)</sub>



### Summary

`✅ 5` untouched  





---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:5871 on 2025-12-11 17:13_

I'd put something other than "critical", like "For cases where first-start time is important"

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:5875 on 2025-12-11 17:15_

I wouldn't compare to pip in a non-pip interface.

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:445 on 2025-12-11 17:19_

Is there a specific reason to sort inside here, vs. ensuring we're getting a fixed order from previous code?

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:583 on 2025-12-11 17:21_

That's elegant

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:519 on 2025-12-11 17:21_

@oconnor663 Since we talked about `.await`ing while another future is pending - you and @EliteTK both have a better understanding of this than me.

The tl;dr from that conversation is that if we await here, the download futures stall. I think we actually have to do the bytecode compilation inside the download stream and use a semaphore to limit the number of parallel stdlib compilations?

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:1123 on 2025-12-11 17:25_

Is that for pyodide?

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:5872 on 2025-12-11 17:27_

Notably, also larger directory sizes, which is the reason why the pyc files are often not shipped by default

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-11 17:28_

Doesn't that count change when we bump the patch version?

---

_Review comment by @konstin on `crates/uv/tests/it/help.rs`:583 on 2025-12-11 17:32_

The option can be in the main options section, "Installer options" generally refers to things that control wheel installs, not Python installs.

---

_@konstin reviewed on 2025-12-11 17:34_

Looks good! Added some smaller comments.

---

_@zanieb reviewed on 2025-12-11 17:40_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:5872 on 2025-12-11 17:40_

Yeah this will also increase your Docker image size, so it's a trade-off in that regard

---

_@zanieb reviewed on 2025-12-11 17:40_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6118 on 2025-12-11 17:40_

We should also document at https://docs.astral.sh/uv/guides/integration/docker/#compiling-bytecode

---

_@EliteTK reviewed on 2025-12-11 18:37_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:445 on 2025-12-11 18:37_

I wasn't sure which would be better, was actually going to ask. Sounds like it should be sorted beforehand?

---

_@EliteTK reviewed on 2025-12-11 18:41_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:583 on 2025-12-11 18:41_

Don't give me too much credit, it was taken from another similar bit of uv code when I was looking around for a simple way to put an upper bound on concurrency.

I personally didn't know buffer_unordered existed.

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:445 on 2025-12-11 18:48_

It may be that we're already using a deterministic order that's derived from the request? If we get a vec from the user input and don't iterate over hash sets or hash maps, it should already be well ordered.

---

_@konstin reviewed on 2025-12-11 18:48_

---

_@EliteTK reviewed on 2025-12-11 18:56_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:519 on 2025-12-11 18:56_

So it's not quite like that, as in, `.await`ing one future here won't stop the other one from progressing, it's more that not actively `next().await`ing the BufferUnordered future will mean that the download futures won't be progressed.

In practice this will mean for example that up to `currency.downloads` worth of futures will be started and progressed until the first of them is ready, at which point `.next().await` will complete. Now, none of the futures that are in that BufferUnordered will be polled until we `.next().await` again but the actual impact here isn't obvious as while we're not progressing the futures, the system will still keep accepting and buffering packets (until the buffers get full).

But yeah, since the compliation task can be quite long, there's a good chance it would slow things down.

We could fix this locally by creating a future which just keeps doing `.next().await` and throwing the ready downloads into an async queue, and then a second future which just calls the compiler, and we could `join!` these. But there is the loop above which handles all the pre-existing installations for which we've requested compilation, and that won't be included and currently actually stalls the downloads until it completes.

Instead, it might be best to add a wrapper which creates the queue, passes the write side to install(), passes the read side to a future which just reads from the queue and initiates bytecode compilation, and then join!s them (there are other options than join!). - I can write something quickly tomorrow morning.

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:1123 on 2025-12-11 18:58_

Yeah, but I realised we should also handle anything weird we might end up getting out of `--python-downloads-json-url`. Although I think maybe we should:

* Mention pyodide in the --help
* Maybe silence the warning by default for pyodide

---

_@EliteTK reviewed on 2025-12-11 18:58_

---

_@EliteTK reviewed on 2025-12-11 18:59_

---

_Review comment by @EliteTK on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-11 18:59_

Yes, maybe I'll take some older python version for this test so it won't change in the future.

---

_@EliteTK reviewed on 2025-12-11 20:12_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:445 on 2025-12-11 20:12_

I would have hoped so too; alas, it kept changing in the test.

I can look into why. 

---

_@EliteTK reviewed on 2025-12-12 12:30_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:445 on 2025-12-12 12:30_

Ah, I misidentified the source of the nondeterministic order. It wasn't compiling the satisfied, it was compiling the downloads, the futures complete in an arbitrary order. Well anyway, once I add the wrapper to make the compilation happen in the background, I can just make it summarise compilation step once.

---

_@EliteTK reviewed on 2025-12-12 12:34_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:519 on 2025-12-12 12:34_

I forgot to mention, the future itself won't get progressed, but if the future does something like tokio::spawn then tokio will progress the spawned task, and while I can't find anything like that in the path from this code until reqwest::Client::execute().await is called, I don't know that reqwest doesn't spawn tasks...

But either way, the compilation of satisfied things will block the downloading so it's a good idea to add the wrapper I mention anyway.

---

_@konstin reviewed on 2025-12-14 21:13_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:1123 on 2025-12-14 21:13_

Given that we know the `ImplementationName`, we should match over the interpreter kind to determine whether to compile std or not. For `LenientImplementationName::Unknown` I'm not sure, given that there's bunch of cases where I wouldn't warn, e.g. with an env var or other implicit (future) ways that get std bytecode compile, only warning with an explicit `uv python install --bytecode-compile`, and that already requires doing something so custom we don't know your Python interpreter kind, I tend to make this only a `warn!` instead.

---

_@konstin reviewed on 2025-12-14 21:15_

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-14 21:15_

I'd pick some marker that wouldn't change when our python distribution changes, like a `.pyc` file for a Python module that usually doesn't get imported (i.e. not `sys` and `os` cause our interpreter probing alone will bytecode compile it)

---

_Label `enhancement` added by @konstin on 2025-12-14 21:15_

---

_@EliteTK reviewed on 2025-12-15 15:29_

---

_Review comment by @EliteTK on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-15 15:29_

I thought about it, but the marker could stop working silently (as in, something may start using it for some unknown reason due to e.g. some change in the probing) and then the test would silently stop being a test. I think a total count is not necessarily the best metric, but as long as we pick a version of python which is old enough to never get any patch release again, it seems like it should be stable and unlikely to break if we change the interpreter probing or whatnot.

Alternatively, I could just compare the count of .py files to the count of .pyc files, or directly try to match them all up against each other.

---

_@konstin reviewed on 2025-12-15 15:48_

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-15 15:48_

Comparing `.py` to `.pyc` files sounds like a great metric, it will catch if any std files fail to compile.

---

_@EliteTK reviewed on 2025-12-16 18:16_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:519 on 2025-12-16 18:16_

So I pushed https://github.com/astral-sh/uv/pull/17088/changes/7cc9212051a3b6c656dc44a3821412a9a70caf10 which should address this.

---

_@EliteTK reviewed on 2025-12-16 18:20_

---

_Review comment by @EliteTK on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-16 18:20_

Although there are still the logs, or should I add a filter?

---

_@konstin reviewed on 2025-12-17 11:21_

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-17 11:21_

Whatever you think is best

---

_@EliteTK reviewed on 2025-12-17 14:09_

---

_Review comment by @EliteTK on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-17 14:09_

I added a filter, on windows the counts are different.

---

_@EliteTK reviewed on 2025-12-17 14:15_

---

_Review comment by @EliteTK on `crates/uv-cli/src/lib.rs`:5872 on 2025-12-17 14:15_

Should I add that to the other ones too? (the other bytecode compilation options, this was based on them)

---

_@zanieb reviewed on 2025-12-17 14:17_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:5872 on 2025-12-17 14:17_

It seems reasonable

---

_@EliteTK reviewed on 2025-12-17 14:19_

---

_Review comment by @EliteTK on `crates/uv-cli/src/lib.rs`:5872 on 2025-12-17 14:19_

Will do it as a separate PR so as to not add any more noise to this one though.

---

_@EliteTK reviewed on 2025-12-17 15:40_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:1123 on 2025-12-17 15:40_

`warn!` or `warn_user!`?

As I understand it, `warn!` will only show up with --verbose?

Regarding `LenientImplementationName::Unknown` - we don't support that for managed installs right now. Or at least we panic if we hit it. Not sure if you could trigger that with a custom `downloads.json`.

In any case, the heuristic I've come up with is: if the command (install or upgrade) was ran with explicit targets, warn for any targets which are unsupported (pyodide currently), otherwise if there were no explicit targets, don't warn. Regardless of the env var or not.

There is now also always a separate warning if you pass that check but your stdlib is in a weird place.

---

_@konstin reviewed on 2025-12-17 16:28_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:1123 on 2025-12-17 16:28_

ok, sounds good!

---

_@EliteTK reviewed on 2025-12-17 17:09_

---

_Review comment by @EliteTK on `crates/uv-cli/src/lib.rs`:6118 on 2025-12-17 17:09_

Not sure how far I should go, I just added this for now:

https://github.com/astral-sh/uv/blob/539cf72671e6a6f7ea34b277e90ee9e9ea704f72/docs/guides/integration/docker.md?plain=1#L337-L347

---

_@konstin reviewed on 2025-12-18 12:25_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:6118 on 2025-12-18 12:25_

I'd add a note that we're only compiling the standard library if it's a managed Python interpreter, I don't think people realize atm that this makes a perf difference and notably that `python:3` is slow in that regard (with the caveats in https://github.com/astral-sh/uv/issues/16378, ignoring the whole story that we could compile system interpreter's std, but only if we have permissions, unless the system interpreter already comes with pyc files (e.g. `debian` with `apt install python3` does, but `python:3` doesn't), but that's a discussion for a GitHub thread. Simply recommending PBS avoids this whole decision tree.)

---

_@konstin approved on 2025-12-18 12:52_

---

_@zanieb reviewed on 2025-12-18 13:18_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:335 on 2025-12-18 13:18_

I wonder if we should switch to recommending the `UV_COMPILE_BYTECODE` variable instead?

---

_@zanieb reviewed on 2025-12-18 13:19_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6118 on 2025-12-18 13:19_

I think I'd say 

> at the cost of increased installation time and image size



---

_Review comment by @EliteTK on `docs/guides/integration/docker.md`:335 on 2025-12-18 13:19_

It is recommended below that section, but I assume you mean swapping them around and recommending it first?

---

_@EliteTK reviewed on 2025-12-18 13:19_

---

_@zanieb reviewed on 2025-12-18 13:21_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6118 on 2025-12-18 13:21_

I think it makes sense to have Konsti's recommendation in an admonition, e.g.,

```
!!! note

     uv will only compile the standard library of _managed_ Python versions during
    `uv python install`. The distributor of unmanaged Python versions decides if the
    standard library is pre-compiled. For example, the official `python` image will not
    have a compiled standard library.
```

---

_@zanieb reviewed on 2025-12-18 13:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:1195 on 2025-12-18 13:24_

This could be pretty annoying if `UV_COMPILE_BYTECODE` is just set globally? I think we usually want _warnings_ to be actionable, and this one isn't really. I might just move this into verbose logs?

---

_@zanieb reviewed on 2025-12-18 13:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:1219 on 2025-12-18 13:25_

What does it mean for it to have "misreported its location"? I don't think this will make sense to a user. What action can they take to resolve this?

---

_@zanieb reviewed on 2025-12-18 13:27_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:335 on 2025-12-18 13:27_

Ah sorry that's not in the diff, one sec...

---

_@zanieb reviewed on 2025-12-18 13:27_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:335 on 2025-12-18 13:27_

eh this seems reasonable as-is, nevermind

---

_@zanieb reviewed on 2025-12-18 13:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:1195 on 2025-12-18 13:40_

We could differentiate between explicit `--compile-bytecode` and implicit `UV_COMPILE_BYTECODE` as we just did for `UV_GIT_LFS`, I guess. Then the action is "remove the flag", but it's weird if you do

```
uv python install cpython-3.12 pyodide-3.12 --compile-bytecode
```

---

_@zanieb reviewed on 2025-12-18 13:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:1195 on 2025-12-18 13:42_

(also discussed a bit in https://github.com/astral-sh/uv/pull/17088#discussion_r2627564730)

---

_@EliteTK reviewed on 2025-12-18 13:45_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:1195 on 2025-12-18 13:45_

I had spoken about the warnings stuff with konsti yesterday, see the new commit, maybe that's better?

---

_@EliteTK reviewed on 2025-12-29 14:17_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:217 on 2025-12-29 14:17_

Should we propagate errors or just quietly log them and mark those installs as skipped?

---

_Review requested from @konstin by @EliteTK on 2025-12-30 18:25_

---

_Review requested from @zanieb by @EliteTK on 2025-12-30 18:25_

---

_@konstin reviewed on 2026-01-06 09:18_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:1219 on 2026-01-06 09:18_

Talk to us and/or the maintainer of that Python distribution to get that sysconfig fixed, otherwise it's just silently broken.

---

_@konstin reviewed on 2026-01-06 09:24_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:217 on 2026-01-06 09:24_

I'd start with an error, there's no known reason this would be valid to fail and we're already most lenient in the compilation itself. As a user who decided to activate bytecode compilation in my deploy pipeline, I'd want a compilation failure to fail the pipeline.

---

_@EliteTK reviewed on 2026-01-06 15:39_

---

_Review comment by @EliteTK on `crates/uv/src/commands/python/install.rs`:217 on 2026-01-06 15:39_

Alright, I'll leave it as is then.

---

_@konstin approved on 2026-01-12 09:18_

---

_Merged by @EliteTK on 2026-01-12 09:31_

---

_Closed by @EliteTK on 2026-01-12 09:31_

---

_Branch deleted on 2026-01-12 09:31_

---
