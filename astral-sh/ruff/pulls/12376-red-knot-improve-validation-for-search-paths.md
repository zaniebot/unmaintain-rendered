```yaml
number: 12376
title: "[red-knot] Improve validation for search paths"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: search-path-consistent-type
created_at: 2024-07-18T11:57:50Z
updated_at: 2024-08-02T16:11:53Z
url: https://github.com/astral-sh/ruff/pull/12376
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Improve validation for search paths

---

_Pull request opened by @AlexWaygood on 2024-07-18 11:57_

## Summary

This PR cleans up the settings validation and types in the module resolver. Namely:
- Currently we validate that a `ModuleResolutionPathBuf` has either no extension, a `.pyi` extension or a `.py` extension. But for search paths specifically (which are the only module-resolution paths that are ever directly constructed), that's the wrong kind of validation for us to do: we should just validate that the path points to a directory.
- For custom typeshed directories, validate that a `<typeshed>/stdlib/` directory exists, that a `<typeshed>/stdlib/VERSIONS` file exists, and that the `VERSIONS` file parses.

## Test Plan

`cargo test -p red-knot_module_resolver`. I haven't added any new tests specifically as part of this PR, but I'm happy to if you'd find them useful, either as this PR or as a followup.


---

_Label `red-knot` added by @AlexWaygood on 2024-07-18 11:57_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-18 11:57_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-18 11:57_

---

_Comment by @github-actions[bot] on 2024-07-18 12:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-07-18 12:37_

I think it would ease reviewing if the validation change and the structure change aren't in the same PR. Or is there a specific reason why you put both changes into one PR?

---

_@MichaReiser reviewed on 2024-07-18 12:37_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:114 on 2024-07-18 12:37_

What's the motivation for prefixing the query with `resolve`?

---

_@MichaReiser reviewed on 2024-07-18 12:38_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:770 on 2024-07-18 12:38_

All the `is_directory` reads will be a problem because they by-pass Salsa. 

---

_Comment by @AlexWaygood on 2024-07-18 12:40_

> I think it would ease reviewing if the validation change and the structure change aren't in the same PR. Or is there a specific reason why you put both changes into one PR?

I started off only doing the validation changes, but then I realised that really there were different invariants for module-search paths as opposed to module-resolution paths in general, and so improving the validation didn't really make sense while they were still being represented by one type. But I can certainly split it into two PRs.

---

_@AlexWaygood reviewed on 2024-07-18 12:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:114 on 2024-07-18 12:44_

This function now returns a `Result`. We now have another `module_resolution_settings(db: &dyn Db)` function that simply calls `resolve_module_resolution_settings(db).as_ref().unwrap()`. This still obviously isn't correct, because we're still panicking when we shouldn't, but:
- it's now isolated to one `.unwrap()` call and TODO comment
- we should get a nicer error message when the panic happens rather than just "called `.unwrap()` on a None value"

---

_@MichaReiser reviewed on 2024-07-18 12:55_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:114 on 2024-07-18 12:55_

I would call this method `try_module_resolution_settings` to make it clear that this returns a `Result`. 

We should make the `module_resolution_settings` a query because it panics anyway (nothing to cache)

---

_@AlexWaygood reviewed on 2024-07-18 13:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:770 on 2024-07-18 13:16_

I thought search-path settings were set once at the start of the process, and then never changed after that. Given that, is Salsa invalidation necessary here? If the settings never change, then the settings will only ever be validated once, so this function will only ever be executed once.

---

_Converted to draft by @AlexWaygood on 2024-07-18 13:49_

---

_Comment by @AlexWaygood on 2024-07-18 13:49_

I've extracted the structure change into its own PR: https://github.com/astral-sh/ruff/pull/12379.

---

_Renamed from "[red-knot] Use a distinct type for module search paths in the module resolver" to "[red-knot] Improve validation for search paths" by @AlexWaygood on 2024-07-23 10:57_

---

_Marked ready for review by @AlexWaygood on 2024-07-23 10:58_

---

_Comment by @AlexWaygood on 2024-07-23 11:00_

Following the changes made in https://github.com/astral-sh/ruff/commit/2a8f95c437d64fdd9b2d512ac065288d775aabfb (which this PR is now rebased on top of), this PR is now solely focussed on improving validation for search-path settings.

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-23 11:00_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:770 on 2024-07-23 22:34_

I think we need to watch these paths and if something we are assuming is a directory becomes not-a-directory, we need to react to that -- but I'd also assumed that this type of change (which should be rare) would mean totally starting over, which seems like it means Salsa invalidation wouldn't be relevant.

---

_@carljm reviewed on 2024-07-23 22:34_

This looks good to me, but will leave to Micha to stamp once the question of Salsa invalidation is resolved.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:770 on 2024-07-24 07:10_

Watching the directories wouldn't be sufficient. We would have to watch the parent directories because a change from file to directory is technically a different file (watching is file and not path-based). But that would significantly increase the number of files we watch, and some systems limit the number of files that can be watched. That's why I think that we don't want to do that.

There's also the problem that we can't watch the folders if resolving the module resolution failed. At least not without duplicating a lot of this logic in the code where  we set-up file watching. That's why I'm actually not sure if we want to error in the first place or if it would be better fallback to reasonable defaults. Because failing to start means that the LSP can support no-completion whatsoever. It also can't provide any help fixing the ruff settings. It's just dead. 

> I thought search-path settings were set once at the start of the process, and then never changed after that.

This is correct today but not long-term. A user can change the workspace settings at any time, and we have to respond to that. 



---

_@MichaReiser reviewed on 2024-07-24 07:13_

I like the validation code but I don't think the "all settings must be valid or we don't use any of the provided settings" provides the best user experience. It means the CLI will fail to start and so will the LSP. This severely limits the LSP because it can't provide any help when editing the settings. 

I think a better approach would be to recover as many user settings as possible. This gives us the *closest* to what the user wanted. We can surface the error messages with `tracing::warn` or `tracing::error` for now. 

---

_Comment by @AlexWaygood on 2024-07-24 10:52_

> I like the validation code but I don't think the "all settings must be valid or we don't use any of the provided settings" provides the best user experience. It means the CLI will fail to start and so will the LSP.

I think this is somewhere where we might want different behaviour depending on whether redknot is being run as a CLI tool or as an LSP server.

As an LSP server, I can see that it makes sense to try to recover as much as possible, as the user may be in the middle of editing their workspace settings -- and even if they're not, it's annoying if redknot completely gives up from linting all your code because of a small typo in your workspace settings.

As a CLI tool, I think the tradeoff is pretty different. Something I've really liked about Ruff in the past is how strictly it validates the settings passed to it. It means that there's no ambiguity, and no chance of Ruff silently doing the wrong thing. This is especially important, in my view, for a tool that applies autofixes to your code -- moreover, it's a tool that many people have installed as a pre-commit hook, which means it'll automatically apply autofixes as part of every commit.

---

_@AlexWaygood reviewed on 2024-07-24 10:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:770 on 2024-07-24 10:53_

> > I thought search-path settings were set once at the start of the process, and then never changed after that.
> 
> This is correct today but not long-term. A user can change the workspace settings at any time, and we have to respond to that.

I see -- this is different to what I had previously understood

---

_Comment by @MichaReiser on 2024-07-24 11:30_

I agree on that assessment. But that still means that the infrastructure has to handle it gracefully for the LSP case. The CLI will want to call some function that tells it if the settings are correct. 

---

_Comment by @MichaReiser on 2024-07-24 12:33_

I don't have error handling figured out entirely at this point, but I generally see two options, and I'm leaning towards option 1 for module resolution. But it's not entirely clear how we would implement it for module resolution specifically. 

## Handle possible fatal errors **outside** of salsa

What I mean by that, is that we avoid handling errors that are potentially fatal inside of Salsa queries and instead force the CLI/LSP to handle them when creating the Salsa inputs. Specifically to module resolution, this would mean that we validate the search paths **before** setting them on `Program` by exposing a method that has a `Result<Settings, (Settings, Errors)>`  return type where `Ok` means the settings are okay and `Err` returns the "recovered" settings with the encountered errors. The CLI and LSP can then decide how to recover from the error, e.g. by using the fallback setting, exit, or e.g. continue using any old search path settings from a previous pass.

This has two benefits:

* The validation would happen outside of Salsa. That means we don't have to worry about using any `system` APIs
* The salsa queries can assume that the input are valid. There's still a slight chance that someone deletes the directory between us checking the paths and running the analysis. But there's not much we can do about this. That's just bad luck (or bad intention)

I think I want to use a similar design for handling errors related to the configuration. 

The main challenge I see with this, specifically for module resolution, is that `Program` lives in `ruff_db`. I don't have a good solution for this just now.

## Gracefully handle the errors inside Salsa

This is the approach that I want to use for any non-fatal errors, e.g. we fail to read a single file. We extend `SourceText` with an `error: Option<Error>` field that indicates that reading the file failed. Regular queries that call `source_text` won't bother whether the error is set. They'll simply assume that everything worked just fine. However, `check_file` would explicitly call `source_text` and check if reading the file was successful. If not, abort checking and instead emit a diagnostic. 

We could do the same for module resolution but it does have the downside that *forgetting* to check if the operation failed is silent and, therefore, easy. 



---

_Comment by @AlexWaygood on 2024-07-24 13:05_

I agree there's lots of design work still to be done here regarding error handling and settings resolution. It wasn't really my intent to solve that problem in this PR; I just really wanted to make the validation we were already doing make some kind of sense, and to add some proper error messages for it rather than simply having the module resolver call `.unwrap()` on an `Option` value.

@MichaReiser, what's your specific concern with the changes this PR is making? It's already the case on `main` that we panic in the module resolver as soon as we find a single search path that we consider to be invalid; this PR doesn't change anything in that regard. Should I add some more TODOs around the system calls in `path.rs` saying that we'll need to revisit them if we end up deciding to do settings resolution inside Salsa?

I can think of lots of possible ideas for solving the bigger questions you're posing here, but I'm not sure any of them are in the scope of this PR.

---

_Comment by @MichaReiser on 2024-07-24 13:13_

My main concern is that it isn't a clear improvement to me because it introduces new unobserved `system` calls and isn't strictly moving toward the desired design. I'm not opposed to merging it. It's just that it kind of solves validation but kind of doesn't. 

---

_@MichaReiser approved on 2024-07-24 13:52_

But I don't think these are reasons enough to not land this PR

---

_Merged by @AlexWaygood on 2024-07-24 14:02_

---

_Closed by @AlexWaygood on 2024-07-24 14:02_

---

_Branch deleted on 2024-07-24 14:02_

---
