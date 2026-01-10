---
number: 14720
title: "Move environment variable parsing to `EnvironmentOptions`"
type: issue
state: open
author: zanieb
labels:
  - good first issue
  - help wanted
  - internal
assignees: []
created_at: 2025-07-18T12:56:38Z
updated_at: 2025-10-22T20:33:51Z
url: https://github.com/astral-sh/uv/issues/14720
synced_at: 2026-01-10T01:25:48Z
---

# Move environment variable parsing to `EnvironmentOptions`

---

_Issue opened by @zanieb on 2025-07-18 12:56_

In https://github.com/astral-sh/uv/pull/14662 I added a new `EnvironmentOptions` abstraction which allows us to express semantics that Clap cannot and perform up-front parsing of non-CLI environment variables, e.g., https://github.com/astral-sh/uv/pull/14544 and https://github.com/astral-sh/uv/pull/14369.

We should move other extraneous environment variables to be parsed there and provided through our usual settings structs.

These changes should be relatively easy. We should probably do it one variable at a time. 

- [x] `UV_HTTP_RETRIES`
- [ ] `UV_COMPILE_BYTECODE_TIMEOUT`
- [x] `UV_HTTP_TIMEOUT`
- [x] `UV_REQUEST_TIMEOUT`
- [x] `HTTP_TIMEOUT`
- [ ] `UV_RUN_RECURSION_DEPTH`
- [ ] `UV_RUN_MAX_RECURSION_DEPTH`
- [x] `UV_CONCURRENT_DOWNLOADS`
- [x] `UV_CONCURRENT_BUILDS`
- [x] `UV_CONCURRENT_INSTALLS`
- [x] `UV_PYTHON_INSTALL_MIRROR`
- [x] `UV_PYPY_INSTALL_MIRROR`
- [x] `UV_PYTHON_DOWNLOADS_JSON_URL`
- [x] `UV_NO_GITHUB_FAST_PATH`
- [ ] `UV_GITHUB_FAST_PATH_URL`
- [ ] `UV_GIT_LFS`
- [ ] `UV_CUDA_DRIVER_VERSION`
- [ ] `UV_AMD_GPU_ARCHITECTURE`
- [ ] `UV_STACK_SIZE`
- [x] `UV_LOG_CONTEXT`
- [ ] `TRACING_DURATIONS_FILE`
- [ ] `UV_LOCK_TIMEOUT` (https://github.com/astral-sh/uv/pull/16342#discussion_r2448547512)

---

_Label `help wanted` added by @zanieb on 2025-07-18 12:56_

---

_Label `internal` added by @zanieb on 2025-07-18 12:56_

---

_Comment by @zanieb on 2025-07-18 12:58_

For people that want to help out, it'd be good to search for other variables I should add to the todo list.

---

_Label `good first issue` added by @zanieb on 2025-07-18 12:58_

---

_Comment by @zanieb on 2025-07-18 17:53_

I pulled a bunch with AI, feel free to say that we shouldn't apply it for a given variable if it doesn't make sense.

---

_Comment by @Zaloog on 2025-07-30 16:43_

Hi @zanieb may I take this one?

Just started learning rust (motivated by astrals work in the python field), but this looks like a good issue to start getting more into it.
I might need some guidance on the way though :)

Since the boolean parsing is already defined, adding the boolean env vars to `EnvironmentOptions` should be relative straightforward or?
What do you mean exactly by `one variable at a time`?

Best regards

---

_Comment by @zanieb on 2025-07-30 16:54_

Hey @Zaloog, yeah these should be fairly straight-forward. By "one at a time" I mean start with just (for example) `UV_HTTP_RETRIES` and make sure we're aligned on the pattern instead of moving _all_ of these variables at once.

---

_Comment by @Zaloog on 2025-07-30 17:59_

like that @zanieb? 

---

_Comment by @zanieb on 2025-07-30 18:40_

You'll then need to propagate it to where it's used. 

---

_Comment by @Zaloog on 2025-07-31 15:53_

