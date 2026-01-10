```yaml
number: 4989
title: "Check `python pin` compatibility with `Requires-Python`"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: python-pin-compatibility
created_at: 2024-07-11T09:43:11Z
updated_at: 2024-07-19T19:16:28Z
url: https://github.com/astral-sh/uv/pull/4989
synced_at: 2026-01-10T13:42:52Z
```

# Check `python pin` compatibility with `Requires-Python`

---

_Pull request opened by @blueraft on 2024-07-11 09:43_

## Summary

Resolves #4969 

## Test Plan

`cargo test` and manual tests.


---

_Review requested from @zanieb by @konstin on 2024-07-11 09:44_

---

_@T-256 reviewed on 2024-07-11 13:43_

---

_Review comment by @T-256 on `crates/uv/tests/python_pin.rs`:263 on 2024-07-11 13:43_

what if after successfully pinning, `requires-python` increased? should we show error on `uv python pin`?

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:263 on 2024-07-11 14:24_

A subsequent `uv python pin` (without arguments) could display a warning that the pin isn't compatible, that seems nice.

---

_@zanieb reviewed on 2024-07-11 14:24_

---

_@zanieb reviewed on 2024-07-11 14:25_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:253 on 2024-07-11 14:25_

Maybe we should say "is incompatible with the workspace Python requirement"? Maybe some sort of indication where we got it from? I think there are other messages like this, we should be consistent but I'm not sure what they say.

---

_@zanieb reviewed on 2024-07-11 14:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:68 on 2024-07-11 14:25_

```suggestion
                    "Pinned Python version is incompatible with Requires-Python: {requires_python}"
```

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:60 on 2024-07-11 14:25_

Let's skip this behavior if the the `--isolated` flag is given

---

_@zanieb reviewed on 2024-07-11 14:25_

---

_Label `enhancement` added by @zanieb on 2024-07-11 14:27_

---

_Label `preview` added by @zanieb on 2024-07-11 14:27_

---

_@T-256 reviewed on 2024-07-11 14:28_

---

_Review comment by @T-256 on `crates/uv/tests/python_pin.rs`:263 on 2024-07-11 14:28_

and also, showing warning that pinned python version is not installed

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:263 on 2024-07-11 14:37_

@T-256 if you're interested in taking that last part in a separate pull request that'd be cool

---

_@zanieb reviewed on 2024-07-11 14:37_

---

_@blueraft reviewed on 2024-07-11 14:51_

---

_Review comment by @blueraft on `crates/uv/tests/python_pin.rs`:253 on 2024-07-11 14:51_

I didn't find a similar message elsewhere. But we could output the `pyproject.toml` path here?

"The pinned Python version is incompatible with the workspace's Python requirement ({requirement}) specified at {workspace_root}."

What do you think?


---

_@zanieb reviewed on 2024-07-11 14:57_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:253 on 2024-07-11 14:57_

I think I was thinking of this one

https://github.com/astral-sh/uv/blob/540ff2430267e504887cf34c2e30264f7836ef3f/crates/uv/tests/lock.rs#L1556

There's also this like... workspace / project display name difference at 

https://github.com/astral-sh/uv/blob/540ff2430267e504887cf34c2e30264f7836ef3f/crates/uv/src/commands/project/run.rs#L159-L169

we should probably match that terminology?

Displaying the path seems too verbose if it's present in `-v`, I think we could stick with something like 

> The requested Python version is incompatible with the [project/workspace]'s `Requires-Python` requirement of `...`

Wdyt?

---

_@blueraft reviewed on 2024-07-11 15:22_

---

_Review comment by @blueraft on `crates/uv/tests/python_pin.rs`:253 on 2024-07-11 15:22_

Makes sense, done.

---

_@blueraft reviewed on 2024-07-11 18:33_

---

_Review comment by @blueraft on `crates/uv/tests/python_pin.rs`:263 on 2024-07-11 18:33_

Done :D

