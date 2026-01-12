```yaml
number: 15036
title: "Enable extra build dependencies to 'match runtime' versions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - no-build
assignees: []
merged: true
base: main
head: charlie/match-runtime
created_at: 2025-08-03T00:37:44Z
updated_at: 2025-08-05T18:00:46Z
url: https://github.com/astral-sh/uv/pull/15036
synced_at: 2026-01-12T16:11:32Z
```

# Enable extra build dependencies to 'match runtime' versions

---

_@charliermarsh_

## Summary

This is an alternative to https://github.com/astral-sh/uv/pull/14944 that functions a little differently. Rather than adding separate strategies, you can instead say:

```toml
[tool.uv.extra-build-dependencies]
child = [{ requirement = "anyio", match-runtime = true }]
```

Which will then enforce that `anyio` uses the same version as in the lockfile.


---

_Label `no-build` added by @charliermarsh on 2025-08-03 00:37_

---

_Label `no-test` added by @charliermarsh on 2025-08-03 00:37_

---

_Review requested from @zanieb by @charliermarsh on 2025-08-03 00:44_

---

_@charliermarsh reviewed on 2025-08-03 00:51_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:742 on 2025-08-03 00:51_

These are blockers but conceptually doable.

---

_@charliermarsh reviewed on 2025-08-03 02:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:742 on 2025-08-03 02:17_

https://github.com/astral-sh/uv/pull/15038 should help.

---

_Comment by @charliermarsh on 2025-08-04 23:40_

(Need to rebase this, then fix the TODO I commented-on above.)

---

_Marked ready for review by @charliermarsh on 2025-08-05 10:15_

---

_Label `no-test` removed by @charliermarsh on 2025-08-05 10:15_

---

_Label `enhancement` added by @charliermarsh on 2025-08-05 10:15_

---

_Comment by @charliermarsh on 2025-08-05 10:18_

I addressed the remaining TODOs, should be ready for review.

---

_Review requested from @geofft by @charliermarsh on 2025-08-05 12:55_

---

_Review requested from @konstin by @charliermarsh on 2025-08-05 12:55_

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:12596 on 2025-08-05 14:37_

We should call this `requires` instead of `requirement` to by consistent with PEP 517 (It's still a str here and a list in PEP 517, but it's the same key) 

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:12631 on 2025-08-05 14:42_

Can you add a test where we change the requirement in a compatible way (say `anyio>3.1` to `anyio>3.2`) to track whether we rebuild?

---

_Review comment by @konstin on `crates/uv-distribution-types/src/resolved.rs`:107 on 2025-08-05 14:47_

This should be deduplicated, with `impl From<&ResolvedDist> for RequirementSource`. (There's currently a difference with `location.set_fragment(None)`)

---

_Comment by @konstin on 2025-08-05 14:59_

Is it intentional that extra build dependencies work in `uv pip` too, but `match-runtime = true` is limited to project mode, while being ignore in `uv pip` mode? (I can make an MRE if helpful)

---

_Comment by @charliermarsh on 2025-08-05 15:01_

We could make it work in `uv pip`, probably.

---

_Comment by @zanieb on 2025-08-05 15:06_

It seems fine to defer that to start.

---

_Comment by @charliermarsh on 2025-08-05 15:07_

It could help inform the name though... If we intend for that to work, then it's another point against things like `locked` or `match-lockfile` etc.

---

_Comment by @zanieb on 2025-08-05 15:16_

This name has sort of grown on me, fwiw. The entire field is in preview too, so we have some leeway here.

---

_Comment by @konstin on 2025-08-05 15:20_

I'm always in favor of smaller PRs and adding this another PR :) I do see the current behavior (`match-runtime` is ignored in `uv pip` without warning) as a blocker to stabilization, we should interpret the extra build deps table the same between project mode and `uv pip` for extra build dependencies.

---

_Review comment by @konstin on `crates/uv-distribution-types/src/build_requires.rs`:52 on 2025-08-05 15:23_

What about only having `AnnotatedBuildRequirement` with `match_runtime: Option<bool>`, to having to match on the two cases? This would allow us to `Either` too. The only place where we'd still need the enum is in the deserialize impl. 

---

_@konstin reviewed on 2025-08-05 15:25_

---

_@zanieb reviewed on 2025-08-05 16:11_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:12596 on 2025-08-05 16:11_

I don't think I agree — I don't think that's particularly clearer and matching `build-requires` isn't really a goal here, i.e., it's not called `extra-build-requires`.

---

_@charliermarsh reviewed on 2025-08-05 16:42_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/build_requires.rs`:52 on 2025-08-05 16:42_

That sounds reasonable.

---

_@charliermarsh reviewed on 2025-08-05 16:43_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/resolved.rs`:107 on 2025-08-05 16:43_

Oh perfect, thank you. I was looking for that...

---

_@zanieb reviewed on 2025-08-05 16:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:821 on 2025-08-05 16:48_

It feels a little weird that this isn't a method on `ExtraBuildRequires` are you avoiding some circular dependency?

---

_@zanieb reviewed on 2025-08-05 16:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:821 on 2025-08-05 16:49_

Should it be must use?

---

_@zanieb reviewed on 2025-08-05 16:52_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:852 on 2025-08-05 16:52_

I'm a little confused here. Why do we toggle it to `false`?

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:829 on 2025-08-05 16:53_

What happens if the requirement has a pin or constraint that's incompatible with the runtime dist version? We should error, right?

---

_@zanieb reviewed on 2025-08-05 16:53_

---

_@charliermarsh reviewed on 2025-08-05 16:54_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:852 on 2025-08-05 16:54_

That's an oversight, sorry. I think this value doesn't matter from here on out. We could probably use a different type post-transform but seems like a lot of work.

---

_@charliermarsh reviewed on 2025-08-05 16:55_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:829 on 2025-08-05 16:55_

I was considering erroring entirely if a version specifier or constraint is provided with `match-runtime = true`. What do you think?

---

_@charliermarsh reviewed on 2025-08-05 16:56_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:821 on 2025-08-05 16:56_

I'm just matching the style of the rest of the file, but I suspect it could be a method on `ExtraBuildRequires` yeah.

---

_@charliermarsh reviewed on 2025-08-05 17:00_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:829 on 2025-08-05 17:00_

It would be relatively easy to support preserving _both_ though. We'd just add a new requirement here rather than overwriting, and let the resolver do the rest.

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:829 on 2025-08-05 17:07_

It seems okay to eagerly fail if any constraint is provided. It's plausible someone will have a use-case for it? but I can't think of one right now.

---

_@zanieb reviewed on 2025-08-05 17:07_

---

_@zanieb reviewed on 2025-08-05 17:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:829 on 2025-08-05 17:08_

I think letting the resolver handle it sounds cool, but we'd probably need to add a hint to the error for it to be a good experience and that sounds like more work we don't want to tackle rn.

---

_@zanieb reviewed on 2025-08-05 17:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:852 on 2025-08-05 17:09_

Okay cool, that makes sense — I thought maybe we were trying to toggle it to `false` to indicate it. I think a different type seems overkill. I'd probably retain `true` though in case we need to do some debugging or hinting based on that information later.

---

_@zanieb approved on 2025-08-05 17:09_

---

_@charliermarsh reviewed on 2025-08-05 17:39_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:829 on 2025-08-05 17:39_

Okay, made it reject for now.

---

_Merged by @charliermarsh on 2025-08-05 18:00_

---

_Closed by @charliermarsh on 2025-08-05 18:00_

---

_Branch deleted on 2025-08-05 18:00_

---
