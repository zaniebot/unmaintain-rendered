```yaml
number: 13689
title: "Add experimental `uv sync --output-format json`"
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: gankra/sync-json
created_at: 2025-05-27T19:56:23Z
updated_at: 2025-07-14T14:53:41Z
url: https://github.com/astral-sh/uv/pull/13689
synced_at: 2026-01-12T16:10:48Z
```

# Add experimental `uv sync --output-format json`

---

_@Gankra_

This is a continuation of the work in 

* #12405 

I have:
* moved to an architecture where the human output is derived from the json structs to centralize more of the printing state/logic
* cleaned up some of the names/types
* added tests
* removed the restriction that this output is --dry-run only

I have not yet added package info, which was TBD in their design.

---

_Label `enhancement` added by @Gankra on 2025-05-27 19:56_

---

_@Gankra reviewed on 2025-05-27 19:57_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:3205 on 2025-05-27 19:57_

Out of date comment

---

_@Gankra reviewed on 2025-05-27 19:58_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:923 on 2025-05-27 19:58_

This is a new thing from the previous PR, since the human messages all branched on this info

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:947 on 2025-05-27 19:59_

I think we could come up with a better name than this one.

---

_@Gankra reviewed on 2025-05-27 19:59_

---

_@Gankra reviewed on 2025-05-27 20:00_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:949 on 2025-05-27 20:00_

The previous PR had this arbitrary split of "replace" vs "update" where each was exclusively used by Sync or Lock (respectively). I split that enum so they didn't need to share state, but we could possibly rename Replace to Update for consistency?

---

_@Gankra reviewed on 2025-05-27 20:02_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:977 on 2025-05-27 20:02_