---

_@T-256 approved on 2024-07-11 19:31_

---

_Assigned to @zanieb by @zanieb on 2024-07-15 17:58_

---

_@zanieb reviewed on 2024-07-15 18:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:54 on 2024-07-15 18:21_

This will:

1. Perform workspace lookup for each pin the file
2. Throw an error on the first incompatible pin

Instead, we should:

1. Find the `VirtualProject` one time before doing anything
2. Display warnings for incompatibilities on stderr without failing the display since we're just reading pins here

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:45 on 2024-07-15 18:24_

Here we're checking if the _resolved_ request is compatible with the project. This seems nice to have, but we also need to check if the _request_ itself is compatible with the Python requirement. I think what this looks like is matching the `PythonRequest` variants to extract the cases that contain a version and warning if they are incompatible. Then we can do something like:

1. Check if the request is compatible with the project, if not display a warning e.g. "The pinned Python version ..."
2. If that above cannot be checked or is compatible, resolve the request into an interpreter
3. Then, if it is incompatible say "The pinned Python version `...` resolves to `...` which is incompatible with ..."

---

_@zanieb reviewed on 2024-07-15 18:24_

---

_@zanieb reviewed on 2024-07-15 18:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:82 on 2024-07-15 18:25_

Same commentary as https://github.com/astral-sh/uv/pull/4989/files#r1678230289

Unlike when reading the pins, during a write I think we should:

1. Error if the _request_ is incompatible with the Python requirement
2. Warn if the _resolved_ Python is incompatible with the Python requirement (unless `--resolved` is used, then error).

---

_@zanieb reviewed on 2024-07-15 18:26_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:132 on 2024-07-15 18:26_

I think this should be "virtual workspace"

---

_@zanieb reviewed on 2024-07-15 18:26_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:132 on 2024-07-15 18:26_

(or "workspace")

---

_Comment by @zanieb on 2024-07-15 18:27_

I took a closer look and have some more substantive requested changes. Let me know if you have questions or if the scope is too big.

---

_@blueraft reviewed on 2024-07-15 19:30_

---

_Review comment by @blueraft on `crates/uv/src/commands/python/pin.rs`:54 on 2024-07-15 19:30_

Done

---

_@blueraft reviewed on 2024-07-15 19:33_

---

_Review comment by @blueraft on `crates/uv/src/commands/python/pin.rs`:45 on 2024-07-15 19:33_

I didn't really get this part. I think this is the else case when `request` is None, so `uv python pin` without any args.

---

_@zanieb reviewed on 2024-07-15 19:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:45 on 2024-07-15 19:40_

If we have no args, then the "requests" are the lines in the pin file.

---

_@blueraft reviewed on 2024-07-15 20:38_

---

_Review comment by @blueraft on `crates/uv/src/commands/python/pin.rs`:45 on 2024-07-15 20:38_

Ah makes sense. 

---

_@blueraft reviewed on 2024-07-15 21:12_

---

_Review comment by @blueraft on `crates/uv/src/commands/python/pin.rs`:45 on 2024-07-15 21:12_

Done

---

_@blueraft reviewed on 2024-07-15 21:12_

---

_Review comment by @blueraft on `crates/uv/src/commands/python/pin.rs`:82 on 2024-07-15 21:12_

Done

---

_@zanieb reviewed on 2024-07-16 17:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:134 on 2024-07-16 17:07_

We also need to handle `PythonRequest::InterpreterVersion` and `PythonRequest::Key` when there's a version present.

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:135 on 2024-07-16 17:10_

Won't this always fail for `VersionRequest::Range`? Should we make that a part of the `match`? I think if so you can just `unwrap()` instead of matching and logging errors because it'll be "guaranteed" to succeed.

---

_@zanieb reviewed on 2024-07-16 17:10_

---

_@zanieb reviewed on 2024-07-16 17:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:164 on 2024-07-16 17:12_

