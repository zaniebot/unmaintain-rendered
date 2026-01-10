```yaml
number: 16375
title: "[red-knot] Add support for `knot check <paths>`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/check-paths
created_at: 2025-02-25T16:43:21Z
updated_at: 2025-03-03T13:10:25Z
url: https://github.com/astral-sh/ruff/pull/16375
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Add support for `knot check <paths>`

---

_Pull request opened by @MichaReiser on 2025-02-25 16:43_

## Summary

This PR adds support for an optional list of paths that should be checked to `knot check`. 

E.g. to only check the `src` directory

```sh
knot check src
```

The default is to check all files in the project but users can reduce the included files by specifying one or multiple optional paths.

The main two challenges with adding this feature were:

* We now need to show an error when one of the provided paths doesn't exist. That's why this PR now collects errors from the project file indexing phase and adds them to the output diagnostics. The diagnostic looks similar to ruffs (see CLI test)
* The CLI should pick up new files added to included folders. For example, `knot check src --watch` should pick up new files that are added to the `src` folder. This requires that we now filter the files before adding them to the project. This is a good first step to supporting `include` and `exclude`. 


The PR makes two simplifications:

1. I didn't test the changes with case-insensitive file systems. We may need to do some extra path normalization to support those well. See https://github.com/astral-sh/ty/issues/189
2. Ideally, we'd accumulate the IO errors from the initial indexing phase and subsequent incremental indexing operations. For example, we should preserve the IO diagnostic for a non existing `test.py` if it was specified as an explicit CLI argument until the file gets created and we should show it again when the file gets deleted. However, this is somewhat complicated because we'd need to track which files we revisited (or were removed because the entire directory is gone). I considered this too low a priority as it's worth dealing with right now.

The implementation doesn't support symlinks within the project but that is the same as Ruff and is unchanged from before this PR.



Closes https://github.com/astral-sh/ruff/issues/14193

## Test Plan

Added CLI and file watching integration tests. Manually testing.

---

_Label `red-knot` added by @MichaReiser on 2025-02-25 16:43_

---

_Renamed from "[red-knot] Early version of `check <paths>`" to "[red-knot] Add support for `knot check <paths>`" by @MichaReiser on 2025-02-27 13:26_

---

_Marked ready for review by @MichaReiser on 2025-02-27 14:18_

---

_Review requested from @carljm by @MichaReiser on 2025-02-27 14:18_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-27 14:18_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-27 14:18_

---

_Review comment by @carljm on `crates/red_knot_project/src/lib.rs`:80 on 2025-02-27 23:15_

How is this different from the already-present `open_fileset` above? How does the meaning of "included" differ from the meaning of "open"? (The current doc comment on `open_fileset` sounds like it is basically the same thing: these are the files to check.) Do we need both of these concepts?

---

_Review comment by @carljm on `crates/red_knot_project/src/lib.rs`:124 on 2025-02-27 23:20_

I'm not sure I understand the implication of the `.gitignore` handling from a user perspective. Do we not check git-ignored files? Do we treat them as non-existent if you try to import them? It would be cool if we somewhere documented specifically how we use `.gitignore`. (Maybe this question is really not very related to this PR.)

---

_Review comment by @carljm on `crates/red_knot_project/src/db/changes.rs`:79 on 2025-02-27 23:26_

My assumption is that if you run `knot check some/path.py --watch` and then add `some/foo.py`, we still only want to check and output diagnostics for `some/path.py`, and the implementation and tests seem to confirm that.

But then I am confused by the comment here "That is because we want to include those files in the next check run." I assume that in this comment you are just using a different meaning of the word "include", or not considering the possibility that we have an explicitly-specified included file list? Because if we do, new files observed by the watcher should not be included.

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:276 on 2025-02-27 23:34_

I suspect we should have a `-q` option to silence this type of output, but that's a separate feature.

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:946 on 2025-02-27 23:39_

Is this making the test pass cross-platform?

---

_Review comment by @carljm on `crates/red_knot/tests/file_watching.rs`:579 on 2025-02-27 23:52_

```suggestion
    // and write a second file in the root directory -- this should not be included
