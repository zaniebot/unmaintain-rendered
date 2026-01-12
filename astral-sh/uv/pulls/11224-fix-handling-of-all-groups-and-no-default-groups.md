```yaml
number: 11224
title: "fix handling of `--all-groups` and `--no-default-groups` flags"
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/group-cleanup
created_at: 2025-02-04T19:28:17Z
updated_at: 2025-02-05T20:31:25Z
url: https://github.com/astral-sh/uv/pull/11224
synced_at: 2026-01-12T16:09:44Z
```

# fix handling of `--all-groups` and `--no-default-groups` flags

---

_@Gankra_

This is a rewrite of the groups subsystem to have more clear semantics, and some adjustments to the CLI flag constraints. In doing so, the following bugs are fixed:

* `--no-default-groups --no-group foo` is no longer needlessly rejected
* `--all-groups --no-default-groups` now correctly evaluates to `--all-groups` where previously it was erroneously being interpretted as just `--no-default-groups`
* `--all-groups --only-dev` is now illegal, where previously it was accepted and mishandled, as if it was a mythical `--only-all-groups` flag

Fixes #10890
Closes #10891 

---

_Label `internal` added by @Gankra on 2025-02-04 19:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2735 on 2025-02-04 20:50_

`--only-dev` and `--dev` do not conflict, `--no-dev`?
```suggestion
    #[arg(long, conflicts_with_all = ["group", "no_dev", "all_groups"])]
    pub only_dev: bool,
```

---

_@zanieb reviewed on 2025-02-04 20:50_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2954 on 2025-02-04 20:50_

Similarly, `--dev` and `--only-dev` do not conflict 
```suggestion
    #[arg(long, conflicts_with_all = ["group", "no_dev", "all_groups"])]
    pub only_dev: bool,
```

---

_@zanieb reviewed on 2025-02-04 20:50_

---

_@zanieb reviewed on 2025-02-04 20:52_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2963 on 2025-02-04 20:52_

Hm, this is tough because `--group dev` and `--only-dev` seems fine, similarly for `--group foo` and `--only-group foo`. `--only-group foo` and `--group bar` seems like an error as does `--group foo` and `--only-dev`.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3393 on 2025-02-04 20:53_

See previous comment.

---

_@zanieb reviewed on 2025-02-04 20:53_

---

_@Gankra reviewed on 2025-02-04 20:55_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:2735 on 2025-02-04 20:55_

The idea I'm trying to express here is essentially that `--dev --only-dev` is "what are you even doing". 

This is more clear with `--group A --only-group B`, which is clearly "what no", but rejecting that and *not* rejecting `--group A --only-group A` is a pain for very little benefit. Since `--dev --only-dev` essentially desugars to that, I figured it's best to reject it for uniformity (and the followup code essentially assumes only one of these flags is ever true, although in a trivial way where we could fix it to "saturate" to `--only-dev`).

---

_@zanieb reviewed on 2025-02-04 20:58_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:20 on 2025-02-04 20:58_

"non-group things" not obvious to me. What's that mean?

Is this what we're chatting about on Discord? Should this just be "exclude_project" or something?

---

_@zanieb reviewed on 2025-02-04 20:58_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:20 on 2025-02-04 20:58_

"users of this API" is also a little confusing, idk who that is in this context

---

_@Gankra reviewed on 2025-02-04 21:00_

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:20 on 2025-02-04 21:00_

