```yaml
number: 14308
title: Workspace discovery
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/workspace-discovery
created_at: 2024-11-13T08:21:46Z
updated_at: 2024-11-15T18:20:17Z
url: https://github.com/astral-sh/ruff/pull/14308
synced_at: 2026-01-12T15:55:47Z
```

# Workspace discovery

---

_@MichaReiser_

## Summary

Disocver workspaces and packages by searching for `pyproject.toml`. This PR loads the pyproject.toml files but it disregards all settings other than `knot.workspace` and `project.name`. I plan to add settings support in a separate PR (this one is already chunky).

1. Traverse upwards in the `path`'s ancestor chain and find the first `pyproject.toml`.
1. If the `pyproject.toml` contains no `knot.workspace` table, then keep traversing the `path`'s ancestor
   chain until we find one or reach the root.
1. If we've found a workspace, then resolve the workspace's members and assert that the closest
    package (the first found package without a `knot.workspace` table is a member. If not, create
    a single package workspace for the closest package.
1. If there's no `pyproject.toml` with a `knot.workspace` table, then create a single-package workspace.
1. If no ancestor directory contains any `pyproject.toml`, create an ad-hoc workspace for `path`
    that consists of a single package and uses the default settings.

## Differences to uv

I tried to keep the implementation close to uv's behavior. The main differences are:

* Running Red Knot in a directory without any `pyproject.toml` (nor in any of its ancestor) is a valid use case. Red knot then creates a single package workspace with the default settings.
* Red knot allows `pyproject.toml` without a `project` table.
* uv errors when the current workspace is included in any outer workspace. However, I think it won't complain if the current workspace contains a member that is a workspace itself. This seems the wrong way around to me because what's important is that the current workspace has no nested workspace. Outer workspaces can be validated when running a command in them. This is why Red Knot only validates the workspace's members instead of discovering parent directories (https://github.com/astral-sh/uv/blob/e47e8fe9656b9f23891f9651f7bbc881f9e2bc7f/crates/uv-workspace/src/workspace.rs#L1193)
* ~~Red knot includes a member if it is explicitly listed in members but matches a `exclude` glob. E.g. `members=["packages/*", "frontend"]` and `exclude=["frontend-*]` still includes `frontend` if the directory is an exact match (not a glob-match). I think uv always excludes `frontend` which seems counterintuitive~~
* The member discovery in Red Knot collects all member paths by running all `members` globs and then removing excluded members. uv does that too, but not for the "current package" where it uses `is_excluded_from_workspace` and `is_included_in_workspace`. I found using only globs easier. (https://github.com/astral-sh/uv/blob/e47e8fe9656b9f23891f9651f7bbc881f9e2bc7f/crates/uv-workspace/src/workspace.rs#L1133-L1147)

CC: @charliermarsh 

## Test Plan

Added unit tests and basic file watcher tests. We should add more file watcher tests but I struggled coming up with good examples because most test cases require some form of configuration support to make the change observable.




---

_Label `red-knot` added by @MichaReiser on 2024-11-13 09:45_

---

_Comment by @github-actions[bot] on 2024-11-13 10:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-11-13 12:24_

Oh damn you pre-commit. I'm just going to exclude that file over wasting time fighting pre-commit configuration.

---

_Marked ready for review by @MichaReiser on 2024-11-13 16:28_

---

_Review requested from @carljm by @MichaReiser on 2024-11-13 16:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-13 16:28_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-13 16:28_

---

_Comment by @charliermarsh on 2024-11-13 17:23_

I will review this today.

---

_@charliermarsh reviewed on 2024-11-13 17:29_

---

_Review comment by @charliermarsh on `crates/red_knot_workspace/src/workspace/pyproject/package_name.rs`:10 on 2024-11-13 17:29_

Can you not use uv's implementation of this?

---

_@charliermarsh reviewed on 2024-11-13 17:32_

---

_Review comment by @charliermarsh on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-11-13 17:32_

(Couldn't this be incredibly expensive? Doesn't it mean you're iterating over everything in `.git`, `node_modules`, etc.?)

---

_@charliermarsh reviewed on 2024-11-13 17:34_

---

_Review comment by @charliermarsh on `crates/red_knot_workspace/src/workspace/pyproject.rs`:96 on 2024-11-13 17:34_

I thought it was "exact match"?

---

_Comment by @charliermarsh on 2024-11-13 17:34_

Oh I'm not requested here -- that's fine actually. Some feedback on the summary:

> uv errors when the current workspace is included in any outer workspace. However, I think it won't complain if the current workspace contains a member that is a workspace itself. This seems the wrong way around to me because what's important is that the current workspace has no nested workspace. Outer workspaces can be validated when running a command in them. 

No strong opinion though it'd be nice to change this in uv if you decide to do something different here.

> Red knot includes a member if it is explicitly listed in members but matches a exclude glob. E.g. members=["packages/*", "frontend"] and exclude=["frontend-*] still includes frontend if the directory is an exact match (not a glob-match). I think uv always excludes frontend which seems counterintuitive

I would probably advise against doing this... It makes the logic more complex to implement, test, and document, and it deviates from uv without good reason? I would be surprised if this ever comes up. Even the example you gave isn't quite right given that the exclude contains a dash, if I'm understanding correctly ;)


---

_@MichaReiser reviewed on 2024-11-13 17:39_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/pyproject/package_name.rs`:10 on 2024-11-13 17:39_

I used it as a starting point and then changed it.

---

_@MichaReiser reviewed on 2024-11-13 17:40_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-11-13 17:40_

It does, but it's a memory file system that we use for testing and the playground... We can do a proper implementation when we run into cases where we have many files in those use cases.

---

_@charliermarsh reviewed on 2024-11-13 17:52_

---

_Review comment by @charliermarsh on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-11-13 17:52_

Got it!

---

_@charliermarsh reviewed on 2024-11-13 17:54_

---

_Review comment by @charliermarsh on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-11-13 17:54_

I see, and for the "real" implementation you're running each glob separately via `glob::glob`.

---

_@MichaReiser reviewed on 2024-11-13 18:05_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-11-13 18:05_

Yes

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/program.rs`:73 on 2024-11-13 20:15_

I'm not clear what our intended semantics are in cases where the location of `pyproject.toml` is not the Python search path root, for example if the search path root for Python code is inside a `src/` directory relative to `pyproject.toml` (I think this is a common setup.) Should `workspace_root` in this case be the directory containing `pyproject.toml`, or the `src/` subdirectory?

It seems to me that the "workspace root" should probably be the directory containing `pyproject.toml`, but that path doesn't really belong in `SearchPathSettings`, it belongs in a project/workspace config struct. `SearchPathSettings` should IMO only contain the path(s) where we actually find Python imports relative to (which in the above example would be the `src/` subdirectory.) So renaming a `SearchPathSettings` member to `workspace_root` seems confusing to me, and probably not the right direction? (I realize the comment already described this as "the root of the workspace".)

A related question is how workspace members' code is expected to get onto the import search path. Do we offer some red-knot-specific functionality to make this happen automatically, or do we require that either workspace members are editable-installed into the venv (and thus we find them via `.pth` files) or else you have to use `extra_paths` explicitly for them?

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:72 on 2024-11-13 20:36_

Can you run red-knot on a single file within a package (if you want to see errors only from that one file, but you want imports from other modules to work)? If so, is it the caller's responsibility to call this function with just the directory portion of the file name?

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:172 on 2024-11-13 21:03_

Should we be looking for `__init__.py` and walking up until we don't find one here, so that virtual packages can work (and imports within them can work) even if you give a path inside the package? I think both pyright and mypy have some version of this logic. It doesn't work with namespace packages, but I'm not sure there's anything that can be done there -- at that point we can't really figure out where the root is supposed to be, and you need explicit config.

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:173 on 2024-11-13 21:03_

I'm confused by this TODO comment; isn't this the case where there is no `pyproject.toml`?

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:215 on 2024-11-13 21:10_

Is there any reason this needs to be an error for us? It seems odd to me that we'll happily synthesize a workspace with the name `<virtual>` if you don't have a `pyproject.toml` at all, but if you do have a `pyproject.toml` we'll error if it doesn't supply a name.

---

_Review comment by @carljm on `_typos.toml`:7 on 2024-11-13 21:11_

What are the typos it complains about?

I don't have any particular affection for the typo linter, but I also don't love adding random files to permanent exclude lists without a clear rationale.

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:62 on 2024-11-13 21:11_

```suggestion
    /// 1. If there's no `pyproject.toml` with a `knot.workspace` table, then create a single-package workspace.
```

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:462 on 2024-11-13 21:32_

In the project structure in this test, the workspace root should not be a search path at all for the module resolver; only the `src/` subdirectory should be. Because Python imports in this structure won't look like `import src.foo`, they'll look like `import foo`. It's not clear to me the intention in this PR of how to handle that.

Maybe we expect the `src/` subdirectory to get onto the search path via editable install of this project into the venv, but in that case we really don't want to add the workspace root to the search path, because then both `/app` and `/app/src` would be on the search path and we'd wrongly support _both_ `import src.foo` and `import foo` as different ways to import the same module, and this is really confusing; those imports won't both work at runtime, and if they do that's even worse, because you end up with two separate copies of the `foo` module and all its contents. Overlapping search paths are bad.

If the intention is that `SearchPathSettings::workspace_root` should not itself go onto the `SearchPaths::static_paths` at all, but instead should just be used to categorize other search paths discovered (e.g. from editable-install pth files in the venv) into first-party vs third-party (depending whether they are within the workspace root), this could work, but it would mean that we need to change the code in `SearchPaths::from_settings` so that it doesn't add `workspace_root` to `static_paths`.

In this case my concern would be that I think some projects _don't_ install their main first-party codebase into their venv as an editable install, and we need to figure out how to support that case (and how to tell the difference.) I think other type checkers don't rely on editable install of the main first-party codebase.

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:436 on 2024-11-13 21:37_

I find it somewhat confusing to use the `src/` naming convention in this test, because I think it strongly suggests that this case simply represents user error.

The specific name `src/` suggests "my top-level Python package(s) live inside `app/src/`, and that should be the search path root", not "my top-level Python package is named `src`, and `app/` should be the search path root". But our handling here assumes the latter.

If someone has a Python codebase that isn't packaged with a `pyproject.toml`, it's unlikely they put their top-level Python modules inside a `src/` subdirectory; they probably just put them directly in `/app`. (The reason this is likely is that if they don't have packaging metadata, they can't easily editable-install their project into the venv, and so they are probably relying on Python's implicit "CWD is on `sys.path`" behavior.) I've seen lots of real-world apps without packaging metadata that worked this way.

If they don't have packaging metadata, they should run red-knot directly on their Python source tree, not on a parent directory of it, because we don't have a good way to figure out which subdirectory is the Python import root (unless we special-case `src/` and assume that a subdirectory with that name is where the Python code lives.)

If they do run red-knot on `/app` when their Python modules live in `app/src`, and we have no packaging metadata to tell us that the `src/` subdirectory is the real import root, I guess we have to just assume that `/app` is the import root (and their top-level Python package is actually named `src`), since there's no config to tell us otherwise. But given the specific subdirectory name `src`, this is probably the wrong conclusion; most likely all their imports will fail to resolve because they `import foo`, they don't `import src.foo`. We could consider some heuristics, e.g. treat the subdirectory name `src/` specially, or look for whether or not there is a `src/__init__.py`.

This is also related (in the opposite direction) to whether if they run red-knot on `/app/src/foo/bar/` and `/app/src/foo/__init__.py` exists, should we walk up to the first directory that doesn't have `__init__.py` and determine that `app/src/` is the actual root, or should we just let their imports fail because we are assuming the wrong root. (I think in this case we should do the walk-up.)

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/settings.rs`:80 on 2024-11-13 22:26_

So the `src_root` from the `SearchPathConfiguration` represents some explicit red-knot config where the user tells us their source root, and this overrides setting the discovered workspace root as the workspace root in the `SearchPathSettings`?

---

_@carljm approved on 2024-11-13 22:28_

The workspace discovery looks great, thank you!

My main question about this PR (and most of my comments) are about how we intend to map these workspace settings to a module resolver search paths config (which is one of the most important outputs from workspace discovery.) I think currently in this PR there is a mismatch between workspace discovery and the module resolver's translation to search paths, which means we will do the wrong thing in a lot of cases, but I'm not sure if it's the module resolver or the workspace code that should change, because I don't understand what the intended semantics of `SearchPathSettings::workspace_root` are.

My review is mostly focused on these semantic and project structure questions; relying on Charlie's greater relevant experience from `uv` for the more filesystem and pyproject oriented stuff. 

---

_@MichaReiser reviewed on 2024-11-14 06:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/program.rs`:73 on 2024-11-14 06:56_

I did ask about this in Discord because I felt we discussed this before and it was unclear to me how it should work. @AlexWaygood then gave me a reply that seemed reasonable to me, see https://discord.com/channels/1039017663004942429/1228460843033821285/1304410214757433364

Generally, this PR doesn't change how we resolve the search path settings. It keeps doing what we used to do and I also don't mind reverting the rename of the `SearchPathSettings::workspace_root`. 

> A related question is how workspace members' code is expected to get onto the import search path. Do we offer some red-knot-specific functionality to make this happen automatically, or do we require that either workspace members are editable-installed into the venv (and thus we find them via .pth files) or else you have to use extra_paths explicitly for them?

I understand that we rely on the package manager to put projects on the path. 

---

_@MichaReiser reviewed on 2024-11-14 06:58_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:72 on 2024-11-14 06:58_

> Can you run red-knot on a single file within a package (if you want to see errors only from that one file, but you want imports from other modules to work)? 

Not today, long-term yes. But it doesn't fundamentally change how red knot works. It only changes which files it calls `check` for. 

In the file case, red knot searches the workspace, and then sets the `open_files` (or possibly `open_paths`, this is TBD) to the paths provided via the CLI and then calls `check`. 

It's important that the workspace discovery is done as if no path was provided to guarantee that red knot picks up the same settings.

---

_@MichaReiser reviewed on 2024-11-14 07:00_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:172 on 2024-11-14 07:00_

We could consider adding a feature like this if users ask for it in the future. Supporting it would complicate things a bit, because we then create a workspace that covers more files than `path`, that means that red knot needs to filter out all files that are outside `path` before calling `check`.

---

_@MichaReiser reviewed on 2024-11-14 07:05_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:215 on 2024-11-14 07:05_

I don't think there's a strong reason for it. The same as that it doesn't matter much to us whether packages have unique names inside a workspace. But I do see value in having the discovery consistent between uv and red-knot and `name` is a required setting according to the [`pyproject.toml` specification](https://packaging.python.org/en/latest/specifications/pyproject-toml/#declaring-project-metadata-the-project-table), unless the `project` table isn't specified at all. 

That's why I think the question rather is: Should the `project` table be required? 

---

_@MichaReiser reviewed on 2024-11-14 07:05_

---

_Review comment by @MichaReiser on `_typos.toml`:7 on 2024-11-14 07:05_

It doesn't like `FrIeNdLy-._.-bArD`. For good reasons, but it's intentional and changing the pattern to something else would make the test more complicated.

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:215 on 2024-11-14 08:57_

> Is there any reason this needs to be an error for us? It seems odd to me that we'll happily synthesize a workspace with the name <virtual> 

We also only use `virtual` in case red knot runs in `/`. It otherwise uses the directory name as fallback

---

_@MichaReiser reviewed on 2024-11-14 08:57_

---

_@MichaReiser reviewed on 2024-11-14 09:14_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:436 on 2024-11-14 09:14_

> This is also related (in the opposite direction) to whether if they run red-knot on /app/src/foo/bar/ and /app/src/foo/__init__.py exists, should we walk up to the first directory that doesn't have __init__.py and determine that app/src/ is the actual root, or should we just let their imports fail because we are assuming the wrong root. (I think in this case we should do the walk-up.)

This is an interesting example and I think it might make sense for us to walk up the tree, but I'm not sure if it should change the workspace-root or only the inferred `SearchPathSettings`. I'd like to exclude this for now but it's something I should bring up in the CLI design document

---

_@charliermarsh reviewed on 2024-11-14 14:00_

---

_Review comment by @charliermarsh on `crates/red_knot_workspace/src/workspace/metadata.rs`:215 on 2024-11-14 14:00_

I think it would be reasonable to allow this (projects without a `project` table). The `project` table isn't required in the spec, and recent PEPs (like the dependency groups PEP) have tried to make those more first-class. We don't fully support them in uv (we allow them as workspace roots, but it's considered legacy), but we arguably should in the future.

So, in short: I think it _would_ make more sense for red-knot to allow members / roots that lack a `[project]` table, but it's also reasonable to defer in favor of consistency with uv.

---

_@carljm reviewed on 2024-11-14 15:23_

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:436 on 2024-11-14 15:23_

Yes, I think it's totally fine to defer the walking-up thing, as long as we have it noted somehow as something we probably want to address in future.

In general I don't think my comments on this PR are blocking, which is why I accepted. I don't think this PR does anything wrong, it's more that the rename to `workspace_root` and the way the projects in the tests are structured highlight an existing issue that I think we need to come to a resolution on. (If the tests just didn't use a `src/` subdirectory layout, then I think they would be fully correct as-is, and we'd just be punting the problem of how we handle `src/` layouts for future resolution.)

---

_@MichaReiser reviewed on 2024-11-14 15:27_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:215 on 2024-11-14 15:27_

I'm leaning towards keeping it as is for now. We might actually need the name, depending on where we land on how workspaces integrate into module resolution (do we rely on the packager, in this case we won't need the name, or do we have our own, custom handling for packages, in which case we need the name)

---

_@carljm reviewed on 2024-11-14 15:53_

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:72 on 2024-11-14 15:53_

Makes sense, but I'm not sure if I fully understand this comment:

> It's important that the workspace discovery is done as if no path was provided to guarantee that red knot picks up the same settings.

Workspace discovery always starts with _some_ path that is provided by the user, right? So what does it mean to do workspace discovery "as if no path was provided"?

---

_@carljm reviewed on 2024-11-14 16:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/program.rs`:73 on 2024-11-14 16:00_

I agree with everything @AlexWaygood says in the first two paragraphs of that Discord comment. I'm not sure about this part:

> I think we should probably rename the first-party search path; calling it src is kind of confusing. It's not meant to correspond to the src directory inside a workspace; it's meant to correspond to the workspace root

In a project layout where the Python packages all go inside a `src/` subdirectory, at runtime the project must be installed to be importable, and usually it is run via some installed command/script, and `.` is not actually added to the import search path in that case. Which is a good thing, because if it were, you'd end up with overlapping search paths and all your modules importable as both `foo` and `src.foo`, which is not good, especially at runtime.

It is not clear to me how we can detect this project structure and avoid putting the workspace root on the search path in this case. I guess one option is just to punt on it and hope nobody is affected by the fact that `src.foo` imports are wrongly allowed.

> I understand that we rely on the package manager to put projects on the path.

👍 

---

_@carljm reviewed on 2024-11-14 16:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/program.rs`:73 on 2024-11-14 16:08_

I guess I'm also not clear that it's right to have only one "first-party" search path. I guess it partly depends what we mean by "first-party", but to the extent that it influences things like "which code do we show diagnostics for", I think it is important that all code within the workspace root (even if it is inside a `src/` subdir or a workspace member, and thus discovered via editable-install `.pth` files in the venv) should be considered as "first-party".

---

_@AlexWaygood reviewed on 2024-11-14 16:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:73 on 2024-11-14 16:11_

> I guess I'm also not clear that it's right to have only one "first-party" search path. I guess it partly depends what we mean by "first-party", but to the extent that it influences things like "which code do we show diagnostics for", I think it is important that all code within the workspace root (even if it is discovered via editable-install `.pth` files in the venv) should be considered as "first-party".

I suppose that depends what we're using the "first-party search path" for. If we're using it as the canonical definition for what constitutes "first-party" code, then what you say makes sense. But _do_ we need to tie those two concepts together? (I guess it would simplify some things?)

---

_@carljm reviewed on 2024-11-14 16:14_

---

_Review comment by @carljm on `crates/red_knot_workspace/src/workspace/metadata.rs`:215 on 2024-11-14 16:14_

> I'm leaning towards keeping it as is for now. We might actually need the name, depending on where we land on how workspaces integrate into module resolution (do we rely on the packager, in this case we won't need the name, or do we have our own, custom handling for packages, in which case we need the name)

I don't understand how these two things are related at all, or why we would ever need a project name for module resolution. The project name is just PyPI metadata that's unrelated to the import system or package names.

---

_@carljm reviewed on 2024-11-14 16:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/program.rs`:73 on 2024-11-14 16:15_

There are obviously lots of possible ways to structure this that could work, but I think it's pretty confusing to have a "first-party search path" and then a concept of "first-party code" which is not related to the first-party search path. It overloads the term "first-party" and suggests that one of those things should be renamed.

---

_@AlexWaygood reviewed on 2024-11-14 16:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:73 on 2024-11-14 16:17_

Agreed. If we're going to go with the "there should only ever be one of this kind of search path", I'd rename it everywhere to `workspace_root` or just `root`, rather than `first_party`.

---

_@MichaReiser reviewed on 2024-11-14 16:55_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:215 on 2024-11-14 16:55_

We talked this through in our 1:1 and this was a misunderstanding on my end of how python does module resolution. 

---

_@MichaReiser reviewed on 2024-11-15 17:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/program.rs`:73 on 2024-11-15 17:31_

I don't have a strong opinion. I'm leaning towards removing the field and pushing the workspace root path as the first `extra_path`. But I won't do this as part of this PR. For this PR, I'll undo the rename because the changes are entirely unrelated.

---

_@MichaReiser reviewed on 2024-11-15 17:39_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/settings.rs`:80 on 2024-11-15 17:39_

We should change the behavior as we discussed it in our 1:1 but it's out of scope for this PR

---

_@MichaReiser reviewed on 2024-11-15 17:42_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:72 on 2024-11-15 17:42_

My CLI proposal goes into that in more detail and I think it's best if we discuss it there. The design question is if red knot should always discover the workspace closest to the current working directory or if it should e.g. consider the provided paths. The current behavior is to only consider the current working directory. I'll change the behavior in a follow up PR if we decide that red knot should take the provided path arguments into account.

---

_@MichaReiser reviewed on 2024-11-15 17:46_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:215 on 2024-11-15 17:46_

I changed it to make the project table and name optional

---

_@MichaReiser reviewed on 2024-11-15 17:55_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:462 on 2024-11-15 17:55_

I moved the `src` into the `app` directory for now. I'll address the module resolution path configuration in a follow up PR

---

_@MichaReiser reviewed on 2024-11-15 17:55_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/metadata.rs`:436 on 2024-11-15 17:55_

I went ahead and deleted all `src` usages 😆 Now there's no files left other than `pyproject.toml`. So all of them are in the right place ;)

---

_Merged by @MichaReiser on 2024-11-15 18:20_

---

_Closed by @MichaReiser on 2024-11-15 18:20_

---

_Branch deleted on 2024-11-15 18:20_

---