```

---

_Review comment by @carljm on `crates/red_knot/tests/file_watching.rs`:589 on 2025-02-27 23:54_

Just checking my understanding here: if one of the "included" files imported one of the "not included" files, that "not included" file would still end up in the indexed files, right? But it not be in the "included" set and we wouldn't check it.

---

_Review comment by @carljm on `crates/red_knot_project/src/lib.rs`:75 on 2025-02-27 23:58_

On first read I found the use of the term "included" here in this method and throughout this PR to be confusing, because "included" can mean lots of different things, and it seemed like e.g. "checked" would more clearly communicate the actual effect.

But after reading the whole PR I think I understand the reason for this term: it's connected to the future `include` and `exclude` configuration options we will add.

But I think this doc comment would be a good place to have a clearer description of what "included" actually means: that these are the files we will check and emit diagnostics for.

---

_Review comment by @carljm on `crates/red_knot_project/src/lib.rs`:121 on 2025-02-28 00:02_

If "included" means "one of the paths that should be checked", then I find this sentence confusing, both because "included in the project" sounds like it has a different meaning (under the project root?), and the "AND one of the paths that should be checked" also suggests that isn't already part of the meaning of "included". If we are going to use "included" to mean "paths requested for checking", let's define what that means in one place (I would do it above on the struct field) and then be consistent and clear about that.
```suggestion
    /// Returns `true` if `path` is both part of the project, and included (see `included_paths_list`).
```

---

_Review comment by @carljm on `crates/red_knot_project/src/walk.rs`:38 on 2025-02-28 00:05_

This suggests that "considered being part of the project" means the same thing as "included", but I find that confusing, because I would say if we have `project/a.py` and `project/b.py`, and we run `knot check project/a.py`, then only `a.py` is "included", but both of the files are still "part of the project", we just aren't checking all of them.
```suggestion
    /// A file is included if it is a sub path of the project's root
```

---

_Review comment by @carljm on `crates/red_knot_project/src/walk.rs`:100 on 2025-02-28 00:09_

```suggestion
        // It's unnecessary to filter on included paths because it only iterates over those to start with.
```

---

_Review comment by @carljm on `crates/red_knot_project/src/walk.rs`:104 on 2025-02-28 00:09_

```suggestion
            .expect("included_paths_or_root to never return an empty iterator")
```

---

_Review comment by @carljm on `crates/red_knot_project/src/walk.rs`:107 on 2025-02-28 00:09_

```suggestion
    /// Creates a walker for indexing the project files incrementally.
```

---

_Review comment by @carljm on `crates/red_knot_project/src/walk.rs`:110 on 2025-02-28 00:10_

Seems like maybe you also used "checked" instead of "included" in an earlier draft of this PR. But whichever we use, let's be consistent.

```suggestion
    /// that aren't part of the included files.
```

---

_Review comment by @carljm on `crates/red_knot_project/src/watch/project_watcher.rs`:78 on 2025-02-28 00:14_

I don't think `deduplicate_nested_paths` really find the common ancestor? If you give it two unrelated paths it will return both of them, not their common ancestor. It just removes paths that are entirely nested within another one. 
```suggestion
        // Watch both the project root and any paths provided by the user on the CLI (removing any redundant nested paths).
```

---

_@carljm approved on 2025-02-28 00:14_

Looks great!

---

_@dhruvmanila reviewed on 2025-02-28 06:39_

---

_Review comment by @dhruvmanila on `crates/red_knot_project/src/files.rs`:220 on 2025-02-28 06:39_

```suggestion
        Arc::get_mut(&mut self.indexed).expect("All references to `FilesSet` should have been dropped")
