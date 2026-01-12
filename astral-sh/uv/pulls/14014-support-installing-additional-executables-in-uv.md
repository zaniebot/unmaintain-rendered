```yaml
number: 14014
title: "Support installing additional executables in `uv tool install`"
type: pull_request
state: merged
author: aaron-ang
labels: []
assignees: []
merged: true
base: main
head: tool-install
created_at: 2025-06-13T05:23:08Z
updated_at: 2025-08-06T10:05:36Z
url: https://github.com/astral-sh/uv/pull/14014
synced_at: 2026-01-12T16:10:58Z
```

# Support installing additional executables in `uv tool install`

---

_@aaron-ang_

Close #6314

## Summary

Continuing from #7592. Created a new PR to rebase the old branch with `main`, cleaned up test errors, and improved readability.

## Test Plan

Same test cases as in #7592.


---

_Assigned to @zanieb by @konstin on 2025-06-13 10:24_

---

_Comment by @zanieb on 2025-06-13 11:52_

Cool, thanks! I'll take a look.

---

_@zanieb reviewed on 2025-06-13 13:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:281 on 2025-06-13 13:07_

Nit: We use `s` as the name pretty consistently in this repository, it'd be nice to retain the style there.

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:300 on 2025-06-13 13:12_

It looks like we add all the entries to `new_entrypoints` so it's identical to `installed_entrypoints`? Should we just return `installed_entrypoints`? I'm confused about what this is for.

---

_@zanieb reviewed on 2025-06-13 13:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:300 on 2025-06-13 13:17_

(Nevermind I had the wrong commit checked out locally where `installed_entrypoints` was not passed in as a mutable ref, let me look at this again...)

---

_@zanieb reviewed on 2025-06-13 13:17_

---

_@zanieb reviewed on 2025-06-13 13:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:300 on 2025-06-13 13:19_

I think I'm still confused about `new_entrypoints`, it's not used? Why do we take `installed_entrypoints` as a mutable ref instead of just returning the entrypoints that we've installed?

---

_@zanieb reviewed on 2025-06-13 13:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:314 on 2025-06-13 13:20_

I don't think we should return `ExitStatus` from utilities — if this has no return value we should use `Ok(())`

---

_@zanieb reviewed on 2025-06-13 13:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:627 on 2025-06-13 13:27_

It's sad we need to clone all these. These are just needed for the receipt, right? but we don't create a receipt for each extra executable, those are included in the top-level tool receipt?

---

_Review comment by @aaron-ang on `crates/uv/src/commands/tool/common.rs`:300 on 2025-06-13 18:29_

my bad. I didn't remove `new_entrypoints` from the refactoring in the previous commit.

---

_@aaron-ang reviewed on 2025-06-13 18:29_

---

_Review comment by @aaron-ang on `crates/uv/src/commands/tool/install.rs`:627 on 2025-06-13 18:38_

Perhaps we could move the logic of accumulating `install_entrypoints` into `install_executables`, which will also reduce cloning.

---

_@aaron-ang reviewed on 2025-06-13 18:38_

---

_Review requested from @zanieb by @aaron-ang on 2025-06-13 23:05_

---

_Comment by @aaron-ang on 2025-06-19 23:41_

rebased with main

---

_@aaron-ang reviewed on 2025-06-27 21:04_

---

_Review comment by @aaron-ang on `crates/uv/src/commands/tool/common.rs`:207 on 2025-06-27 21:04_

We deduplicate and sort the entrypoints, then iterate over them and the root package to install.

---

_Review comment by @aaron-ang on `crates/uv/src/commands/tool/common.rs`:342 on 2025-06-27 21:09_

And add a single tool receipt at the end containing all installed entrypoints.

---

_@aaron-ang reviewed on 2025-06-27 21:09_

---

_Comment by @zanieb on 2025-07-17 12:23_

Thanks for your patience here! I think this is approaching being ready to merge.

---

_@zanieb reviewed on 2025-07-17 12:24_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4424 on 2025-07-17 12:24_