Yeah basically, this feeds into `.prod()` which is a name that has survived *several* refactors and baffled me for a while (didn't want to churn it).

---

_@zanieb reviewed on 2025-02-04 21:00_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:60 on 2025-02-04 21:00_

Should this assert `!only_groups`? Or are you allowed to do `--only-group foo --all-groups`? If that's banned at the CLI level, should it be mentioned here?

---

_@zanieb reviewed on 2025-02-04 21:01_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:63 on 2025-02-04 21:01_

Perhaps something more aligned with the action we're taking
```suggestion
            // Include the defaults if they're not disabled
```

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:60 on 2025-02-04 21:04_

It could for sure, wasn't sure on the assert culture here.

---

_@Gankra reviewed on 2025-02-04 21:04_

---

_@zanieb reviewed on 2025-02-04 21:05_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2735 on 2025-02-04 21:05_

That makes sense. I'd prefer to just allow redundant options, but understand it can get complicated and if it's not worth it it's not a big deal. Is it a breaking change though?

`--only-dev --no-dev` should still error though, no? Or does it not because we say that "exclusions override" and that's fine?



---

_@zanieb reviewed on 2025-02-04 21:07_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:31 on 2025-02-04 21:07_

This probably doesn't need to be `pub` right? It'd be nice to only have one public constructor.

---

_@zanieb reviewed on 2025-02-04 21:09_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:20 on 2025-02-04 21:09_

I guess regarding this name and "prod" churn, maybe this should be like `ProjectMode` with included and excluded variants as an approximate corollary to `DevMode`? Fine to pursue separately. Here, I'm mostly looking for some clarity in the comments.

---

_@Gankra reviewed on 2025-02-04 21:11_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:2735 on 2025-02-04 21:11_

Yeah I wasn't sure how exclusions and overrides interact. It should probably be tightened up but I didn't want to break something (I'll test it out / read the docs properly to figure out how this should be handled).

In terms of breaking... hmm... seems like the old impl tried harder to rationalize both inputs being provided... but in a bit of an adhoc way... 

https://github.com/astral-sh/uv/blob/ac1004284acfe3e3b0a7cce08741c8396dafc964/crates/uv-configuration/src/dev.rs#L234-L256

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:60 on 2025-02-04 21:11_

Sorry I used assert sort of vaguely there — that could be a user-facing error or an actual assertion or just allowed. The contract just wasn't clear to me.

On assert culture, we're generally anti-assertions in production unless it's truly critical (because panics are unfriendly). I use `debug_assert` a bit to help encode assumptions and catch bugs in development. Charlie doesn't do that. :)

---

_@zanieb reviewed on 2025-02-04 21:11_

---

_@zanieb reviewed on 2025-02-04 21:23_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:174 on 2025-02-04 21:23_

Is this blocking merge? Why wouldn't it add dev to the list?

---

_@zanieb reviewed on 2025-02-04 21:25_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:174 on 2025-02-04 21:25_

Ah this is used to warn if a group is missing? Does the `dev` group always exist since it's special-cased?

---

_@zanieb reviewed on 2025-02-04 23:47_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2735 on 2025-02-04 23:47_

In general, I'd say a breaking change here would not be ideal. I'd suggest PR'ing some tests for edge-cases to `main` first so we can see if they change here? If it seems hard to avoid a regression, then I guess we should wait for 0.6?

---

_@zanieb reviewed on 2025-02-04 23:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:285 on 2025-02-04 23:49_

As an aside `dev: DevGroupsManifest` can be renamed — I think `dev` / `prod` is the wrong split there. It should be `groups` / `project` or similar e.g., `dependency-groups` / `project-dependencies`. Alas the standardized names are a little confusing.

---

_@zanieb reviewed on 2025-02-04 23:53_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:13 on 2025-02-04 23:53_

I think this was called `DevGroups` because it included `dev` and `groups`? Also could be a leftover from the `dev` / `prod` split. Regardless, we can rename to `DependencyGroupsSpecification` or something.

---

_@zanieb reviewed on 2025-02-04 23:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:254 on 2025-02-04 23:54_

This looks a little weird?

Related https://github.com/astral-sh/uv/pull/11224/files#r1942059001

---

_@zanieb reviewed on 2025-02-04 23:55_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:138 on 2025-02-04 23:55_

It's a bit surprising this returns a new type? I'd expect `with_...` to return `Self`. Perhaps it just needs to be renamed?

Related https://github.com/astral-sh/uv/pull/11224/files#r1942058015

---

_@Gankra reviewed on 2025-02-05 14:14_

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:13 on 2025-02-05 14:14_

I'd prefer to do those kinds of renames in a followup PR, otherwise this one will be horribly merge-conflicty with non-trivial changes.

---

_@Gankra reviewed on 2025-02-05 14:33_

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:174 on 2025-02-05 14:33_

I went back and checked and yeah if we try to complain about `--dev` being passed with no defined dev group it breaks some tests. Updated the comment to reflect this.

---

_@Gankra reviewed on 2025-02-05 14:33_

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:31 on 2025-02-05 14:33_

NB there's already other convenience constructors but yeah making this non-pub is fine.

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:20 on 2025-02-05 14:51_

I clarified the comments, I agree more followup renames would be good here.

---

_@Gankra reviewed on 2025-02-05 14:51_

---

_@zanieb reviewed on 2025-02-05 14:52_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:13 on 2025-02-05 14:52_

Yep no problem 

---

_@Gankra reviewed on 2025-02-05 15:14_

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:60 on 2025-02-05 15:14_

the current code and this PR ban `--all-groups --only-group` at the CLI level. However current code allows `--all-groups --only-dev` (which is supposed to be sugar for the former), which sets DevMode::Only and IncludeGroups::All

I am semi-confident that this combination is not well-defined in the current code, and things will act inconsistently between the different APIs (i think it probably ends up behaving a lot like the mythical `--only-all-groups`)

i would be comfortable with denying that being regarded as a bug fix. Added a comment noting this.

---

_@Gankra reviewed on 2025-02-05 15:15_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/mod.rs`:285 on 2025-02-05 15:15_

More renaming for a followup.

---

_@Gankra reviewed on 2025-02-05 15:17_

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:138 on 2025-02-05 15:17_

Hmm maybe `manifest_defaults` (more renaming for later...)

---

_@Gankra reviewed on 2025-02-05 15:18_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/run.rs`:254 on 2025-02-05 15:18_

I agree it's a bit weird, but on the other hand I do kind of like that it's really hammering in "there were two different things I was theoretically supposed to be inputting and I'm doing Neither of them".

---

_@Gankra reviewed on 2025-02-05 15:55_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:2735 on 2025-02-05 15:55_

Ok cool so this is the Final Potential Breakage to hash out (collapsed a few others that I regard as Equivalent).

Looking closer, the current code:
* Hard panics(!) on `--group --only-dev` and `--dev --only-group`
* CLI conflicts  `--group --only-group`
* CLI conflicts `--dev --only-dev`
* Saturates `--only-dev --dev` to `--only-dev`
* CLI conflicts `--only-dev --no-dev`
* Positionally Overrides `--dev --no-dev --dev ...`

So overall the current code hard rejects trying to mix `--only-*` with `--group`/`--dev`, except in the exact case of `--only-dev --dev`, which it turns into `--only-dev`. Which is fair enough, if a bit inconsistent. It's interesting that there's no overriding between `--only-dev and --no-dev` but I don't want to poke that bear in this PR.

I've adjusted things to preserve this behaviour, although the hard panics are now CLI conflicts.

---

_@zanieb reviewed on 2025-02-05 20:08_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1981 on 2025-02-05 20:08_

Weird to allow this but not `// --group and --only-group should error if they name same things` but... eh.

---

_@zanieb approved on 2025-02-05 20:08_

---

_@Gankra reviewed on 2025-02-05 20:08_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:1981 on 2025-02-05 20:08_

Preserving previous behaviour :)

---

_Label `bug` added by @Gankra on 2025-02-05 20:16_

---

_Label `internal` removed by @Gankra on 2025-02-05 20:16_

---

_Renamed from "Rewrite DevGroupSpecification and DevGroupManifest" to "fix handling of --all-group and --no-default-groups flags" by @Gankra on 2025-02-05 20:17_

---

_Renamed from "fix handling of --all-group and --no-default-groups flags" to "fix handling of `--all-groups` and `--no-default-groups` flags" by @Gankra on 2025-02-05 20:17_

---

_Merged by @Gankra on 2025-02-05 20:31_

---

_Closed by @Gankra on 2025-02-05 20:31_

---

_Branch deleted on 2025-02-05 20:31_

---