I duplicate this state into both SyncReport and LockReport kinda arbitrarily -- it could live in the parent type and only exist once. I have this vague sense these types might be reusable elsewhere so this felt right? (and it's really harmless to duplicate)

---

_@Gankra reviewed on 2025-05-27 20:03_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:1045 on 2025-05-27 20:03_

Entries like this are me trying to make explicit all the places where the old code kinda "randomly" didn't print anything. Helped make it clear to me that my refactor was correct.

---

_@Gankra reviewed on 2025-05-27 20:13_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:302 on 2025-05-27 20:13_

Hmm, this probably shouldn't be null...

---

_Review requested from @oconnor663 by @zanieb on 2025-05-27 20:38_

---

_Assigned to @zanieb by @zanieb on 2025-05-27 20:38_

---

_Review requested from @konstin by @zanieb on 2025-05-27 20:39_

---

_@x0rw reviewed on 2025-05-27 20:47_

---

_Review comment by @x0rw on `crates/uv/src/commands/project/sync.rs`:949 on 2025-05-27 20:47_

I got a headache trying to come up with a consistent and suitable name for this.

---

_Review comment by @x0rw on `crates/uv/src/commands/project/sync.rs`:942 on 2025-05-27 20:56_

Since the struct was changed, it might be good to update both comments.



---

_@x0rw reviewed on 2025-05-27 20:58_

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:971 on 2025-05-28 11:25_

Can we generate jsonschema for our JSON output? The output here gets complex enough that we should have some documentation for it, and jsonschema is self-documenting and allows generating and validating consumer code. 

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:976 on 2025-05-28 11:26_

`dry` -> `dry_run`, for consistency

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:974 on 2025-05-28 11:27_

```suggestion
    /// Whether a project or a script environment is used.
```

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:983 on 2025-05-28 11:27_

Should we add the Python interpreter flavor too?

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:978 on 2025-05-28 11:29_

```suggestion
    /// Whether the virtual environment was reused or recreated.
```

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:950 on 2025-05-28 11:29_

Should that be past tense?

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:928 on 2025-05-28 11:37_

```suggestion
    /// Represents a lockfile and whether it needs to be created or update.
```

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:417 on 2025-05-28 11:38_

Can we DRY this up by only determining the action in the branches?

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:436 on 2025-05-28 11:51_

I do like ndjson output that, e.g., Cargo's `--message-format` uses, and this would allow reporting builds and such too, but I see that this one is easier to start with.

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:849 on 2025-05-28 11:53_

Can we report the workspace root too? That's probably the more interesting path for IDEs and similar tools.

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:908 on 2025-05-28 11:54_

Here use `_dir`, for the env we use `_path`, we should have a shared nomenclature here (maybe `root`?)

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:942 on 2025-05-28 11:57_

Does this action describe other changes than those to the venv?

---

_@konstin reviewed on 2025-05-28 11:58_

---

_@Gankra reviewed on 2025-05-28 15:28_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:971 on 2025-05-28 15:28_

In the past I've done that kind of thing, but I've found the value extremely low. I think users will just see what uv emits and go with that.

JSON Schema is extremely hard for a human to read, and IME it doesn't really help you consume json someone else produced. Its primary value is for when a human is hand-writing json inputs.

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:436 on 2025-05-28 15:29_

Yeah agreed on both fronts.

---

_@Gankra reviewed on 2025-05-28 15:29_

---

_@zanieb reviewed on 2025-05-28 15:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:971 on 2025-05-28 15:32_

I think this ties into the larger story of JSON output abstractions in uv, which is sort of out of scope here. cc @oconnor663 who may take a look at that in the future.

---

_@Gankra reviewed on 2025-05-28 18:12_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:942 on 2025-05-28 18:12_

It shouldn't but I'm not sure if you're making a subtle semantic distinction.

---

_@Gankra reviewed on 2025-05-28 18:39_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:417 on 2025-05-28 18:39_

Wow good call, I got so tunnel-visioned during the refactor I didn't realize how simple I had made this code!

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:950 on 2025-05-28 18:46_

It's odd because it's past-tense in normal runs and future-tense/hypothetical in --dry-run.

---

_@Gankra reviewed on 2025-05-28 18:46_

---

_@Gankra reviewed on 2025-05-28 18:47_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:983 on 2025-05-28 18:47_

Is there a clear enum to use? Looking at the various methods on Interpreter it seems like there's a few complex notions bouncing around.

---

_@zanieb reviewed on 2025-05-28 18:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:983 on 2025-05-28 18:49_

I presume he means the implementation name?

---

_@Gankra reviewed on 2025-05-28 18:58_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/sync.rs`:849 on 2025-05-28 18:58_

Yeah actually I'm not even sure how valuable the project_path is. Although there's a bit of a can of worms here with project_root vs script_path being aiui mutually exclusive?

---

_Review comment by @oconnor663 on `crates/uv/src/commands/project/sync.rs`:201 on 2025-05-28 22:21_

Clearly not new in this diff, but it looks like `ProjectEnvironment` and `ScriptEnvironment` are mostly (actually, 100%?) duplicated. Is that bad? It seems like it's leading to more duplication in both of these matches.

---

_Review comment by @oconnor663 on `crates/uv/src/commands/project/sync.rs`:284 on 2025-05-28 22:32_

This part seems pretty easy to forget about and mess up if/when we add other early exits. I'm not sure what the better approach is, though, and I assume we don't want the report to just print itself on Drop (which would include error cases). Maybe an outer function could own the report and pass it down by mutable reference to an inner function, which would be free to short circuit? But not obvious to me that that's better.

---

_Review comment by @oconnor663 on `crates/uv/tests/it/sync.rs`:326 on 2025-05-28 22:45_

What's the plan with the human readable stderr output? Is this the sort of thing that might get subsumed by NDJSON in the future?

---

_@oconnor663 reviewed on 2025-05-28 22:45_

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:947 on 2025-05-30 08:33_

`Fresh` is usually used in caching.

---

_@konstin reviewed on 2025-05-30 08:33_

---

_@konstin reviewed on 2025-05-30 08:33_

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:942 on 2025-05-30 08:33_

no this was really just about the difference between the container docstring and the variant docstrings.

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:950 on 2025-05-30 08:36_

We should have consistent grammar between e.g. `The environment would be updated.` and `Create a new environment.` (indicative vs conditional) (unless I've missed something and we are always creating but the update is not done in a dry run?)

---

_@konstin reviewed on 2025-05-30 08:36_

---

_@konstin reviewed on 2025-05-30 08:37_

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:935 on 2025-05-30 08:37_

nit: we can move the actual printing out of the match, we can have a `Option<&str>` returned from the match.

---

_@zanieb reviewed on 2025-06-02 22:39_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:60 on 2025-06-02 22:39_

A bit weird to impl this when we don't for the other formats, but 🤷‍♀️ 

---

_@zanieb reviewed on 2025-06-02 22:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:947 on 2025-06-02 22:42_

Maybe just `Check`?

---

_@zanieb reviewed on 2025-06-02 22:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:950 on 2025-06-02 22:43_

This is my first instinct

```
/// The environment was checked and required no updates.
/// The environment was updated.
/// A new environment was created.
```

---

_@zanieb reviewed on 2025-06-02 22:45_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:950 on 2025-06-02 22:45_

I think I agree these should all be past-tense? Dry-run fills in the blank "The environment would be ___"

---

_@zanieb reviewed on 2025-06-02 22:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:907 on 2025-06-02 22:46_

We should probably nest these, e.g., into a `python` child structure.

---

_@zanieb reviewed on 2025-06-02 22:49_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:326 on 2025-06-02 22:49_

I think so, yeah

---

_@zanieb reviewed on 2025-06-02 22:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:359 on 2025-06-02 22:50_

I would move consider moving `python` to the top-level. We find an interpreter when locking too.

---

_@zanieb reviewed on 2025-06-02 22:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:355 on 2025-06-02 22:51_

We might even just want to move `environment` to a top-level key with children?

---

_@zanieb reviewed on 2025-06-02 22:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:363 on 2025-06-02 22:52_

I'd do `path` or `lockfile` or `lockfile_path`?

---

_@zanieb reviewed on 2025-06-02 22:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:4584 on 2025-06-02 22:54_

Should we version the JSON output? 🤔 

---

_@zanieb reviewed on 2025-07-10 14:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:935 on 2025-07-10 14:10_

eh this is kind of annoying because of the mixed use of `dimmed()`

---

_Label `preview` added by @zanieb on 2025-07-11 12:42_

---

_Renamed from "Add `uv sync --format json`" to "Add experimental `uv sync --output-format json`" by @zanieb on 2025-07-11 12:42_

---

_@zanieb reviewed on 2025-07-11 13:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:175 on 2025-07-11 13:06_

Sorry for the churn here, these changed during testing and while the snapshot it the same the new version of insta uses different escapes.

---

_@zanieb reviewed on 2025-07-14 14:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:835 on 2025-07-14 14:05_

I'm probably going to move all these report types elsewhere in a follow-up

---

_@zanieb reviewed on 2025-07-14 14:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:835 on 2025-07-14 14:06_

(we'll be able to re-use them in other commands)

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/sync.rs`:842 on 2025-07-14 14:23_

```suggestion
        Self {
```

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/sync.rs`:857 on 2025-07-14 14:23_

```suggestion
        Self {
```

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/sync.rs`:1063 on 2025-07-14 14:25_

```suggestion
        Self {
```

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/sync.rs`:1036 on 2025-07-14 14:25_

```suggestion
        Self {
```

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/sync.rs`:881 on 2025-07-14 14:26_

```suggestion
        Self {
```

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/sync.rs`:1160 on 2025-07-14 14:26_

```suggestion
        Self {
```

---

_@jtfmumm reviewed on 2025-07-14 14:27_

---

_@jtfmumm approved on 2025-07-14 14:30_

Added some minor suggestions but looks good

---

_Merged by @zanieb on 2025-07-14 14:53_

---

_Closed by @zanieb on 2025-07-14 14:53_

---

_Branch deleted on 2025-07-14 14:53_

---
