```yaml
number: 12318
title: "[red-knot] Add basic workspace support"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: workspace-support
created_at: 2024-07-14T09:47:01Z
updated_at: 2024-07-17T09:46:47Z
url: https://github.com/astral-sh/ruff/pull/12318
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Add basic workspace support

---

_Pull request opened by @MichaReiser on 2024-07-14 09:47_

## Summary

This PR adds support for checking an entire workspace rather than just individual files. But it does more than that. It lays the foundation for how Red Knot integrates settings long-term.

The CLI changes from `red_knot <files>` to something more cargo like where running `red_knot` defaults to checking the entire workspace in the current directory. Right now, any directory is a valid workspace and each workspace always has exactly one package. The long-term goal is to find the enclosing workspace by reading Ruff's configuration (one with a `[workspace]` section?) and then finding all packages specified in `[workspace.members]`. 


### New `Workspace` and `Package` ingredients

These ingredients represent the project's structure. They aren't used in any queries today, but I foresee the following usages:

* `package.check`: Checks all files in the package. We could make this a query to short-circuit the analysis. This should be especially useful in projects with many packages.
* `package.files` and `package.settings`: See below for the details on how we can compute the `files` and `settings` lazily. 

This PR also introduces `WorkspaceMetadata` and `PackageMetdata` struct. These are the equivalent of `Workspace` and `Package` but as non-Salsa ingredient. The key motivation for these two is that we need to resolve the `target-version` and the module-resolution settings to decide on the persistent cache to load. This requires access to the workspace's and package's settings, and that's what these two structs will expose in the future.


### Should `Workspace` be a singleton?

Possibly? I'm not sure. The idea is that a `Database` has exactly one `Workspace`. To me it is unclear if it may be necessary to create a second `Workspace` in response to file changes. E.g. what if the `workspace`'s `ruff.toml` gets deleted? Maybe we should just create a new `Database` in that case but that complicates things in the CLI's main loop. My current thinking is to create a new `Workspace` ingredient in that case (which would fail if it is a singleton) and swap the `Program`'s `Workspace`. This should also allow us to keep the existing `Workspace` if loading the new one, for whatever reason, fails.

### New `Program` ingredient

The new `Program` ingredient (singleton) mainly generalizes the existing module resolution settings and moves them into `ruff_db`. The motivation is that these settings aren't just important for module resolution. They're fundamental to the entire analysis. Two analyses using different program settings aren't compatible and should be cached (persistent) separately. 

@AlexWaygood we have to figure out how to solve our merge conflicts. 

I decided to rename the root `Database` from `Program` to `RootDatabase`, mainly because I found it counterintuitive that a program contains a `Workspace` and to avoid a naming conflict with the new `Program` ingredient.

### Should the `Database` support multiple open `Workspace`s?
My initial thinking was that it probably should, but I've concluded that it would complicate persistent caching by a lot. 

Ideally, Ruff uses a different persistent cache for different workspaces. Storing multiple `Workspace`s in a single `Database` creates the challenge that it becomes necessary to decide for every `Ingredient` into which persistent cache it should be stored. Loading the persistent cache would require merging the `Ingredient`s from different caches, which seems a headache. 

Having a single `Database` per `ProgramSettings` also simplifies caching runs using different target-versions individually: `ruff check --target-version 3.8` and `ruff check --target-version 3.10` should each use their own persistent cache. 

For now, I think it's best to have a 1:1 relationship between `Database` and `Workspace`. This doesn't mean Red Knot can't support multiple open `Workspace`s. It just means we must build this functionality around the `Database` instead of into the `Database`. 


### Checing individual files or folders
It remains possible to check a different folder using `red_knot—-current-directory path`. However, this PR removes the functionality for testing specific files or sub-folders. The challenge with explicit paths is that it isn't sufficient to detect the files in these paths once; instead, the paths must be monitored for newly added or removed files (and follow Ruff's exclude semantics). I'm not sure how to implement this best, but I think this is a CLI-specific problem and should be built on the added `open_files` feature.

### How do settings work?
This is about the formatter or linter settings, not the workspace or package settings. Ruff supports that each directory can have its own linter and formatter settings. That means it is necessary to resolve the settings on a directory level. 

The easiest solution is to define a `linter_settings(File)` query that returns the liner settings for a given file by searching the closest package and asking that package what its settings are. The main downside is that the query is more granular than what Ruff allows (it's a file-level query rather than a directory-level query). A possible solution is to create a new `Directory` ingredient (interned) and change the query to `linter_settings(Directory)`. While this query now has the correct granularity, it still means we're creating too many ingredients if a project has only a custom setting in exactly two directories. 

I think the best solution is to instead create a `Settings` input ingredient that has tracked `linter_settings`, `formatter_settings`, etc. methods. A package would create all the `Settings` the first time it is asked for a setting file (we can find all the settings while discovering all the package files). Resolving the linter settings for a file then simply becomes:
* Find the closest package
* Ask the package for the `Settings` (can use a router similar to Ruff today)
* Call `settings.linter_settings` (salsa query)

Dependent queries would only re-run when:
* A new package is added or an existing package is removed from a workspace
* New settings have been added or removed from this package
* The linter settings of this path have changed

That's pretty good. 

### Can `packages.files` and `packages.settings` be lazy?

In this PR, discovering the files of a package is done eagerly. I think it should be possible to make this lazily but i would prefer to do this in a separate PR. The overall idea is:

* Introduce a new `SourceRoot` input ingredient. A source root is a directory, but it's not a 1:1 mapping between `SourceRoot` and a directory on the disk. A source root has two fields: Its a path (id) and a `revision.` 
* We create a `SourceRoot` for the `Workspace` and every `Package` 
* The semantic of `revision` depends on the specific `SourceRoot`
  * For packages: `max`: 
    * the last modified time of any ignore file 
    * the last deletion time of any ignore file
    * the time when the last file was added 
  * For the workspace: Same as for the package but don't bump the revision if the file belongs to a package
  * For site packages: The last modified time of any root `ph` file (https://github.com/astral-sh/ruff/pull/12307)

Edit: I think we can simplify this by simple storing an incrementing counter on `Package` and `Workspace` rather then introducing a new ingredient.

### Follow up work

* File watching is a bit of a mess right now. I plan to tackle this next
* We don't do any error handling yet. We should look at this soon (by creating diagnostics).

## Test Plan

I ran `red_knot --current-directory ../test` and verified that the program is re-checked when adding, removing, or deleting files. 


---

_Label `red-knot` added by @MichaReiser on 2024-07-14 10:05_

---

_Renamed from "[red-knot] Add workspace support" to "[red-knot] Add basic workspace support" by @MichaReiser on 2024-07-14 13:25_

---

_Marked ready for review by @MichaReiser on 2024-07-14 13:34_

---

_Review requested from @carljm by @MichaReiser on 2024-07-14 13:34_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-14 13:34_

---

_Comment by @github-actions[bot] on 2024-07-14 13:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/lib.rs`:103 on 2024-07-14 13:58_

