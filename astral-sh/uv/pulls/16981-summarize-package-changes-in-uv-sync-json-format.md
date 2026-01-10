```yaml
number: 16981
title: "Summarize package changes in `uv sync` json format output"
type: pull_request
state: merged
author: EliteTK
labels:
  - preview
assignees: []
merged: true
base: main
head: tk/sync-summary
created_at: 2025-12-04T15:39:33Z
updated_at: 2025-12-18T19:37:04Z
url: https://github.com/astral-sh/uv/pull/16981
synced_at: 2026-01-10T05:49:14Z
```

# Summarize package changes in `uv sync` json format output

---

_Pull request opened by @EliteTK on 2025-12-04 15:39_

## Summary

Implement #16653 by making `uv sync --output-format=json` output information about package changes.

## Test Plan

Additional tests to test the cases where there is no known package version _may_ be beneficial but as the information used is the same as the information used by the dry run logging now, I don't think that's strictly necessary as those cases are tested.

---

_Label `preview` added by @EliteTK on 2025-12-04 15:39_

---

_Converted to draft by @EliteTK on 2025-12-04 15:39_

---

_@EliteTK reviewed on 2025-12-04 15:41_

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/loggers.rs`:164 on 2025-12-04 15:41_

This now allocates unfortunately. Let me know if there's a straightforward way to avoid this (I am aware of `either` being probably usable here, but I didn't want to add dependencies - although it is used elsewhere).

---

_@EliteTK reviewed on 2025-12-04 15:42_

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/loggers.rs`:157 on 2025-12-04 15:42_

Example of where installed_version is used for comparison when canonical_version is strictly more appropriate.

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/operations.rs`:443 on 2025-12-04 15:45_

This is awful, it's a direct side effect of `ChangeEvent` requiring this type implements `InstalledMetadata`. I think that potentially, since `ChangeEvent` is only used (in this way - actually not sure anymore) in the logger and in uv-sync, it may make sense to use a different type and then this whole thing can be avoided.

---

_@EliteTK reviewed on 2025-12-04 15:45_

---

_@EliteTK reviewed on 2025-12-04 15:47_

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/operations.rs`:506 on 2025-12-04 15:47_

Annoying duplication. I may investigate de-duplicating this.

---

_Review comment by @EliteTK on `crates/uv/src/commands/project/sync.rs`:1267 on 2025-12-04 15:49_

https://github.com/astral-sh/uv/pull/16660#discussion_r2510856345

---

_@EliteTK reviewed on 2025-12-04 15:49_

---

_Review requested from @zanieb by @EliteTK on 2025-12-04 15:50_

---

_@konstin reviewed on 2025-12-04 15:58_

---

_Review comment by @konstin on `crates/uv/src/commands/pip/loggers.rs`:164 on 2025-12-04 15:58_

I'm not worried about this allocating, it's not in a hot loop, it's cheaper than a lot of the hidden allocations we do and the number of versions we print is very limited (and printing is usually very slow in comparison)

---

_Comment by @EliteTK on 2025-12-04 18:36_

So, to summarise a discussion from Discord, a suggestion was made to pull out the planning step from `pip::operations::install` into a separate `plan` function. Or, alternatively, just return the plan from `install`.

The goal being that a `PackageChangeReport::from_plan` can be added and used in the dry-run case, avoiding going through `Changelog` entirely, saving a bunch of code.

I've started to explore the idea of pulling out `plan`, but I don't think it's worth it. It adds a lot of boilerplate. I'll submit it as a separate PR and then I'll consider the alternative. Then whatever the result of that, this PR can be rebased on top and simplified.

---

_Comment by @EliteTK on 2025-12-05 15:01_

So, the idea behind `--dry-run` is to produce output which is similar to what is produced without it, but without actually performing any actions. So, for example, the outputs below are almost identical:

~~~bash session
$ uv sync
Resolved 6 packages in 2ms
Uninstalled 4 packages in 5ms
Installed 3 packages in 8ms
 - anyio==4.12.0
 + charset-normalizer==3.4.4
 - h11==0.16.0
 - httpcore==1.0.9
 - httpx==0.28.1
 + requests==2.32.5
 + urllib3==2.5.0
$ magically_revert_state # Trade secret
$ uv sync --dry-run
Would use project environment at: .venv
Resolved 6 packages in 1ms
Found up-to-date lockfile at: uv.lock
Would uninstall 4 packages
Would install 3 packages
 - anyio==4.12.0
 + charset-normalizer==3.4.4
 - h11==0.16.0
 - httpcore==1.0.9
 - httpx==0.28.1
 + requests==2.32.5
 + urllib3==2.5.0
~~~

And the goal of the json output is to present this information in JSON.

In the output above, the normal path uses `InstallLogger::on_complete`, but `report_dry_run` re-implements the code from `DefaultInstallLogger::on_complete` and kind of effectively produces the same information which would have been in `Changelog`. This[^1] `Changelog` is only ever used to implement `InstallLogger::on_complete` logging, I think it actually would make sense for `Changelog` be the type which uniformly stores the information (regardless of whether it was from an install, or a dry-run install).

I think the only real changes which would be required is to: Expand the kinds of dists that `Changelog` can contain, so it can contain remote dists, and then adjust the respective `ChangeEvent` type (which similarly is only used within `on_complete`) to be able to deal with things without versions (possibly just fall back to a URL like the current `report_dry_run` code does). This would have the knock-on effect of allowing `report_dry_run` to just use the `on_complete` logger, cutting it down further.