Might make sense to warn and return here rather than following with a `match` on another `Result`

---

_@zanieb reviewed on 2024-07-16 17:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:215 on 2024-07-16 17:13_

Can we note that this is the resolved interpreter for the pinned version?

---

_@zanieb reviewed on 2024-07-16 17:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:179 on 2024-07-16 17:14_

We should use `PythonRequest::to_canonical_string` for display here and wrap in backticks.

---

_@zanieb reviewed on 2024-07-16 17:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:150 on 2024-07-16 17:14_

Can you use `err` instead of `e` for consistency?

---

_@zanieb reviewed on 2024-07-16 17:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:150 on 2024-07-16 17:15_

(this applies throughout)

---

_@blueraft reviewed on 2024-07-16 18:52_

---

_Review comment by @blueraft on `crates/uv/src/commands/python/pin.rs`:215 on 2024-07-16 18:52_

We also call this function to check if the `request` python matches the `requires-python`, so it wouldn't just be the resolved interpreter right?

---

_@blueraft reviewed on 2024-07-16 18:59_

---

_Review comment by @blueraft on `crates/uv/src/commands/python/pin.rs`:164 on 2024-07-16 18:59_

I've changed this up to remove the extra match, let me know if that's ok

---

_@zanieb reviewed on 2024-07-16 19:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:215 on 2024-07-16 19:05_

Ah then we'll need some sort of state (i.e. a bool) to track if this is a request or the resolved version. It's also awkward that this message does not include the request at all, so we're just making a leap from a request to a concrete Python version without indicating how or why.

---

_@blueraft reviewed on 2024-07-16 19:20_

---

_Review comment by @blueraft on `crates/uv/src/commands/python/pin.rs`:215 on 2024-07-16 19:20_

I've used an Enum now to differentiate this

---

_Comment by @zanieb on 2024-07-19 17:30_

Ah terrifying you force pushed while I was editing locally :) 

---

_Comment by @zanieb on 2024-07-19 17:30_

I'll make sure this is merged today

---

_Comment by @blueraft on 2024-07-19 17:32_

> Ah terrifying you force pushed while I was editing locally :)

AHH! Sorry, was trying the pull in main and fix the merge conflicts. You can take over the branch :) 

---

_Comment by @zanieb on 2024-07-19 17:33_

No problem haha it's your pull request! I'll take care of the remaining conflicts though :) thanks

---

_Comment by @zanieb on 2024-07-19 18:09_

Thanks for being patient with this one! I made some important fixes as well as some simplifications the code and tweaks to messages — we could probably make factor the code into something further easier to read but it seems decent for now.

---

_@T-256 approved on 2024-07-19 18:17_

Awsome!

---

_Merged by @zanieb on 2024-07-19 18:17_

---

_Closed by @zanieb on 2024-07-19 18:17_

---

_Comment by @blueraft on 2024-07-19 18:23_

Thanks for the thorough review! Appreciate it

---

_Branch deleted on 2024-07-19 18:23_

---

_@T-256 reviewed on 2024-07-19 18:28_

---

_Review comment by @T-256 on `crates/uv/tests/python_pin.rs`:335 on 2024-07-19 18:28_

Am I misunderstood or `cpython` must be resolved to 3.11? since context already contains it.

---

_@T-256 reviewed on 2024-07-19 18:39_

---

_Review comment by @T-256 on `crates/uv/tests/python_pin.rs`:335 on 2024-07-19 18:39_

ref https://github.com/astral-sh/uv/pull/5205

---

_@zanieb reviewed on 2024-07-19 18:56_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:335 on 2024-07-19 18:56_

Yes that's problematic. cc @charliermarsh looks like this regressed.

---

_@zanieb reviewed on 2024-07-19 19:16_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:335 on 2024-07-19 19:16_

This actually isn't a regression or related to #5205, we're just taking the first Python on the path. The problem here is that we need to respect `Requires-Python` on top of pinned version requests (😭).

---