I thought the decision in https://github.com/astral-sh/ruff/issues/12250 was that the current behaviour here was correct

---

_@AlexWaygood reviewed on 2024-07-14 13:58_

---

_@MichaReiser reviewed on 2024-07-14 14:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/lib.rs`:103 on 2024-07-14 14:14_

I'm not changing the existing behavior. I just need an easy way for now to filter out files that we don't understand without relying on the configuration (which defaults to python files only)

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/lib.rs`:103 on 2024-07-14 14:15_

Oh I see, you've added an extra method. Sorry!

---

_@AlexWaygood reviewed on 2024-07-14 14:15_

---

_@MichaReiser reviewed on 2024-07-14 14:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/lib.rs`:103 on 2024-07-14 14:22_

No worries. Would have been bad if I did change the behavior 😂

---

_@AlexWaygood reviewed on 2024-07-14 15:26_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/program.rs`:11 on 2024-07-14 15:26_

I'd prefer for this field and struct to be called `search_path_settings: SearchPathSettings`, rather than `search_paths: SearchPaths`. The fields on the struct don't yet represent search paths that we have confidence are valid for module resolution; they just represent the raw settings inputs given to us by the user. The most obvious example of this is the fact that we have a `custom_typeshed: Option<SystemPathBuf>` field. When we're _actually_ resolving modules in the module resolver, we'll always have _a_ path that represents the typeshed stdlib -- but at this stage, it's unknown whether we'll be using a custom (a this point still unvalidated) path given to us by the user to a directory on disk, or `VendoredPath("stdlib")`.