Finally, `report_dry_run` can just return this `Changelog` type.

I believe this can all be done while keeping things pretty clean. Definitely no `ZERO_VERSION` hack ;).

Let me know what you think.

[^1]: There are two `Changelog` types and two `ChangeEvent` types because of the slightly different requirements of where they are used. Both the logging functionality and the different changelogs could be unified, I think. But I don't think that it would necessarily benefit from being done now. I can kind of see it though, would be good to investigate one day, as there are two places where the `DefaultInstallLogger::on_complete` functionality is re-implemented verbatim. In fact, the changes suggested here would probably make such a change easier to implement.

---

_Comment by @zanieb on 2025-12-05 15:03_

Sounds fine to me!

---

_@EliteTK reviewed on 2025-12-12 12:16_

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/loggers.rs`:157 on 2025-12-12 12:16_

Having thought about it further, maybe it doesn't matter much for this usecase.

---

_Comment by @EliteTK on 2025-12-12 12:17_

Okay, rebased on top of the new changelog stuff, fixed the package naming and attaching the wrong lifetime to a reference in `VersionOrUrlRef`. This is ready I think?

---

_Marked ready for review by @EliteTK on 2025-12-12 12:18_

---

_@zanieb reviewed on 2025-12-15 18:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1370 on 2025-12-15 18:03_

I think I'd expect "Uninstalled" and "Installed" here if we're using "Reinstalled"?

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1378 on 2025-12-15 18:04_

I'm a little confused by the name of this struct. It also appears to be missing from the test coverage?

---

_@zanieb reviewed on 2025-12-15 18:04_

---

_@zanieb reviewed on 2025-12-15 18:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1325 on 2025-12-15 18:06_

It seems weird for `PackageChangeReport::from_changelog` to return `Vec<Self>`? I guess I'd expect a `PackageChangesReport(Vec<PackageChangeReport>)` new-type?

---

_@zanieb reviewed on 2025-12-15 18:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1339 on 2025-12-15 18:06_

Have you thought more about what other information we'll want to include here? I don't think we need to expand on it in this pull request but we should at least open an issue for discussion.

---

_@zanieb reviewed on 2025-12-15 18:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1378 on 2025-12-15 18:07_

I think the name makes sense in the context of the parent, but I'm still a bit confused on the scope here. Isn't the source an enum? like "registry", "path", "url", etc.?

---

_@EliteTK reviewed on 2025-12-15 18:14_

---

_Review comment by @EliteTK on `crates/uv/src/commands/project/sync.rs`:1325 on 2025-12-15 18:14_

Yes I thought this was weird too. I will adjust it.

---

_@EliteTK reviewed on 2025-12-15 18:22_

---

_Review comment by @EliteTK on `crates/uv/src/commands/project/sync.rs`:1378 on 2025-12-15 18:22_

Yes I agree that this would make more sense as the definition of source.

But I don't think we should necessarily try to put that in yet, although it's not the worst idea for something to put in here. I think in this case the nesting is unnecessary and I would probably rather just see an `Option<DisplaySafeUrl>` one level up for now. Would that work?

---

_@EliteTK reviewed on 2025-12-15 18:23_

---

_Review comment by @EliteTK on `crates/uv/src/commands/project/sync.rs`:1339 on 2025-12-15 18:23_

Well part of changing the changelog to just store the dist was also in preparation for this. We can more or less put anything in here we want that can be found in a Dist/LocalDist at this point. But I will make an issue tomorrow.

---

_@EliteTK reviewed on 2025-12-15 18:28_

---

_Review comment by @EliteTK on `crates/uv/src/commands/project/sync.rs`:1370 on 2025-12-15 18:28_

I imagine it's because of the `ChangeEventKind` wording, but that doesn't justify it, `DefaultInstallLogger` uses `Installed`, and `Uninstalled`. `UpgradeInstallLogger` uses `Added` and `Removed`. Maybe this sort of thing should be in a style guide?

---

_@zanieb reviewed on 2025-12-15 18:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1378 on 2025-12-15 18:53_

That could be okay or I'd just omit it entirely until we handle sources more comprehensively?

---

_@zanieb approved on 2025-12-15 18:53_

---

_@EliteTK reviewed on 2025-12-15 19:58_

---

_Review comment by @EliteTK on `crates/uv/src/commands/project/sync.rs`:1378 on 2025-12-15 19:58_

Hmm, but we do print it in the stderr change log in those cases. But I am somewhat indifferent. 

---

_@EliteTK reviewed on 2025-12-18 13:28_

---

_Review comment by @EliteTK on `crates/uv/src/commands/project/sync.rs`:1339 on 2025-12-18 13:28_

Maybe the package details should be in their own specific sub-structure so that the top level is just "what happened" and "to what" instead of "what happened" and a bunch of metadata about what it happened to. I'll make this change.

---

_@zanieb reviewed on 2025-12-18 13:30_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1339 on 2025-12-18 13:30_

Can you sketch what you're thinking of first? I don't want you to waste your time if we don't have consensus on the structure.

---

_Review comment by @EliteTK on `crates/uv/src/commands/project/sync.rs`:1339 on 2025-12-18 13:56_

I'll propose something in the issue I made. For now I've left it alone.

---

_@EliteTK reviewed on 2025-12-18 13:56_

---

_Merged by @EliteTK on 2025-12-18 19:37_

---

_Closed by @EliteTK on 2025-12-18 19:37_

---

_Branch deleted on 2025-12-18 19:37_

---