the `UV_NO_GITHUB_FAST_PATH` env var is used in the `uv-git` crate [here](https://github.com/astral-sh/uv/blob/fa24d9a5e20434ea8c142f36d13942a9645b5cdf/crates/uv-git/src/resolver.rs#L76), 

Id think that I have to add `uv-settings` as workspace dependency to `uv-git` to propagate the EnvironmentsOptions,
I tried to do that, it fails though due to a circular import.
 
Or is there a different approach im not aware of?


---

_Comment by @zanieb on 2025-07-31 15:59_

Can't you propagate the setting without passing the entire settings struct?

---

_Comment by @zanieb on 2025-07-31 16:02_

You may have picked a hard setting to start with, because the `GitResolver` doesn't currently store state so you'll either need to make it hold settings or propagate the setting via invocations to the fast path by exposing it on the `BuildContext`. I'm not sure which is cleaner without digging deeper! We do stash some settings on `GitSource` so maybe that's the place to do so?

---

_Referenced in [astral-sh/uv#15001](../../astral-sh/uv/pulls/15001.md) on 2025-07-31 17:27_

---

_Comment by @Zaloog on 2025-07-31 17:34_

I started with the `UV_LOG_CONTEXT`, the other two booleans might be more effort like you mentioned above.

Is the approach on propagating it correct in that case?

For the `int`-like environment variables, id take a look on how the parsing is done on some current implementation and then
implement a function similar to `parse_boolish_environment_variable` but for integers.

For the string value ones, I think we could break those down further e.g. in environment variables which expect a pathlike string.

But id focus on the `int` ones first if thats ok

---

_Comment by @Zaloog on 2025-08-17 12:31_

I currently do not have much time to continue working on this one. So if anyone else want to take over, feel free

---

_Comment by @andrey-berenda on 2025-09-18 14:21_

Hi
I want to start with this issue and I think I will start with `UV_REQUEST_TIMEOUT` env variable

---

_Comment by @zanieb on 2025-09-18 14:25_

Feel free!

---

_Referenced in [astral-sh/uv#15937](../../astral-sh/uv/pulls/15937.md) on 2025-09-18 20:37_

---

_Comment by @andrey-berenda on 2025-09-18 20:56_

I created the PR #15937

This PR is not small because I need to pass `environment` to a lot of places
Next PR will be smaller

Can anyone take a look? 

---

_Referenced in [astral-sh/uv#16031](../../astral-sh/uv/pulls/16031.md) on 2025-09-25 20:31_

---

_Comment by @andrey-berenda on 2025-09-25 20:46_

I've opened PR #16031. Could someone take a look?

---

_Referenced in [astral-sh/uv#16040](../../astral-sh/uv/pulls/16040.md) on 2025-09-26 13:10_

---

_Comment by @andrey-berenda on 2025-09-26 13:38_

One more PR #16040. This one is for parsing http-timeout


---

_Referenced in [astral-sh/uv#16109](../../astral-sh/uv/pulls/16109.md) on 2025-10-03 02:57_

---

_Referenced in [astral-sh/uv#16223](../../astral-sh/uv/pulls/16223.md) on 2025-10-10 08:35_

---

_Comment by @andrey-berenda on 2025-10-11 17:35_

One more PR https://github.com/astral-sh/uv/pull/16223. This one is for parsing concurrency env variables

---

_Referenced in [astral-sh/uv#16284](../../astral-sh/uv/pulls/16284.md) on 2025-10-13 15:17_

---

_Comment by @andrey-berenda on 2025-10-13 17:04_

One more PR https://github.com/astral-sh/uv/pull/16284
But some CI checks failed because of timeout or missing API key
can anyone help me with this? 

---

_Comment by @andrey-berenda on 2025-10-18 14:18_

I do not know how to fix this
I just added import `reqwest-retry` and right now `Cargo.toml` is not changed
And right now all tests have passed

Can anyone take a look?

---

_Referenced in [astral-sh/uv#16388](../../astral-sh/uv/pulls/16388.md) on 2025-10-21 14:56_

---

_Referenced in [astral-sh/uv#16342](../../astral-sh/uv/pulls/16342.md) on 2025-10-22 08:40_

---

_Comment by @mjpieters on 2025-10-22 17:18_

I noticed that `UV_RUN_RECURSION_DEPTH` is on the list, but `UV_RUN_MAX_RECURSION_DEPTH` is not. Should the latter also be included in the list?

---

_Comment by @andrey-berenda on 2025-10-22 20:22_

One more PR
https://github.com/astral-sh/uv/pull/16388

---