---

_@MichaReiser reviewed on 2024-07-15 06:32_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/program.rs`:11 on 2024-07-15 06:32_

First, I don't mind renaming the struct. I just picked a name and probably not a very good one. 

> The fields on the struct don't yet represent search paths that we have confidence are valid for module resolution; they just represent the user's raw settings inputs. 

Can you tell me a bit more about what form of validation is necessary? 

I think we should store the validated configuration on `Program`. The value we store here is **after** reading and resolving the workspace settings. The fact that it is after resolving the setting is important for persistent caching, where we want configurations that resolve in the same settings (e.g., it shouldn't matter if a user explicitly defines a value that is equal to the default or uses the default value) to have equal cache keys. 



---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:74 on 2024-07-15 19:59_

```suggestion
        let canonicalized = cwd.as_utf8_path().canonicalize_utf8().unwrap();
```

---

_Review comment by @carljm on `crates/red_knot/src/workspace.rs`:52 on 2024-07-15 20:08_

Some things that I think would be useful to clarify here (I'm putting "package" in quotes because I don't think we should use this word, as discussed below, but for now I'm matching the terminology here):

1) Can "packages" be nested?
2) Must "packages" be defined by a `pyproject.toml`, or can they be defined by a `ruff.toml` also?
3) What is our default behavior if no configuration file is found at all?
4) Can this layout also "simplify" to the simplest case, where there is just one "package" and it doesn't use a `src/` subdir; the workspace root is also the first-party import search path root for the only "package" it contains, and `pyproject.toml` lives right there alongside the top-level Python modules/packages? This is still a reasonably common structure for Python applications, and I think we need to make sure we can support it.

---

_Review comment by @carljm on `crates/red_knot/src/workspace.rs`:85 on 2024-07-15 20:19_

I know this is a pain, but I would very strongly prefer that we _not_ use the word "Package" for this concept. The term "Package" is already overloaded with two distinct meanings in Python-land. The older (and IMO stronger) usage of the word is for a Python module that is represented by a directory, not a file, and can thus contain sub-modules. (I.e. a directory containing `__init__.py`, or a namespace package.) This meaning definitely does not match what we have here: in the example layout above, `app-1` is not a package; `app-1/src/app1/` (the actual importable directory containing an `__init__.py`) is a package (and so would `app-1/src/app1/sub` -- packages can be arbitrarily nested.)

The other colloquial meaning of "package" in the Python world is "a PyPI distribution" (or "a directory that contains a `pyproject.toml` with PyPI metadata in it"). I think this use of "package" should usually be avoided (due to confusion with the older above use), and instead "project" or "distribution" should be preferred.

Our meaning here is more like the second meaning, but I think also slightly different, because I suspect we won't require PyPI distribution metadata? So using "package" here is introducing another slightly different overloaded meaning for an already-overloaded term.

I think the term "project" would be a reasonable choice to replace "package" here, and it rhymes nicely with `pyproject.toml` as well, if that's our preferred way for people to supply our config.

---

_Review comment by @carljm on `crates/red_knot/src/workspace/metadata.rs`:32 on 2024-07-15 20:34_

Is this where we should have a TODO for some kind of default workspace heuristics? I think it's important that we don't just give up in the no-config scenario.

---

_Review comment by @carljm on `crates/red_knot/src/workspace/metadata.rs`:35 on 2024-07-15 20:35_

I don't want to require `pyproject.toml` metadata, so we should also have some kind of other fallback, like the directory name.

---

_Review comment by @carljm on `crates/ruff_db/src/program.rs`:7 on 2024-07-15 21:12_

It's not really clear to me why we should have both `Program` and `Workspace` as separate things. I realize that currently `Program` is a true singleton and `Workspace` is not, but the only reason for that is that we might want to hold onto the old Workspace when constructing a new one, in case we fail to build the new one, and it seems like this same rationale applies to `Program` just as much -- it is built from user settings that we might fail to parse. So what is the benefit of having `Program` and `Workspace` be separate, and what is the logic that defines what is owned by `Program` and what is owned by `Workspace`?

---

_@carljm reviewed on 2024-07-15 21:13_

The core abstractions here make a lot of sense to me! This is a great step forward in clarifying how everything will fit together.

---

_@MichaReiser reviewed on 2024-07-16 05:48_

---

_Review comment by @MichaReiser on `crates/red_knot/src/workspace.rs`:85 on 2024-07-16 05:48_

I used package because `uv` uses this term and I want the two interfaces to be identical, at least when it comes to the most basic concepts. 

I personally don't see this as blocking the PR and am happy to do a follow-up PR to rename the struct if we decide on any other name for workspace members in uv and red knot.

---

_@MichaReiser reviewed on 2024-07-16 05:51_

---

_Review comment by @MichaReiser on `crates/red_knot/src/workspace/metadata.rs`:32 on 2024-07-16 05:51_

My preference is to add TODOs only for things that are incorrect but subtle to catch, not for missing features. Otherwise, we end up with a trillion to-dos and will never fix half of them because we decide that we don't need these features after all. 

Proper workspace heuristics is something so obvious that it is unlikely to miss, so we don't need a todo. Missing error recovery is more subtle and not so easy to find, that's when I think a TODO comment is appropriate. 

---

_@MichaReiser reviewed on 2024-07-16 05:52_

---

_Review comment by @MichaReiser on `crates/red_knot/src/workspace/metadata.rs`:35 on 2024-07-16 05:52_

I agree; we can use the directory name. I can remove the TODO if you want but I intentionally don't implement the heuristic as part of this PR.

---

_@MichaReiser reviewed on 2024-07-16 06:00_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/program.rs`:7 on 2024-07-16 06:00_

