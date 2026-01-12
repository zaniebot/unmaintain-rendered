```yaml
number: 8274
title: "WIP/RFC: Wildcards for `banned_api`"
type: pull_request
state: closed
author: akx
labels: []
assignees: []
draft: true
base: main
head: banned-api-wildcard
created_at: 2023-10-27T08:44:35Z
updated_at: 2024-04-08T07:03:09Z
url: https://github.com/astral-sh/ruff/pull/8274
synced_at: 2026-01-12T15:55:25Z
```

# WIP/RFC: Wildcards for `banned_api`

---

_@akx_

## Summary

This PR is a heavy WIP and RFC for adding support for wildcard paths for [`banned-api`](https://docs.astral.sh/ruff/rules/banned-api/).

In short, the goal would be to be able to do

```
[tool.ruff.flake8-tidy-imports.banned-api]
"some_package.ANCIENT*".msg = "None of the ancient powers of `some_package` may be invoked"
```
instead of having to spell out all of the ancients' names in the config.

## How?!

This PR implements this by way of a new `CallPathPattern` struct, which is basically a `CallPath` but with possible wildcard segments (`glob.Pattern`s; I looked at `wildmatch` and decided not to introduce another dependency) and can be matched against a qualified name `CallPath`. I'm not 100% sure that's the best way here, and especially e.g. config parsing becomes a bit hairier since turning the strings in the config to CallPathPatterns could fail (e.g. `some_package.ANCI[` is invalid).  
(I also noticed there's a NameMatchPolicy thing in the `flake8_tidy_imports` linter that kind-of does the same thing.)

Then, there's a possible concern about performance, since we can no longer just use an FxHashMap for looking up bans. (Since this is very much a WIP (and who knows, maybe this is deemed a Bad Idea all in all), I haven't measured things.)

Of course we could come up with a smarter data structure that's both an FxHashMap for the exact-match happy path, and a vector of CallPathPatterns for other situations?

## Test Plan

<!-- How was it tested? -->

Not very well yet. It's probably buggy (but compiles!). The patch is currently also peppered with TODOs.

---

_Comment by @akx on 2023-10-30 10:18_

Sorry to ping, but: any thoughts, @charliermarsh et al.? I wouldn't want to continue here until the whole idea is even slightly validated :)

---

_Comment by @charliermarsh on 2023-10-30 23:52_

I think the overall idea is reasonable. I assume we'd also want to support:

```toml
"some_package.*.ANCIENT".msg = "None of the ancient powers of `some_package` may be invoked"
```

One idea: could we use a `GlobSet`? So, combine all the patterns into one, and do that single test; if it passes, then run all the individual tests to find the relevant message.


---

_Comment by @akx on 2023-10-31 08:41_

@charliermarsh Mm, yeah! (This would already support `some_package.*.ANCIENT`, but not `some_package.**.ANCIENT`.) Using a GlobSet would be a grand idea otherwise, but it doesn't look like you can customize which character(s) the globs think are path separators, so I don't think we can just easily match them against full `CallPath`s then. 

We'd want these patterns to consider `.` path separators, not `/` ... unless we do spooky things on our side and turn the `CallPath`s into slash-separated strings (which, tbh, isn't completely unreasonable, probably...)?

---

_Comment by @akx on 2023-11-01 11:39_

Oh hey, huh... there's some prior art: https://github.com/astral-sh/ruff/pull/5024

---

_Converted to draft by @akx on 2023-11-01 12:44_

---

_Comment by @akx on 2023-11-01 12:44_

Emdraftened pending more discussion in https://github.com/astral-sh/ruff/discussions/8403...

---

_Comment by @zanieb on 2024-03-12 18:33_

@MichaReiser I've assigned you to this to decide what the path forward looks like. Let me know if that's not a good fit.

---

_Assigned to @MichaReiser by @zanieb on 2024-03-12 18:34_

---

_Comment by @MichaReiser on 2024-03-28 17:50_

I only saw the ping when browsing through open Ruff PRs. I'm sorry. 

I don't have a clear path forward for this, but I'm now looking into multifile analysis and plan to significantly change Ruff's semantic model. I think it does make sense to defer this work until we have a better understanding of how our semantic model looks (I don't expect `CallPath` to survive the refactor). 

---

_Closed by @MichaReiser on 2024-04-08 07:03_

---