Sorry if we've discussed this, but should this be `comma::CommaSeparatedRequirements`? It seems nice to use comma separated values since the flag name is long, and it seems fine to allow requirements rather than package names.

---

_@zanieb reviewed on 2025-07-17 12:25_

---

_Review comment by @zanieb on `crates/uv-tool/src/tool.rs`:174 on 2025-07-17 12:25_

Could we avoid taking a `mut`? Is there a strong motivation to do so?

---

_@zanieb reviewed on 2025-07-17 12:26_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:254 on 2025-07-17 12:26_

Does this mean we'll fail if _any_ of the `--with-executables-from` packages doesn't have entry points? It should probably just be a warning hinting to use `--with` instead if it's not the root package.

---

_@zanieb reviewed on 2025-07-17 12:26_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:254 on 2025-07-17 12:26_

Could you add a test case for this scenario?

---

_@aaron-ang reviewed on 2025-07-17 14:49_

---

_Review comment by @aaron-ang on `crates/uv-tool/src/tool.rs`:174 on 2025-07-17 14:49_

I've reverted this change to remove the `mut`

---

_@aaron-ang reviewed on 2025-07-17 14:50_

---

_Review comment by @aaron-ang on `crates/uv-cli/src/lib.rs`:4424 on 2025-07-17 14:50_

by requirements do you mean `requirements.txt`? My understanding is that `requirements.txt` is only used for `pip install`.

---

_Renamed from "Support installing additional executables in`uv tool install`" to "Support installing additional executables in `uv tool install`" by @aaron-ang on 2025-07-17 14:57_

---

_@zanieb reviewed on 2025-07-17 15:05_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4424 on 2025-07-17 15:05_

No, I mean like a `RequirementsTxtRequirement` (which is a single requirement) rather than a `PackageName`.

e.g., as handled at

https://github.com/astral-sh/uv/blob/b7db67edc166de7aa7d689b112cd4cb8a4809efc/crates/uv/src/lib.rs#L1239-L1244

maybe using `RequirementsSource::from_package`?

---

_@zanieb reviewed on 2025-07-17 15:20_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4424 on 2025-07-17 15:20_

No `RequirementsTxtRequirement` is similar to `PackageName` but allows more things. It's not a `requirements.txt` file.

The behavior would be the same, e.g., `--with-executables-from foo` would work, but you'd also be able to do multiple values, e.g., `--with-executables-from foo,bar` 

---

_@zanieb reviewed on 2025-07-30 15:56_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1234 on 2025-07-30 15:56_

It looks like you dropped with `with_capacity` optimization?

---

_@zanieb reviewed on 2025-07-30 15:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:626 on 2025-07-30 15:57_

I wonder if we should resolve these to `PackageName` over in `uv/src/lib.rs` instead so you don't need to do this here? Over there, we know they're package names.

---

_@zanieb reviewed on 2025-07-30 15:58_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1233 on 2025-07-30 15:58_

I think we probably need some sort of comment here that makes it clear that `with_executables_from` are being added to `with`. It's easy to miss otherwise.

---

_@zanieb reviewed on 2025-07-30 15:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:626 on 2025-07-30 15:59_

This would help with https://github.com/astral-sh/uv/pull/14014/files?diff=unified&w=1#r2243165952 too, since the type would be different.

---

_@zanieb reviewed on 2025-07-30 16:03_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:3885 on 2025-07-30 16:03_

Is this resolved now? Can we drop the cfg?

---

_Comment by @aaron-ang on 2025-07-30 17:13_

@zanieb please take a look at my latest commit: [`0023e5d` (#14014)](https://github.com/astral-sh/uv/pull/14014/commits/0023e5de36f0dabbdac96a4086c430331cca9ca9)

---

_@zanieb approved on 2025-07-30 19:50_

---

_Merged by @zanieb on 2025-07-30 19:50_

---

_Closed by @zanieb on 2025-07-30 19:50_

---