I explained one motivation in the summary:

> The new Program ingredient (singleton) mainly generalizes the existing module resolution settings and moves them into ruff_db. The motivation is that these settings aren't just important for module resolution. They're fundamental to the entire analysis. Two analyses using different program settings aren't compatible and should be cached (persistent) separately.

That generally means that `Workspace` holds on to significantly more information than `Program`. It also solves a technical problem. Workspace must hold on to the `Configuration`/`Settings,` but the `Configuration` depends on the `LinterConfiguration` and `FormatterConfiguration` structs that are defined in `ruff_linter` and `ruff_python_formatter`. `ruff_db` can't depend on `ruff_linter`, because `ruff_linter` depends on `ruff_db`. The alternative is not to have a `Program` ingredient and instead keep per-crate specific inputs as we used to have with the module-resolver. But I prefer not to do that because input ingredients require manual creation (and updating).

What should be on `Program`? Just the values that define the *identity* of a `Program`. Another way to think about it is that two projects can't be "checked" together if they require a different value for any of the `Program` fields.


---

_@MichaReiser reviewed on 2024-07-16 06:06_

---

_Review comment by @MichaReiser on `crates/red_knot/src/workspace.rs`:52 on 2024-07-16 06:06_

I think yes, "packages" can be nested. 

I intentionally didn't specify how the heuristic works I think it's not important for setting up the relevant concepts. We have to think about how the configuration works and what happens in the absence of any configuration, but I recommend doing this later and we should mirror uv's behavior for best compatibility between all Astral tools. 

My current thinking is:

1. Yes packages, can be nested, workspaces can't (or they can, but ruff only picks up the closest workspace and ignores nested workspaces)
2. I don't have an opinion on this yet, but I don't see why it can't be a `ruff.toml`.
3. We should use Ruff's default configuration and assume the current folder is a workspace containing a single package/project.
4. It's unclear to me what the challenge with that is. Yes, this should be supported. The workspace members can be arbitrary paths in the project.

---

