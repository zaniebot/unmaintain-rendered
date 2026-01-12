```yaml
number: 12307
title: "[red-knot] Add support for editable installs to the module resolver"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: editables
created_at: 2024-07-12T19:51:44Z
updated_at: 2024-07-16T18:27:06Z
url: https://github.com/astral-sh/ruff/pull/12307
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Add support for editable installs to the module resolver

---

_@AlexWaygood_

## Summary

This PR adds support for editable installations in `site-packages` to the module resolver. Still a few details to be checked vis-à-vis Python's runtime semantics.

Work towards https://github.com/astral-sh/ruff/issues/11653.

## Test Plan

`cargo test -p red_knot_module_resolver`

---

_Label `red-knot` added by @AlexWaygood on 2024-07-12 19:51_

---

_Comment by @github-actions[bot] on 2024-07-12 20:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-07-12 20:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:76 on 2024-07-12 20:24_

I considered just using the `SitePackages` variant for editable installs, since these search paths do come about as a consequence of `.pth` files in the `site-packages` directory. However, the search paths for editable installs aren't necessarily children of the `site-packages` search path (and indeed most often won't be), and I think it would be too easy to assume that all instances of the `SitePackages` variant have the `site-packages` directory as their root.

---

_@AlexWaygood reviewed on 2024-07-12 20:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:222 on 2024-07-12 20:25_

At runtime, Python does no validation for whether or not the path is a directory before adding it to `sys.path`, according to the docs: https://docs.python.org/3/library/site.html#module-site. Possibly we shouldn't either? I should check what mypy and pyright do here.

I'm not sure what happens if you have a path to a file, rather than a path to a directory, in `sys.path`? I've never seen that. I should check that too.

Python _does_ check whether the path actually _exists_ before adding it to `sys.path`.

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:208 on 2024-07-12 20:28_

This routine should probably be a Salsa query?

---

_@AlexWaygood reviewed on 2024-07-12 20:28_

---

_Comment by @MichaReiser on 2024-07-12 20:46_

Dude, stop working 😂 I thought you needed some rest

---

_Comment by @AlexWaygood on 2024-07-12 20:51_

> Dude, stop working 😂 I thought you needed some rest

I have an addiction :(

---

_@AlexWaygood reviewed on 2024-07-12 21:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:239 on 2024-07-12 21:14_

These paths aren't normalized and they could be paths relative to `site-packages` rather than absolute paths. We might want to normalize them.

Mind you, we don't normalize other search paths (to get rid of `.` and `..` components) anywhere yet either.

---

_@AlexWaygood reviewed on 2024-07-12 21:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:222 on 2024-07-12 21:28_

Yeah, if you append a file to `sys.path` at runtime it doesn't seem to have any impact on runtime semantics.

---

_@AlexWaygood reviewed on 2024-07-13 12:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:222 on 2024-07-13 12:55_

Also tried editing a `.pth` file in a `site-packages` directory to include a path to a Python file; it doesn't seem to affect which modules are imported or not. So I think it's fine for us to ignore any paths that don't point to directories

---

_Marked ready for review by @AlexWaygood on 2024-07-13 13:01_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-13 13:01_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-13 13:01_

---

_Comment by @codspeed-hq[bot] on 2024-07-13 14:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/editables)

### Merging #12307 will **improve performances by 4.81%**

<sub>Comparing <code>editables</code> (0d64399) with <code>editables</code> (e29d3d4)</sub>



### Summary

`⚡ 1` improvements
`✅ 32` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `editables` | `editables` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[incremental]` | 90 µs | 85.9 µs | +4.81% |


---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1516 on 2024-07-13 14:49_

This test is failing, and I'm not sure why. To read the contents of the `pth` files, I'm using `system_path_to_file` in `PthFileIterator::next()`, and `source_text()` in `PthFile::new()`. IIUC, I think that should be sufficient to indicate to Salsa that the `module_search_paths()` query depends on the existence and contents of the `_foo.pth` file, and that therefore deleting the `_foo.pth` file should invalidate the previous query.

---

