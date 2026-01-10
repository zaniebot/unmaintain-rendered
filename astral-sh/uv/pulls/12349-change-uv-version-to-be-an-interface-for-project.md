```yaml
number: 12349
title: "change `uv version` to be an interface for project version reads and edits"
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: release/070
head: gankra/version
created_at: 2025-03-20T21:11:18Z
updated_at: 2025-04-24T17:38:31Z
url: https://github.com/astral-sh/uv/pull/12349
synced_at: 2026-01-10T11:10:39Z
```

# change `uv version` to be an interface for project version reads and edits

---

_Pull request opened by @Gankra on 2025-03-20 21:11_

This is a reimplementation of #7248 with a new CLI interface.

The old `uv version` is now `uv self version` (also it has gained a `--short` flag for parity).
The new `uv version` is now an interface for getting/setting the project version.

To give a modicum of support for migration, if `uv version` is run and we fail to find/read a `pyproject.toml` we will fallback to `uv self version`. `uv version --project .` prevents this fallback from being allowed.

The new API of `uv version` is as follows:

* pass nothing to read the project version
* pass a version to set the project version
* `--bump major|minor|patch` to semver-bump the project version
* `--dry-run` to show the result but not apply it
* `--short` to have the final printout contain only the final version
* `--output-format json` to get the final printout as json

```
$ uv version
myfast 0.1.0

$ uv version --bump major --dry-run
myfast 0.1.0 => 1.0.0

$ uv version 1.2.3 --dry-run
myfast 0.1.0 => 1.2.3

$ uv version 1.2.3
myfast 0.1.0 => 1.2.3

$ uv version  --short 
1.2.3

$ uv version  --output-format json 
{
  "package_name": "myfast",
  "version": "1.2.3",
  "commit_info": null
}
```

Fixes #6298

---

_Comment by @zanieb on 2025-03-20 21:55_

I didn't explore this in my document because I was focused on `uv version` vs `uv metadata` as a decision, but it's sort of an open question if we should use `uv project` instead of `uv metadata`. My primary hesitation is that the existing top-level interface is _project_ based, i.e., we have `uv add` which operates on the project and that `uv project` may make that more confusing? We also have `uv add --script <name>` and I don't think we're likely to add a `uv script` child interface? `uv metadata --script <name>` sort of makes sense with this and our existing design?

---

_@zanieb reviewed on 2025-03-20 21:58_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:550 on 2025-03-20 21:58_

Maybe like,
```suggestion
    /// Don't write a new version to the `pyproject.toml`
    ///
    /// Instead, the version will be displayed.
```

---

_Comment by @Gankra on 2025-03-20 22:04_

I think i like `metadata` over `project`, yeah.

---

_Comment by @Gankra on 2025-03-21 13:25_

Oh worth noting that this does not also support `uv metadata project.version`. It didn't seem terribly valuable for this minimal version of the metadata command, and we can always add that later?

---

_Comment by @zanieb on 2025-03-21 13:39_

> It didn't seem terribly valuable for this minimal version of the metadata command, and we can always add that later?

Agree

---

_Comment by @n4z4m3 on 2025-03-21 14:18_

One possible reason to use project over metadata is that pixi uses project.
Not that consistency with pixi is a major determining factor, but I thought I would mention it.

---

_Comment by @HenriBlacksmith on 2025-03-21 17:12_

Not sure it fits in this PR, but you could add a show-bump like: https://callowayproject.github.io/bump-my-version/reference/cli/#bump-my-version-show-bump

---

_Comment by @HenriBlacksmith on 2025-03-21 17:15_

> Not sure it fits in this PR, but you could add a show-bump like: https://callowayproject.github.io/bump-my-version/reference/cli/#bump-my-version-show-bump

Actually it could just be some side effect of a --dry-run with no target maybe?

---

_Comment by @zanieb on 2025-03-21 17:19_

Yeah that's `--dry-run --short`

---

_Comment by @zanieb on 2025-03-21 19:56_

I'm sort of leaning towards transitioning `uv version`, given `uv self version` is a more sensible long-term interface there and we can avoid confusion by displaying the project package name. I'll ponder this some more though.

---

_Comment by @samypr100 on 2025-03-22 02:04_

I'm a tad more in favor of `uv metadata` as it naturally drives me to think about project metadata. `uv version` or `uv self version` makes me think of `uv --version` or `uv self --version` weirdly enough😆

---

_Comment by @samypr100 on 2025-03-22 02:07_