_@AlexWaygood reviewed on 2024-07-16 12:55_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/program.rs`:11 on 2024-07-16 12:55_

There's a lot of TODOs around validation in the module-resolver crate at the moment; it's on my immediate list of things to work on. There's a lot of unwrapping where we should be bubbling up errors ([example](https://github.com/astral-sh/ruff/blob/b1487b6b4fda8edc7c37261861b91c19ec2a6a64/crates/red_knot_module_resolver/src/resolver.rs#L138-L143), [example](https://github.com/astral-sh/ruff/blob/b1487b6b4fda8edc7c37261861b91c19ec2a6a64/crates/red_knot_module_resolver/src/typeshed/versions.rs#L62-L65)); there's some validation that we do that we probably don't need to do; and there's some validation that we don't yet do that we probably should. The validation that we do do is also all in the wrong place!

But the intent I had with the `RawModuleResolutionSettings` struct on `main` (which this struct seems to emulate in its fields and data types) is that it should represent the settings more-or-less directly as passed by the user, that have not yet been validated and transformed into data types that can be used directly by the module resolver:

https://github.com/astral-sh/ruff/blob/b1487b6b4fda8edc7c37261861b91c19ec2a6a64/crates/red_knot_module_resolver/src/resolver.rs#L112-L114

If you want to store the validated settings on `Program`, I think you should be storing a struct that's more similar to [`resolver::OrderedSearchPaths`](https://github.com/astral-sh/ruff/blob/b1487b6b4fda8edc7c37261861b91c19ec2a6a64/crates/red_knot_module_resolver/src/resolver.rs#L184) on `main` (though my open editable-installs PR gets rid of that struct and replaces it with a `ValidatedSearchPathSettings` struct).

> Can you tell me a bit more about what form of validation is necessary?

Here's validation that I think we should do eagerly w.r.t. the module-resolver settings. Again, some of this we half-do currently, and some of it we don't do at all currently:
- No path should be added as a module resolution search path unless we've verified it points to a directory
- If we're passed a custom typeshed directory, we should
  - verify that a `stdlib/` directory exists in that directory
  - verify that a `stdlib/VERSIONS` file exists
  - verify that the `stdlib/VERSIONS` file parses as we'd expect.
- The `typeshed` field on the struct that represents the resolved settings should be a `FilePath` object, rather than an `Option<SystemPathBuf>` object: if no custom typeshed directory is passed, we'll just use the vendored stubs for the standard library, which is `FilePath::vendored("/stdlib")`. Additionally, for module resolution we'll be using the path to the stdlib directory inside the typeshed directory as a search path, not the path to the typeshed directory itself. We should just store the path to the stdlib directory on `Program` directly rather than constructing it from the path to the typeshed directoy every time we resolve a module.
- We need to make sure that no two search paths point to the same directory on disk, emulating the behaviour at runtime where no two entries on `sys.path` ever point to the same directory.
- After all the verification's been done, all paths should end up as `ModuleResolutionPathBuf` instances rather than `SystemPathBuf`, `VendoredPathBuf` or `FilePath` instances

---

_@MichaReiser reviewed on 2024-07-16 13:28_

---

_Review comment by @MichaReiser on `crates/red_knot/src/workspace.rs`:52 on 2024-07-16 13:28_

How this works in uv: https://github.com/astral-sh/uv/blob/00c055a6bd8a93c980c4d32772b7fd55069de4fe/crates/uv-distribution/src/workspace.rs#L70-L160

---

_@carljm reviewed on 2024-07-16 22:04_

---

_Review comment by @carljm on `crates/red_knot/src/workspace.rs`:85 on 2024-07-16 22:04_

Ah, yeah, matching `uv` makes sense. And the term "package" is also pretty strongly embedded in the "packaging" space as well, so we may have trouble finding a better word there. I guess we'll just live with the overloaded term.

---

_@carljm approved on 2024-07-16 23:14_

---

_@MichaReiser reviewed on 2024-07-17 09:16_

---

_Review comment by @MichaReiser on `crates/red_knot/src/workspace.rs`:52 on 2024-07-17 09:16_

I expanded the comment on the assumptions that I think are important regardless of the heuristic to discover workspaces. 

* A workspace must always contain at least one package
* Packages can be nested
* Every file belongs to no or exactly one package.

---

_@MichaReiser reviewed on 2024-07-17 09:26_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/program.rs`:11 on 2024-07-17 09:26_

Thanks. I like the name `SearchPathSettings` and I think we can actually defer some of the validation to after creating the program. 

---

_Merged by @MichaReiser on 2024-07-17 09:34_

---

_Closed by @MichaReiser on 2024-07-17 09:34_

---

_Branch deleted on 2024-07-17 09:34_

---