_@AlexWaygood reviewed on 2024-07-13 14:49_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/testing.rs`:129 on 2024-07-15 07:54_

I don't understand what `other_files` means here. Can this be any file? Where are they written to? 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:177 on 2024-07-15 07:56_

Nit: We may want to fall this `first_party_root` because it isn't equal to the workspace root when the source is e.g. stored in a `src` directory

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:236 on 2024-07-15 07:58_

Nit: Can we use an `IndexSet` here. What I understand is that `OrderSet` is only a thin wrapper around `IndexSet` that implements `Hash`. I don't think we have a `Hash` requirement here.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:256 on 2024-07-15 08:00_

I recommend that we call `db.report_untracked_read();` in the body. It informs salsa to only cache this query for the current revision. This ensures that the results are correct even when we don't track the `system.is_directory` call yet (at a small perf penalty that this query is executed every time we run a new check phase but I think that's neglectable). 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:281 on 2024-07-15 08:04_

The `MemoryFileSystem` also doesn't guarantee this in its contract. It would be okay for it to shuffle the entries before returning. 

I recommend shortening the comment. The first two lines are what's important. 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:286 on 2024-07-15 08:04_

```suggestion
            dynamic_paths.extend(pth_file.editable_installations().filter_map(
```

The `iter` prefix isn't commonly used except for `iter` and `into_iter` methods

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:408 on 2024-07-15 08:11_

Can we store just the `File` instead? 

On the other hand, if we use `db.report_untracked_read`, then there's little benefit for storing `File` or even using `SourceText`. You can then simply store the `SystemPathBuf` and directly call `system.read_to_string`.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:475 on 2024-07-15 08:12_

I think Charlie's implementation also has an upper bound on the `pth` file sizes. Should we add this too?

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:232 on 2024-07-15 08:15_

Considering that we build this set (this is the validated settings). Can't we remove duplicates when constructing this struct. It seems wasteful to repeat the deduplication on every iterator call.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:347 on 2024-07-15 08:17_

Nit: I would make this a method on `ModuleResolutionSettings`, e.g. `.search_paths`. Feels easier to discover and comes with a nicer name (hiding the fact that it is an iterator or not)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:396 on 2024-07-15 08:20_

Is it adviced to trim the path?

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:395 on 2024-07-15 08:22_

Isn't the `site_packages` path guaranteed to be absolute? I would prefer guaranteeing that the `site_packages` path is absolute instead of relying on `system.current_directory` here.

---

_@MichaReiser reviewed on 2024-07-15 08:22_

Overall looks good. 

An unrelated comment for `resolver` (and some of them might be because how I structured the code). I find it difficult to read the code because I always have to jump up and down. The `ModuleResolutionSettings` aren't next to the `ValidatedSearchPathSettings`, instead, the `Pth` code becomes in between. We shouldn't refactor the code as part of this PR but maybe you can come up with an ordering that brings the code closer together.

---

_@AlexWaygood reviewed on 2024-07-15 09:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:232 on 2024-07-15 09:23_

Yes, I wondered about this. I think it makes it slightly less clear to the reader exactly what data is being stored where, but I agree it would be more efficient. I'll make the change.

---

_Comment by @AlexWaygood on 2024-07-15 11:01_

> An unrelated comment for `resolver` (and some of them might be because how I structured the code). I find it difficult to read the code because I always have to jump up and down. The `ModuleResolutionSettings` aren't next to the `ValidatedSearchPathSettings`, instead, the `Pth` code becomes in between. We shouldn't refactor the code as part of this PR but maybe you can come up with an ordering that brings the code closer together.

Yeah, I have the same opinion. I'm planning on doing a followup where I move some more code out into a separate submodule just for finding the search paths -- but although this PR makes the problem worse, I didn't want to do it in this PR, as it would have made the diff harder to read and review.

---

_@AlexWaygood reviewed on 2024-07-15 11:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:236 on 2024-07-15 11:03_

Oh, sure. I just copied what was being done elsewhere in `red-knot` in places where we needed deduplicated sequences that maintained insertion order 😆

---

_@AlexWaygood reviewed on 2024-07-15 11:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:475 on 2024-07-15 11:08_

I saw that. I believe it's copied directly from pyright's logic here: https://github.com/microsoft/pyright/blob/042d2ea58246f0e4a8e1597a540878badfae85b7/packages/pyright-internal/src/analyzer/pythonPathUtils.ts#L193-L206

I don't much like it: it seems pretty ad-hoc to me. There's nothing in the Python documentation that indicates that it will stop adding entries from `pth` files when they exceed a certain size. If you do end up with a massive `pth` file in `site-packages`, I don't think it's Ruff's fault if that creates a startup penalty for Ruff: it's probably the package the user installed, or the build backend that package is using, that's doing something wrong there.

---

_@AlexWaygood reviewed on 2024-07-15 11:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:396 on 2024-07-15 11:10_

Not sure I understand the question here, sorry!

---

_@AlexWaygood reviewed on 2024-07-15 11:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:395 on 2024-07-15 11:13_

The `site-packages` path is guaranteed to be absolute here, but without the call to `SystemPath::absolute()` here I think we could end up storing technically-valid-but-somewhat-strange paths as module resolution roots, e.g. `/site-packages/./../foo/bar/baz/src/`. I'd prefer to have that normalized as `/foo/bar/baz/src` before storing it as a module-resolution root (just for debuggability more than anything else).

---

_@AlexWaygood reviewed on 2024-07-15 11:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:395 on 2024-07-15 11:14_

This is because the paths we're trying to discover in `pth` files could be absolute paths, or they could be relative to `site-packages`

---

_@AlexWaygood reviewed on 2024-07-15 11:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/testing.rs`:129 on 2024-07-15 11:16_

They can be any files, yes. Editable installs can point to directories that are outside any of the "static" module-resolution root paths, so we need a way of modelling that in tests. The files are written to wherever the path points: `.with_other_file([("/foo/bar/src/baz.py", "")])` adds an empty file at location `/foo/bar/src/baz.py`.

I agree it's not a great name... any ideas for how to improve it? :P

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:177 on 2024-07-15 11:17_

Yeah, that's a great point

---

_@AlexWaygood reviewed on 2024-07-15 11:17_

---

_@AlexWaygood reviewed on 2024-07-15 11:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:177 on 2024-07-15 11:35_

I'll defer fixing this to a followup, since calling this `first_party_root` consistently would mean several files untouched by this PR would be altered, and it's a preexisting issue

---

_@MichaReiser reviewed on 2024-07-15 11:45_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:395 on 2024-07-15 11:45_

Makes sense. Can we then pass `site_packages` as the current working directory instead of `system.current_directory`?

---

_@MichaReiser reviewed on 2024-07-15 11:45_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:396 on 2024-07-15 11:45_

I wonder what happens if you have a line like `a/b                                                          ` (a lot of space). Should this resolve to `a/b` or throw because that directory probably doesn't exist on disk :D

---

_@MichaReiser reviewed on 2024-07-15 11:46_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:177 on 2024-07-15 11:46_

Sounds good

---

_@AlexWaygood reviewed on 2024-07-15 12:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:395 on 2024-07-15 12:01_

Ah, yeah that works!

---

_@AlexWaygood reviewed on 2024-07-15 12:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:396 on 2024-07-15 12:07_

Hehe. The Python spec doesn't seem to say anything about preceding and trailing whitespace on each line of a `.pth` file, and I don't know of any build backends that add preceding or trailing whitespace. Pyright trims preceding and trailing whitespace from each line, however (I assume because it wants to err on the side of leniency), and Charlie's port does too. It seems reasonable to me to trim trailing whitespace; I already do that in the `PthFile::editable_installations()` method

---

_@AlexWaygood reviewed on 2024-07-15 12:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:177 on 2024-07-15 12:10_

I filed https://github.com/astral-sh/ruff/issues/12337 so I don't forget

---

_@AlexWaygood reviewed on 2024-07-15 12:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:256 on 2024-07-15 12:18_

I don't fully understand what `report_untracked_read()` is doing here, but it fixes my tests, so I'll take it

---

_@MichaReiser reviewed on 2024-07-15 12:28_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1516 on 2024-07-15 12:28_

I took a quick look at the salsa events and this seems correct. 

```
[crates/red_knot_module_resolver/src/resolver.rs:1555:9] db.take_salsa_events().debug(&db) = [
    Event {
        runtime_id: RuntimeId {
            counter: 0,
        },
        kind: WillCheckCancellation,
    },
    Event {
        runtime_id: RuntimeId {
            counter: 0,
        },
        kind: WillCheckCancellation,
    },
    Event {
        runtime_id: RuntimeId {
            counter: 0,
        },
        kind: WillCheckCancellation,
    },
    Event {
        runtime_id: RuntimeId {
            counter: 0,
        },
        kind: DidValidateMemoizedValue {
            database_key: source_text(2),
        },
    },
    Event {
        runtime_id: RuntimeId {
            counter: 0,
        },
        kind: DidValidateMemoizedValue {
            database_key: parse_typeshed_versions(2),
        },
    },
    Event {
        runtime_id: RuntimeId {
            counter: 0,
        },
        kind: WillCheckCancellation,
    },
    Event {
        runtime_id: RuntimeId {
            counter: 0,
        },
        kind: WillExecute {
            database_key: dynamic_module_resolution_paths(0),
        },
    },
    Event {
        runtime_id: RuntimeId {
            counter: 0,
        },
        kind: DidValidateMemoizedValue {
            database_key: resolve_module_query(0),
        },
    },
]
```

It re-runs the `dynamic_module_resolution_paths` query because the dynamic module resolution paths changed (you deleted a `phf` file). It then re-runs the `resolve_module_query` but evaluates to the same cached result. Not sure why this is the case.

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1516 on 2024-07-15 12:34_

It seems that adding `db.report_untracked_read()` to the function body, like you suggested elsehwere, makes the test start passing. But then if I start using `db.system().read_to_string()` in the query to get the contents of the `.pth` file rather than `source_text()` (which you said _should_ be okay if I added the `report_untracked_read()` call), it starts failing again.

---

_@AlexWaygood reviewed on 2024-07-15 12:34_

---

_@MichaReiser reviewed on 2024-07-15 12:41_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/testing.rs`:129 on 2024-07-15 12:41_

I'm leaning towards calling `db.write_files(...)` in the few tests that need other files. 

---

_@AlexWaygood reviewed on 2024-07-15 12:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/testing.rs`:129 on 2024-07-15 12:42_

Fair enough

---

_@MichaReiser reviewed on 2024-07-15 12:43_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:256 on 2024-07-15 12:43_

It tells Salsa not to cache the result beyond a single revision (every time any input ingredient changes, the revision is bumped). 



---

_@AlexWaygood reviewed on 2024-07-15 12:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:408 on 2024-07-15 12:46_

> Can we store just the `File` instead?

Hmm, if I make this change (which I think is what you're suggesting):

```diff
--- a/crates/red_knot_module_resolver/src/resolver.rs
+++ b/crates/red_knot_module_resolver/src/resolver.rs
@@ -330,7 +330,7 @@ impl<'db> FusedIterator for SearchPathIterator<'db> {}
 struct PthFile<'db> {
     db: &'db dyn Db,
     path: SystemPathBuf,
-    contents: SourceText,
+    file: File,
     site_packages: &'db SystemPath,
 }
 
@@ -344,7 +344,7 @@ impl<'db> PthFile<'db> {
         Self {
             db,
             path: pth_path,
-            contents: source_text(db.upcast(), file),
+            file,
             site_packages,
         }
     }
@@ -355,11 +355,13 @@ impl<'db> PthFile<'db> {
     where
         'a: 'db,
     {
+        let contents = source_text(self.db.upcast(), self.file);
+
         // Empty lines or lines starting with '#' are ignored by the Python interpreter.
         // Lines that start with "import " or "import\t" do not represent editable installs at all;
         // instead, these are files that are executed by Python at startup.
         // https://docs.python.org/3/library/site.html#module-site
-        self.contents.lines().filter_map(|line| {
+        contents.lines().filter_map(|line| {
             let line = line.trim();
             if line.is_empty()
                 || line.starts_with('#')
```

Then I end up with compile errors:

```
error[E0515]: cannot return value referencing local variable `contents`
   --> crates/red_knot_module_resolver/src/resolver.rs:364:9
    |
364 |           contents.lines().filter_map(|line| {
    |           ^-------
    |           |
    |  _________`contents` is borrowed here
    | |
365 | |             let line = line.trim();
366 | |             if line.is_empty()
367 | |                 || line.starts_with('#')
...   |
374 | |             ModuleResolutionPathBuf::editable_installation_root(self.db, possible_editable_install)
375 | |         })
    | |__________^ returns a value referencing data owned by the current function
    |
    = help: use `.collect()` to allocate the iterator
```

Which I could fix easily enough by creating a custom iterator type that stores value returned by `source_text()`. But that doesn't seem much different to storing the value returned by `source_text()` directly on `PthFile` itself, which is what I have now.

> On the other hand, if we use `db.report_untracked_read`, then there's little benefit for storing `File` or even using `SourceText`. You can then simply store the `SystemPathBuf` and directly call `system.read_to_string`.

I tried this, but it makes `deleting_pth_file_on_which_module_resolution_depends_invalidates_cache()` start failing again (it was fixed by adding the `report_untracked_read()` call).

---

_@AlexWaygood reviewed on 2024-07-15 12:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/testing.rs`:129 on 2024-07-15 12:54_

Done in https://github.com/astral-sh/ruff/pull/12307/commits/1adf37d575a6d3571389a8fae3503f41060b5da7, though in my opinion it makes the test code somewhat uglier than it would be otherwise :(

---

_@AlexWaygood reviewed on 2024-07-15 13:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:408 on 2024-07-15 13:15_

I think I got a little bit closer to what you were recommending in https://github.com/astral-sh/ruff/pull/12307/commits/0c1e24c07dd0b742f8796243588f335be225a701

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-15 13:16_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1409 on 2024-07-15 13:35_

Nit: I think I prefer comparing the paths. It gives nicer error messages (1 != 2)
```suggestion
        assert_eq!(
            foo_module.file().path,
            FilePath::System(SystemPathBuf::from("/x/src/foo/__init__.py"))
        );
```

---

_@MichaReiser reviewed on 2024-07-15 13:35_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:76 on 2024-07-15 18:49_

I don't have a problem with introducing a new enum variant for pth-added paths, though I'm also not totally convinced by it: I don't know when we'd ever handle them differently, and it requires one more chunk of duplicated code wherever we handle this enum. I don't think the confusion you mention should occur: in general search paths should never be children of other search paths (this is a really bad setup that leads to the same module being importable under two different names), so it doesn't seem to me to be a likely confusion to assume this would be the case.

I'm not sure about the name `EditableInstall`. `pth` files are a feature of the Python import system that are _used_ by the editable-install feature of some package managers, but the feature predates that use, and `pth` files have been (and I'm sure still are) used in many other ways as well (e.g. for the zipped-egg installation format of setuptools). So I think it's potentially a little misleading to imply that every path discovered in a `.pth` file is an editable install.

I'd suggest `PthEntry` or something similar instead.

---

_@carljm reviewed on 2024-07-15 18:56_

---

_@carljm reviewed on 2024-07-15 19:00_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:222 on 2024-07-15 19:00_

Putting a file on `sys.path` is meaningful at runtime if there is an import loader that understands such an entry. For instance, the builtin zipfile import loader can load modules from zip files listed on `sys.path`. The homebrew-installed `python3` on my MacBook actually has the path `.../lib/python312.zip` on `sys.path` by default.

I don't think it's totally out of the question that we'd support zip loader someday (it wouldn't be technically all that hard, since we're already doing it for vendored files), but it seems pretty unlikely, since these days it's not much used AFAIK. So I think it's fine to just assume/require only directories on `sys.path` for now.

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:256 on 2024-07-15 19:03_

I think we should add a comment above the call to `db.report_untracked_read()` explaining what it does, why it's needed, and under what conditions we could remove it. (This should be a TODO comment if we intend to remove it at some point.)

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:353 on 2024-07-15 19:09_

```suggestion
        // instead, these are lines that are executed by Python at startup.
```

---

_@carljm reviewed on 2024-07-15 19:10_

---

_@carljm reviewed on 2024-07-15 19:17_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:396 on 2024-07-15 19:17_

Python at runtime only strips trailing whitespace, not leading whitespace: https://github.com/python/cpython/blob/main/Lib/site.py#L208

I also verified that behavior experimentally.

I think we should do the same thing the runtime does.

---

_@AlexWaygood reviewed on 2024-07-16 13:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:76 on 2024-07-16 13:04_

> I'm not sure about the name `EditableInstall`. `pth` files are a feature of the Python import system that are _used_ by the editable-install feature of some package managers, but the feature predates that use, and `pth` files have been (and I'm sure still are) used in many other ways as well (e.g. for the zipped-egg installation format of setuptools). So I think it's potentially a little misleading to imply that every path discovered in a `.pth` file is an editable install.

Hrm, true, but the validation we do elsewhere should ensure that we only add paths that point to directories when we iterate through `.pth` files to try to find paths that could potentially be editable installs. While it's true that a directoy path in a `.pth` file could point to something which wasn't intended to be an editable install, the Python runtime will still consider Python modules at that path to be importable, correct (since they'll be added to `sys.path` by `site.py`)? So IIUC a directory path in a `.pth` file is _in effect_ an editable install, even if it... wasn't meant to be.

All of which is to say, from the perspective of the module resolver, this variant should only really be used if we think it's more-likely-than-not that a path represents a directory that has been editably installed, or a path relative to a path representing a directory that has been editably installed. We should have done sufficient validation at this point that it's more than just "an arbitrary line in a `.pth` file".

---

_@AlexWaygood reviewed on 2024-07-16 14:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:170 on 2024-07-16 14:26_

I've realised that using an `OrderSet` here isn't sufficient to enforce that there'll be no duplication in the final list of search paths. `ModuleResolutionPathBuf::FirstParty(SystemPathBuf("/src"))` and `ModuleResolutionPathBuf::EditableInstall(SystemPathBuf("/src"))` will hash differently (and that's correct), due to them being different enum variants. But we shouldn't add the second one to the list of search paths for us to consider if the first one is already in the list, since they point to the same underlying path on disk :/

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:76 on 2024-07-16 14:40_

I'm okay with either name but it might be at the time that we add some documentation to each variant because I don't think the linked order still applies (it doesn't mention editable installs)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:170 on 2024-07-16 14:55_

You probably want to do a dedupe pass first OR collect all system paths into a set and test if they aren't already in the set before adding a new search path. What about `/src` and `/src/foo`. If this needs to be handled too, then you probably have to test the entire ancestor chain of every path before adding a new entry.

Should this be an `IndexMap`?



---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:241 on 2024-07-16 14:58_

Should this be an `IndexSet`?

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:354 on 2024-07-16 14:59_

Do we also need to remove duplicates from here? 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:408 on 2024-07-16 15:00_

What's the motivation for using `SourceText` as `contents` over just `String`?

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:343 on 2024-07-16 16:09_

Nit: I think you can just use the `db` lifetime
```suggestion
    fn editable_installations<'a>(&'db self) -> impl Iterator<Item = ModuleResolutionPathBuf> + 'db
    {
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:382 on 2024-07-16 16:10_

Nit: We can remove this comment now thanks to the `record_untracked_read` call.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:249 on 2024-07-16 16:11_

Nit: Should these method be called `editable_install_resolution_paths` to better align with the naming of the path variant? 

---

_@MichaReiser approved on 2024-07-16 16:13_

This looks good to me. Thanks for addressing all feedback. 

I think we should handle deduplication in a follow up PR. It's not specifically related to editable installs. It's just that editable installs make the problem worse. 

My only ask before merging is to change `PthFile` to use a `String` instead of `SourceText` and to use `IndexMap` over `OrderedSet`.

---

_@AlexWaygood reviewed on 2024-07-16 16:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:170 on 2024-07-16 16:21_

> What about `/src` and `/src/foo`

That's bad coding practice (as Carl mentioned elsewhere), and will probably end up with surprising results for the user, but the runtime adds them both to `sys.path` in that situation, and we should aim to emulate the runtime in that regard. Possibly we could emit some kind of warning telling the user that this probably will have bad effects down the line, but at the same time there might be a reason for doing this (quite unusual) thing, so that might also be annoying.

---

_@AlexWaygood reviewed on 2024-07-16 16:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:354 on 2024-07-16 16:24_

I don't think it's possible to have two duplicate `.pth` files (with duplicate filenames) in the same directory

---

_@AlexWaygood reviewed on 2024-07-16 16:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:249 on 2024-07-16 16:29_

I guess I wanted to have a clean distinction between "static" resolution paths that can be derived solely from the Ruff settings given to us by the user, and "dynamic" paths that we have to figure out for ourselves by iterating through `.pth` files. And I suppose there's a possibility that we might want to add support for some other kind of "dynamic" resolution paths in the future (not editables), which we might also want to implement as part of this routine.

But I'm okay with this proposed rename for now; that's pretty hypothetical, and I don't have any specific idea of what other dynamic search paths there could be.

---

_@AlexWaygood reviewed on 2024-07-16 16:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:241 on 2024-07-16 16:34_

No -- we need to know exactly what the site-packages path is later because we only lazily compute the editable-install search paths from the `.pth` files when and if we hit a module resolution that requires it. When we lazily compute the editable-install paths, we need to know the path to `site-packages` so that we can call `read_dir()` on the site-packages path and find all the `.pth` files in it.

---

_Comment by @AlexWaygood on 2024-07-16 16:36_

> to use `IndexMap` over `OrderedSet`.

I already changed the PR so that it uses `indexmap::IndexSet` internally rather than `ordermap::OrderSet`. But I think I have a confusingly named type alias in there. I'll change the name of that.

---

_@MichaReiser reviewed on 2024-07-16 16:43_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:241 on 2024-07-16 16:43_

Sorry. I meant line 240

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:354 on 2024-07-16 16:45_

What I meant was more whether we need to duplicate the paths to which the `pth` files point to, e.g. if you have one pointing to `ruff` and another to `ruff/api`. Or is this also considered bad practice?

---

_@MichaReiser reviewed on 2024-07-16 16:45_

---

_@AlexWaygood reviewed on 2024-07-16 16:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:241 on 2024-07-16 16:45_

`OrderSet` in this file is currently (following your previous review comments) just defined as

```rs
type OrderSet<T> = indexmap::IndexSet<T, BuildHasherDefault<FxHasher>>;
```

But that's a pretty darn confusing name for me to have given that type alias! So I'll change that.

---

_@MichaReiser reviewed on 2024-07-16 16:47_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:241 on 2024-07-16 16:47_

ah that explains it lol.

---

_@AlexWaygood reviewed on 2024-07-16 16:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:354 on 2024-07-16 16:48_

In the `for` loop immediately below this, we do indeed use a data structure that removes strict duplicates. But yeah, we shouldn't remove paths that overlap, only paths that are strict duplicates.

---

_@AlexWaygood reviewed on 2024-07-16 18:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:408 on 2024-07-16 18:10_

I switched it to use `String`.

---

_@AlexWaygood reviewed on 2024-07-16 18:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:256 on 2024-07-16 18:10_

Added a big TODO

---

_@AlexWaygood reviewed on 2024-07-16 18:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:76 on 2024-07-16 18:11_

Adding some more docs makes sense but I won't do it _just_ now, since I may still refactor this pretty soon according to the idea Carl suggested on one of my earlier PRs

---

_Merged by @AlexWaygood on 2024-07-16 18:17_

---

_Closed by @AlexWaygood on 2024-07-16 18:17_

---

_Branch deleted on 2024-07-16 18:17_

---

_@carljm reviewed on 2024-07-16 18:24_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:76 on 2024-07-16 18:24_

Certainly any path found in a `.pth` file will be added to `sys.path`, and particularly if that path is a directory, it will result in modules there being importable. I'm not sure if there is any other validation you're referring to that we do.

The logical jump you're making that I don't quite follow is "if we add a path to sys.path that makes things importable, that's an 'editable install'." I don't think that follows. It's just importable Python source code. "Editable install" is a very specific user-facing feature that implies a certain use case. For example, there's no reason the path added by the `pth` file line couldn't point to some Python code on a read-only filesystem. We should still support that just fine, but it's not very editable! In that case the pth file entry is being used for some other use case that I wouldn't describe as an "editable install."

---