```

---

_@dhruvmanila reviewed on 2025-02-28 06:43_

---

_Review comment by @dhruvmanila on `crates/red_knot_project/src/lib.rs`:97 on 2025-02-28 06:43_

nit: we could possibly remove the `_list` suffix to make it `included_paths` ?

---

_@MichaReiser reviewed on 2025-02-28 08:30_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:946 on 2025-02-28 08:30_

Yes, this is for my friend Windows

---

_@MichaReiser reviewed on 2025-02-28 08:31_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:75 on 2025-02-28 08:31_

Funny, I used `checked_paths` first and then renamed it to `included_paths` because the concept is similar to the `include` setting (which are globs and not paths but it's otherwise the same idea).

---

_@MichaReiser reviewed on 2025-02-28 08:32_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:97 on 2025-02-28 08:32_

I've added manual setters and getters that are named `included_paths` that would conflict with the salsa generated once.

---

_Comment by @MichaReiser on 2025-02-28 08:35_

I get the impression that writing fewer code comments results in fewer review comments 😆 

---

_@MichaReiser reviewed on 2025-02-28 08:45_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:589 on 2025-02-28 08:45_

No, we don't index `not_included` files. We don't want to walk the entire project tree if you run `knot check a.py`. That's why we only index files that are relevant for the current session. 

You can also see this from this test. The `indexed_project_files` doesn't contain `src/test2.py`

---

_@MichaReiser reviewed on 2025-02-28 08:48_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:75 on 2025-02-28 08:48_

I think part of the confusion is that we sort of abuse the `Project` struct for anything session-related (I don't have a good name for it). `open_files`, `included_paths` and `files` aren't project-specific. They store state relevant for the current (check) session, and some of the information is derived from the project, but it's a more narrow view than just the project. 

The reason why I put those fields on `Project` for now is that it's somewhat annoying to have global - alive forever structs because they require a new `Db` trait method, but I think it might be worth making the split if it helps clarify things. I don't plan on doing this split in this PR, but I'm interested to hear if you think that would help clarify concepts

---

_@MichaReiser reviewed on 2025-02-28 08:54_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:80 on 2025-02-28 08:54_

I can add a comment. In short, we may be able to remove `open_fileset` but I'm not sure yet. The `open_fileset` is currently used for wasm and I plan on using it in VS code when users only want to check the open files but not the entire workspace.

The main difference between the two is that `open_fileset` is a set of files, unlike `included_paths` that is a list of paths that require indexing to resolve them to a hash set. That makes `open_fileset` much cheaper if we know the files we want to check -- which is the case for IDEs and WASM. 

I want to keep the field for now and revisit it when we work on the LSP. 

---

_@MichaReiser reviewed on 2025-02-28 08:57_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:124 on 2025-02-28 08:57_

We do respect `.gitignore` but only when indexing files (because discovering and parsing `.ignore` files is expensive. Users can opt out from `.gitignore` support. The main motivation for this comment is that calling `is_included` for every path and collecting them into a hash set will give you **more files** than calling `files` which does respect `.gitignore` files.


---

_Review comment by @MichaReiser on `crates/red_knot_project/src/db/changes.rs`:79 on 2025-02-28 09:34_

Let me revisit the comment. This is not even specific about the changes in this PR. I added it for myself because I had to think about why `Files::sync_recursively` isn't enough.

---

_@MichaReiser reviewed on 2025-02-28 09:34_

---

_@MichaReiser reviewed on 2025-02-28 09:49_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:276 on 2025-02-28 09:49_

Yeah. I'm also not sure if we should use `writeln` here but that's a different story :)

---

_Review comment by @carljm on `crates/red_knot_project/src/lib.rs`:92 on 2025-02-28 16:31_

```suggestion
    /// In short, `open_files` is cheaper in contexts where the set of files is known, like
```

---

_Review comment by @carljm on `crates/red_knot/tests/file_watching.rs`:589 on 2025-02-28 16:34_

> The `indexed_project_files` doesn't contain `src/test2.py`

Yes, I thought perhaps the reason for this was that none of the included files import `test2`, thus my question. But I think maybe I just need to refresh my memory of how our files support works in general. I guess indexed files are just files that we "discover" for checking via tree-walking, but are not the full set of files we know about as Salsa inputs.

---

_Review comment by @carljm on `crates/red_knot_project/src/lib.rs`:75 on 2025-02-28 16:35_

Yes, it's not urgent but I do think splitting out "our understanding of the user's project" from "our understanding of what the user has requested in the current checking session" would be helpful in making this clearer!

---

_Comment by @carljm on 2025-02-28 16:37_

> I get the impression that writing fewer code comments results in fewer review comments 😆

Maybe! Or maybe then you get review comments saying "I don't understand this, please add a comment to explain" 😆 

---

_@carljm approved on 2025-02-28 16:37_

---

_@MichaReiser reviewed on 2025-03-03 12:52_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:589 on 2025-03-03 12:52_

Yes, this is correct. `File`s can be created by many different parts in Red Knot:

* Module resolver
* Project file discovery (indexed)
* Extension (e.g. when opening a file from outside the project but we still want to provide intelli sense and semantic syntax highlighting)


There are probably more, but these should be the most important one.


---

_Merged by @MichaReiser on 2025-03-03 12:59_

---

_Closed by @MichaReiser on 2025-03-03 12:59_

---

_Branch deleted on 2025-03-03 12:59_

---

_@MichaReiser reviewed on 2025-03-03 13:10_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:75 on 2025-03-03 13:10_

Hmm, looking into this it's hard to draw a hard line here. The files to check seem *clearly* to be session-specific. But so does setting, because changing the settings on the CLI changes the understanding of the projects (you can include or exclude files, enable and disable rules). This makes the split less clear and, to some degree, already exists today with the distinction of `Project` (stateful) and `ProjectMetadata` (the *what*). 

That makes me inclined to leave it as is. We could consider renaming `Project` to `Session`, but I like an *active* project object.

This doesn't mean we can't improve the terminology but it hasn't clicked to me yet what the terminology would be.

---
