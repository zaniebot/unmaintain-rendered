```yaml
number: 12590
title: Add tests for hard and soft links
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: watcher-link-tests
created_at: 2024-07-31T07:48:46Z
updated_at: 2024-08-02T14:53:59Z
url: https://github.com/astral-sh/ruff/pull/12590
synced_at: 2026-01-12T15:55:41Z
```

# Add tests for hard and soft links

---

_@MichaReiser_

# Summary

The goal I set out for was to add support for symlinks and hard links when watching files. I no longer think we should pursue this goal, at least not now. This PR has mainly become a documentation PR and it adds a few tests demonstrating the behavior on different systems/platforms. 

Why not support watching symlinks and hardlinks? Primarily, because I don't think there's sufficient use to justify the complexity. I tested both VS Code and Pycharm and both show stale results after making changes to a hard-linked file or updating a symlinked file. This is as good as Red Knot is today.  Specifically, the following changes are not supported:

* Changing a symlinked file only updates the symlink target or source, but not both
* Changing a hardlinked file only updates the specific file to which the change was written. Creating or removing hard links are regular file events and they are supported on all platforms (this is important because uv uses hardlinks)

The other reason is that the platform specific file watcher APIs don't generate the necessary events. I compared the events with `@parcel/watcher` (used by VS Code, and parcel), and `fs.watch` (nodejs) and the generated events match. The only system that I found that supported hard links well is Pyright which is based on [`chokidar`](https://github.com/paulmillr/chokidar). I'm not a 100% sure how chokidar makes it work but I belive it does a directory traversal to find changes rather than relying on the events only. We could do that as well, but it seems rather expensive to support a not so common workflow. 

There are ways how we could support symlinks and hardlinks without traversing the directory. 

* We could keep a map from `Handle` to `Vec<File>` to lookup all files pointing to the same hard link. There's some complexity how we would support this with our `System` abstraction
* We could keep a reverse map from `real_path` to `Vec<File>` for all symlinked files. 

I think any approach (or traversing the directory to find changes) is worth exploring if this is something that users run into often. 

