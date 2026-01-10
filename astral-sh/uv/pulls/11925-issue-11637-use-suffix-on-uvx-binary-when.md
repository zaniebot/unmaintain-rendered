```yaml
number: 11925
title: "[Issue #11637] Use suffix on uvx binary when searching for uv binary"
type: pull_request
state: closed
author: cliebBS
labels: []
assignees: []
base: main
head: feature/111637/pipx-suffix-uvx
created_at: 2025-03-03T15:23:38Z
updated_at: 2025-04-16T18:53:24Z
url: https://github.com/astral-sh/uv/pull/11925
synced_at: 2026-01-10T11:10:39Z
```

# [Issue #11637] Use suffix on uvx binary when searching for uv binary

---

_Pull request opened by @cliebBS on 2025-03-03 15:23_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When running uvx that has been installed using a pipx suffix, the corresponding uv binary cannot be found due to uvx always looking for an executable called `uv` on the PATH, while it has instead been installed using a suffix.  This change makes it so that uvx discovers the suffix to its name and then searches for a uv binary with the same suffix.  For example, if uvx is called `uvx@0.6.2`, then it will search for `uv@0.6.2` instead of `uv`.

## Test Plan

1. Remove all `uv` and `uvx` binaries from the PATH
2. Build code on `main`
3. Rename `uvx` -> `uvx@0.6.2` and `uv` -> `uv@0.6.2`
4. Run `uvx@0.6.2` and observe that it produces the following output:
```
error: Could not find the `uv` binary at: /Users/me/.local/bin/uv
```
5. Delete the `uv@0.6.2` and `uvx@0.6.2` binaries
6. Build the code on this branch
7. Run `uvx` and observe that it behaves like normal (maintains current behavior)
8. Rename `uvx` -> `uvx@0.6.2` and `uv` -> `uv@0.6.2`
9. Run `uvx@0.6.2` and observe that it behaves like normal (exhibits correct behavior for suffixed executable names)
10. Rename `uv@0.6.2` -> `uv`
11. Run `uvx@0.6.2` and observe that it emits a warning about falling back to bare `uv`, followed by normal `uvx` output
```
warning: Unable to find /Users/jane.doe/code/uv/target/debug/uv@0.6.2, defaulting to /Users/jane.doe/code/uv/target/debug/uv
Provide a command to run with `uvx <command>`.

The following tools are installed:

- keyring v25.5.0

See `uvx --help` for more information.
```
12. Run `uvx@0.6.2` and observe that it presents an error message stating that `uv` could not be found either with the suffixed name or with the bare name
```
error: Could not find uv binary at /Users/jane.doe/uv/target/debug/uv@0.6.2 or /Users/jane.doe/uv/target/debug/uv
```

Resolves #11637 

---

_Renamed from "[Issue 111637] Use suffix on uvx binary when searching for uv binary" to "[Issue 11637] Use suffix on uvx binary when searching for uv binary" by @cliebBS on 2025-03-03 15:23_

---

_Renamed from "[Issue 11637] Use suffix on uvx binary when searching for uv binary" to "[Issue #11637] Use suffix on uvx binary when searching for uv binary" by @cliebBS on 2025-03-03 15:24_

---

_@zanieb reviewed on 2025-03-03 17:46_

---

_Review comment by @zanieb on `crates/uv/src/bin/uvx.rs`:48 on 2025-03-03 17:46_

Should all these error cases have different error messages? Seems helpful to distinguish between them if someone actually encounters these failures.

Does it make sense to use a `io:Error` here? `NotFound` seems weird in this context, though I'm not sure if it's worth complicating the `run` signature.

---

_@cliebBS reviewed on 2025-03-03 18:25_

---

_Review comment by @cliebBS on `crates/uv/src/bin/uvx.rs`:48 on 2025-03-03 18:25_

I wasn't sure how much of the details we wanted to expose to users.  I updated the code to have distinct error messages for each potential error situation, switched a couple of places to use `InvalidData` as the error kind instead of `NotFound`, and removed one of the error cases entirely in favor of using a fallback value.

---

_Comment by @zanieb on 2025-03-03 18:54_

I think this makes sense to me. Thanks for taking the time to contribute it.

Do you think we should fallback to `uv` if `uv-suffix` does not exist? I think we might need to for backwards compatibility?

---

_Assigned to @zanieb by @zanieb on 2025-03-03 18:54_

---

_Comment by @cliebBS on 2025-03-03 19:16_

Falling back to plain `uv` when `uv{suffix}` doesn't exist I think has a couple of problems:

1. There's no guarantee that there will be a `uv` on the system.  Doing a suffixed install using `pipx` will rename all binaries to use the suffix, so you'd actually need a separate installation of uv on your system to have the fallback option in place
2. Falling back to bare `uv` can result in a different version of uv being used to execute the command than the version of `uvx` that is trying to exec the command.  While this would work for now, if a breaking change were made to `uv tool uvx` or `uv tool run` in the future (this is still a 0.x product, after all), it could result in `uvx{suffix}` becoming incompatible with `uv` and likely producing some very confusing errors for the user

---

_Comment by @zanieb on 2025-03-03 19:42_

This assumes that `pipx` is the only reason that the binary would have a different name though, which I'm not comfortable with.

---

_Comment by @cliebBS on 2025-03-03 19:57_

Ok, it now does the following:

1. Run `uv{suffix}` if it is present in `uvx` directory
2. Run `uv` if it is present in `uvx` directory
3. Provide an error message that includes the names we tried to use to find `uv`

---

_Comment by @cliebBS on 2025-03-12 12:32_

@zanieb Is there anything more I can do at this point to help get this fix/change merged?

---

_Comment by @zanieb on 2025-04-16 15:27_

I don't think so, sorry — just about finding time to review.

@Gankra can you own this?

---

_Review requested from @Gankra by @Gankra on 2025-04-16 15:28_

---

_Assigned to @Gankra by @Gankra on 2025-04-16 15:29_

---

_@Gankra requested changes on 2025-04-16 17:10_

I like the idea, but if we're willing to fallback to `uv` then it doesn't make sense to consider it an error for the `uvx` binary to have a weird/unparseable name. At most we could emit a warning?

---

_Comment by @Gankra on 2025-04-16 18:25_

Continued in:
* #12923

---

_Comment by @cliebBS on 2025-04-16 18:35_

@Gankra I updated the code to emit a warning when falling back, and also to fall back when there is an issue in finding the extension for the `uvx` binary.  It will now only fail if no `uv` is found at all.

---

_Comment by @Gankra on 2025-04-16 18:53_

Ah, sorry for the race, I've merged my impl! I should have mentioned I was going to push up a cleanup (incredible response time on a PR this old, wow!).

Thanks for the great idea/contribution!

---

_Closed by @Gankra on 2025-04-16 18:53_

---