Perhaps a future thought, given feature requests like https://github.com/astral-sh/uv/issues/6794 could this interface evolve towards supporting updating dependency versions in pyproject.toml as well (as it would follow semantics similar to bump) or would you still see that being part of other existing/new top-level interfaces?

---

_Comment by @InSyncWithFoo on 2025-03-22 03:59_

I have a use case for this command: [In-editor version bumping](https://insyncwithfoo.github.io/ryecharm/rye/intentions/#bump-project-version) (currently this feature relies on Rye).

![](https://github.com/user-attachments/assets/e79deb25-5491-446b-bb72-24cacf6931e7) 

I like the command's design in whole. One minor annoyance is that `uv metadata` is a bit lengthy; I can't think of a better name though.

---

_Comment by @rumpelsepp on 2025-03-22 08:00_

Simply `meta` instead of `metadata` perhaps?

---

_@struckchure approved on 2025-03-22 08:06_

---

_Comment by @crimoniv on 2025-03-22 19:58_

Hi there! Just my 2 cents on this:

- This is the first release with a minimal/preliminary version.
- There's still an ongoing discussion about the (sub)command interface and naming (e.g., `version` vs `meta` vs `metadata`).
- Some features might be considered for future iterations (e.g., pre-release versions, calver support, git tags, custom version sources...).

Would it make sense to include a warning indicating that the command is experimental and subject to future changes? Similar to the warning that was shown with [uv publish](https://github.com/astral-sh/uv/blob/fb2944599942448aef0f542ecd704ede0b27fde7/crates/uv/src/lib.rs#L1213).

Printing the warning to stderr shouldn't interfere with scripted usage, and will allow to 1) defer commitment on the interface and 2) start collecting usage feedback.

---

_Comment by @zanieb on 2025-03-22 23:25_

> could this interface evolve towards supporting updating dependency versions in pyproject.toml as well (as it would follow semantics similar to bump) or would you still see that being part of other existing/new top-level interfaces?

I think we'll probably create a top-level interface for that since other dependency management commands are there, but I'm not sure yet. That design work is planned to happen soon. My thoughts about an expansion of the `metadata` section are mostly around editing or showing other `pyproject.toml` fields as well as dumping project metadata for integration with other tooling (i.e., `cargo metadata`).

---

_Comment by @uglybug on 2025-03-31 11:03_

I guess I could just clone, build and test this locally, but I thought it might be easier just to ask the question: how does this cope with `dynamic = ["version"]`? It should gracefully refuse to bump or set the version at least. Even reading the version in this case I guess is impossible without actually building the project. So, again, it should gracefully exit. I just wouldn't want it to trample over a dynamic version if someone accidentally ran it to set a static one, I think. Maybe there is a use case there, though, if migrating from dynamic to static versioning. Just some thoughts anyway.

---

_Comment by @Gankra on 2025-03-31 14:54_

The current implementation errors the command on all operations if the `version` field is not defined (which I believe would be the case if `dynamic = ["version"]`).

I should do a cleanup pass on the message though.

---

_Comment by @Gankra on 2025-03-31 20:39_

Pushed up a version of `uv metadata version` that:

* has better error message for missing version field (i.e. if `dynamic = ["version"]`)
* takes a superset of the flags of `uv version` (notably `--output-format=json`)
  * some minor design work here on if we want to change/update the json format (see snapshots) 
* respects `--project`
* if search for a pyproject.toml fails, then we:
  * default: warn and run `uv --version`
  *  if `--project` was passed: error
 
I left out the actual step of replacing the current `uv version` with `uv metadata version` (and I guess deleting `uv metadata version`?) to make it easier to review the diff, but I'll do that swap in a followup commit.

The existence of `--output-format=json` also breaks the parity between `uv version` and `uv --version`, as the latter does not respect that flag! As such to complete the migration plan I believe we must add `uv self version` which is just the current `uv version` (or I guess mess with clap and make `--version` respect it? but gross to have more random top-level flags...).

---

_Label `enhancement` added by @Gankra on 2025-04-01 16:43_

---

_Label `breaking` added by @Gankra on 2025-04-01 16:43_

---

_Added to milestone `v0.7.0` by @Gankra on 2025-04-01 16:43_

---

_Renamed from "add `uv metadata version` command" to "change `uv version` to be an interface for project version reads and edits" by @Gankra on 2025-04-01 16:46_

---

_Comment by @Gankra on 2025-04-01 16:46_

Ok, updated the main comment and pushed up the latest changes

---

_Review requested from @konstin by @zanieb on 2025-04-01 20:34_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:725 on 2025-04-01 21:59_

Should this say something about it being dynamic?

---

_@zanieb reviewed on 2025-04-01 21:59_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:763 on 2025-04-01 21:59_

This error seems especially confusing

---

_@zanieb reviewed on 2025-04-01 21:59_

---

_@zanieb reviewed on 2025-04-01 22:01_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:806 on 2025-04-01 22:01_

Reading this... I wonder if we should suggest `--preview` to make this an error? `--project .` is a weird thing for someone to keep around later?

---

_@zanieb reviewed on 2025-04-01 22:03_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:832 on 2025-04-01 22:03_

Can we test this where there's not a project as well? The `managed = false` case is pretty niche

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:543 on 2025-04-01 22:05_

```suggestion
    /// Only show the version
    ///
    /// By default, uv will show the project name before the version.
```

---

_@zanieb reviewed on 2025-04-01 22:05_

---

_@zanieb reviewed on 2025-04-01 22:07_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:532 on 2025-04-01 22:07_

```suggestion
    /// Set the project version to this value
    ///
    /// To update the project using semantic versioning components instead, use `--bump`.
```
(or similar)

---

_@zanieb reviewed on 2025-04-01 22:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/version.rs`:89 on 2025-04-01 22:08_

We should probably special case the error message for `--bump` values here? To ease migration for Poetry users?

---

_@zanieb reviewed on 2025-04-01 22:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/version.rs`:89 on 2025-04-01 22:08_

(I am assuming they're not valid versions)

---

_@Gankra reviewed on 2025-04-02 13:23_

---

_Review comment by @Gankra on `crates/uv/tests/it/version.rs`:832 on 2025-04-02 13:23_

I wasn't sure if "project missing" was something we could reliably enforce (given we spider into parent dirs)

---

_@zanieb reviewed on 2025-04-02 13:27_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:832 on 2025-04-02 13:27_

Hmm... I guess during testing we should stop discovery at the uv test directory (`UV_INTERNAL__TEST_DIR`)?

I'm sure we have tests that count on not finding a `pyproject.toml` in a parent though.

---

_@Gankra reviewed on 2025-04-02 17:44_

---

_Review comment by @Gankra on `crates/uv/src/commands/version.rs`:89 on 2025-04-02 17:44_

good idea

---

_@Gankra reviewed on 2025-04-02 17:48_

---

_Review comment by @Gankra on `crates/uv/tests/it/version.rs`:806 on 2025-04-02 17:48_

Conversely `--preview` will enable a ton of random other things right..? Maybe that's fine though.

---

_Comment by @Gankra on 2025-04-02 17:53_

Pushed up tweaks -- only thing not done is the `--preview` idea.

---

_Comment by @Gankra on 2025-04-02 18:58_

Added the `--preview` change

---

_Comment by @T-256 on 2025-04-06 12:17_

This is Great ❤️
There is also an open issue in Rye about [bumping to prereleases versions](https://github.com/astral-sh/rye/issues/753), Do we need to consider that in uv too?


---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:984 on 2025-04-07 14:09_

I got confused about why we're creating a `project` table with only a `version` entry (while a `name` is also required), can we `get` instead for those?

```suggestion
            .get("project")
            .ok_or(Error::MalformedWorkspace)?
```

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:991 on 2025-04-07 14:12_

nit: `Item::as_str`

---

_Review comment by @konstin on `crates/uv/src/commands/version.rs`:76 on 2025-04-07 14:20_

This function returns only this one error variant, do we need this matching or should it be an `Option<T>` instead?

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:1023 on 2025-04-07 14:21_

A missing `project` table should error, otherwise we risk creating an invalid `project` table with a `version` but no `name` (should be solved by `entry()` -> `get()`, too)

---

_Review comment by @konstin on `crates/uv-cli/src/version.rs`:22 on 2025-04-07 14:37_

Is there a reason to not make this a `PackageName`?

---

_Review comment by @konstin on `crates/uv/tests/it/version.rs`:68 on 2025-04-07 14:47_

Should we emit a commit info field when we don't query that information for the non-legacy fallback path?

---

_Review comment by @konstin on `crates/uv/tests/it/version.rs`:474 on 2025-04-07 16:06_

Edge case test suggestion: `1!1a1.post1.dev1+deadbeef1` (you can drop the epoch fwiw, it's not used in the ecosystem)

---

_Review comment by @konstin on `crates/uv/src/commands/version.rs`:32 on 2025-04-07 16:12_

```suggestion
/// Read or update project version (`uv version`)
```

---

_Review comment by @konstin on `crates/uv/src/commands/version.rs`:170 on 2025-04-07 17:35_

`any_prerelease` and `is_local`  catch this more generally. another option is to move bumping into `uv_pep440` and using the `VersionFull` type

---

_Comment by @konstin on 2025-04-07 18:37_

I agree that we should improve the prerelease handling, usually when bumping a prerelease version is want to get to either jump to the next version of that prerelease (1.2.3a1 -> 1.2.3a2) or to the stable version of that release instead of the next release (1.2.3a2 -> 1.2.3).

---

_@konstin reviewed on 2025-04-07 18:37_

---

_@konstin reviewed on 2025-04-07 18:42_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:984 on 2025-04-07 18:42_

The span here is wrong, i wanted both lines.

---

_Review comment by @konstin on `crates/uv/src/commands/version.rs`:79 on 2025-04-07 19:03_

nit: backticks around the path

---

_@konstin reviewed on 2025-04-07 19:04_

---

_@Gankra reviewed on 2025-04-08 16:54_

---

_Review comment by @Gankra on `crates/uv/src/commands/version.rs`:76 on 2025-04-08 16:54_

Ah good catch, I fixed the impl to properly forward version parse errors

---

_@Gankra reviewed on 2025-04-08 16:55_

---

_Review comment by @Gankra on `crates/uv/tests/it/version.rs`:474 on 2025-04-08 16:55_

epoch?

---

_@konstin reviewed on 2025-04-08 16:57_

---

_Review comment by @konstin on `crates/uv/tests/it/version.rs`:474 on 2025-04-08 16:57_

the `1!` in front, which allows switching versioning schemes

---

_Review comment by @Gankra on `crates/uv/tests/it/version.rs`:474 on 2025-04-08 16:57_

Also incredible nightmare input

---

_@Gankra reviewed on 2025-04-08 16:58_

---

_Review comment by @Gankra on `crates/uv/tests/it/version.rs`:474 on 2025-04-08 17:03_

oh my god epochs oh my god

---

_@Gankra reviewed on 2025-04-08 17:03_

---

_@zanieb reviewed on 2025-04-08 17:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/version.rs`:79 on 2025-04-08 17:05_

We don't use backticks following a `:` usually

---

_Review comment by @Gankra on `crates/uv/src/commands/version.rs`:79 on 2025-04-08 17:26_

That seems inconsistent with style elsewhere? normally it's trailing-colon *xor* wrapped-in-ticks?

---

_@Gankra reviewed on 2025-04-08 17:26_

---

_@Gankra reviewed on 2025-04-08 17:34_

---

_Review comment by @Gankra on `crates/uv/tests/it/version.rs`:68 on 2025-04-08 17:34_

The current format for what is now `uv self version --output-format json` includes the field even if it's null, so I figured I wouldn't mess with the schema, and that it would be good to have the same schema for both commands?

---

_@Gankra reviewed on 2025-04-08 17:38_

---

_Review comment by @Gankra on `crates/uv/src/commands/version.rs`:170 on 2025-04-08 17:38_

I'm increasingly dubious that this even needs a warning...

---

_@Gankra reviewed on 2025-04-08 17:59_

---

_Review comment by @Gankra on `crates/uv-cli/src/version.rs`:22 on 2025-04-08 17:59_

It's a write-only format and the version was already being flattened to a String, so I kept it consistent.

---

_@konstin reviewed on 2025-04-09 09:44_

---

_Review comment by @konstin on `crates/uv/tests/it/version.rs`:68 on 2025-04-09 09:44_

I expect this will be confusing when someone expects to get `commit_info` for their package but even if it is in a git repository, we never give them a value for `commit_info`

---

_@konstin approved on 2025-04-09 09:45_

---

_@konstin reviewed on 2025-04-09 09:46_

---

_Review comment by @konstin on `crates/uv/src/commands/version.rs`:170 on 2025-04-09 09:46_

Intuitively, I'd expect a `stable` bump that goes from prerelease to stable, and `alpha`, `beta`, `rc`, `dev`, `post` bump types that bump inside of pre/post-releases 

---

_Merged by @Gankra on 2025-04-24 17:38_

---

_Closed by @Gankra on 2025-04-24 17:38_

---

_Branch deleted on 2025-04-24 17:38_

---