It is probably also worth exploring [watchman](https://facebook.github.io/watchman/) as a possible `notify` backend to support very large projects. 

Side note: Ruff doesn't follow symlinks today. Pyright has support for it. 

# Test Plan

This PR is only tests 😆 


---

_Comment by @github-actions[bot] on 2024-07-31 10:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-07-31 12:46_

---

_Marked ready for review by @MichaReiser on 2024-07-31 13:48_

---

_Review requested from @carljm by @MichaReiser on 2024-07-31 13:48_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-31 13:48_

---

_@MichaReiser reviewed on 2024-07-31 13:49_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1154 on 2024-07-31 13:49_

This fixes a few unused warnings on windows

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:52 on 2024-07-31 13:50_

I hope this will help to improve the stability of the tests. We can now use a high timeout for functions that expect at least a single event and a smaller timeout for functions that don't.

---

_@MichaReiser reviewed on 2024-07-31 13:50_

---

_@MichaReiser reviewed on 2024-07-31 13:50_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:265 on 2024-07-31 13:50_

Let's hope this fixes the other instability

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:26 on 2024-07-31 14:14_

I assume this is stored on the `TestCase` instance, even though it's unused, because the temporary directory will be immediately cleaned up if there are no remaining references to the `TempDir` instance, and we need the temporary directory to exist for the duration of the test that's using the `TestCase` instance? Could you maybe add a comment to that effect?

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:51 on 2024-07-31 14:19_

This seems to assume that `self.watcher` will never be `None` when this method is called. It's not immediately obvious to me what the invariant is here, because it's not immediately obvious to me why the `watcher` field is an `Option` in the first place. With this PR branch, do we ever create `TestCase` instances currently when `watcher` is set to `None`?

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:788 on 2024-07-31 14:23_

```suggestion
/// For reference: VS Code and PyCharm have the same behavior where the results for one of the
```

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:1093 on 2024-07-31 14:32_

`extra-paths`? It looks like the `extra_paths` search-path setting has been set to an empty `vec![]` immediately above?

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:1132 on 2024-07-31 14:33_

```suggestion
        // * PyCharm doesn't update diagnostics if a symlinked module is changed (same as red knot).
```

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:1124 on 2024-07-31 14:33_

```suggestion
        // The best we can assert here is that one of the files should have been updated.
```

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:1128 on 2024-07-31 14:33_

```suggestion
        // only `chokidar` seems to support it (used by Pyright).
```

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:1142 on 2024-07-31 14:34_

This is a very long line :P

```suggestion
        assert!(
            original_updated || symlinked_updated,
            "Expected one of the files to be updated but neither file was updated.\nOriginal: {original_text}\nSymlinked: {symlinked_text}",
            original_text = original_text.as_str(),
            symlinked_text = symlinked_text.as_str(),
        );
```

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:1160 on 2024-07-31 14:35_

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:1236 on 2024-07-31 14:36_

```suggestion
        // Pyright does support it, thanks to chokidar.
```

---

_@AlexWaygood reviewed on 2024-07-31 14:36_

Some of this I've only skimmed, but it LGTM overall. I have a few questions and a few nits:

---

_@MichaReiser reviewed on 2024-07-31 14:48_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:788 on 2024-07-31 14:48_

Changing it to Pycharm triggers clippy :laughing: 

---

_Review comment by @carljm on `crates/red_knot/tests/file_watching.rs`:954 on 2024-07-31 17:33_

It doesn't look like there is any `foo.py` in this test.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot/tests/file_watching.rs`:1047 on 2024-07-31 17:38_

Given the description of the scenario above ("workspace contains a symlink to another directory inside the workspace"), I'm not clear why site-packages should be involved in this test at all. That described scenario can be constructed without even having a site-packages.

We probably do need some tests around having site-packages inside vs outside the workspace, since both will be common setups that we need to support, and it does pose some potentially-tricky questions around properly distinguishing third-party from first-party code. (FWIW, and off-topic for this PR, my feeling is that when site-packages is inside the workspace, we should basically pretend that it isn't, and treat it the same as if it were outside. Putting your venv in your workspace doesn't mean that site-packages is now first-party code.)

But having site-packages contain symlinks to first-party code is an extremely strange scenario that I doubt we ever encounter, and it just feels odd to include that complexity in this test, when that's not part of the documented intention of the test.

---

_@carljm approved on 2024-07-31 17:43_

---

_@MichaReiser reviewed on 2024-07-31 20:15_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:51 on 2024-07-31 20:15_

I'll add a comment. Althought his isn't new code. 

The reason why it is an `Option` is because `watcher.stop` consumes the watcher and I didn't want to make the method take a `self` because it would consume the `TestCase` (which deletes the tempdir). 

The invariant here is that you can only call `stop_watch` once. Calling it a second time panics because the watcher isn't running anymore.

---

_@AlexWaygood reviewed on 2024-07-31 22:32_

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:51 on 2024-07-31 22:32_

> Althought his isn't new code.

Well, the `.unwrap()` call you're introducing in this PR is new. Previously you used `if let Some(watcher) = self.watcher.take()`, which implied it was okay to call `stop_watch()` several times — it just wouldn't have have done anything the second time, I suppose.

> The invariant here is that you can only call `stop_watch` once. Calling it a second time panics because the watcher isn't running anymore.

Great, thanks. Maybe we could use `.expect("Cannot call stop_watch() more than once!")` instead of `.unwrap()`?

---

_@dhruvmanila reviewed on 2024-08-01 06:04_

---

_Review comment by @dhruvmanila on `crates/red_knot/tests/file_watching.rs`:788 on 2024-08-01 06:04_

I guess we could add "PyCharm" in https://github.com/astral-sh/ruff/blob/a3e67abf4ce7b519152c03ff441424c18e4c18ee/clippy.toml#L1-L13

---

_Merged by @MichaReiser on 2024-08-02 10:14_

---

_Closed by @MichaReiser on 2024-08-02 10:14_

---

_Branch deleted on 2024-08-02 10:14_

---
