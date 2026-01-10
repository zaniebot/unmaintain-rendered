```yaml
number: 16021
title: "[red-knot] User-level configuration"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/user-level-configuration
created_at: 2025-02-07T14:34:49Z
updated_at: 2025-02-10T15:44:26Z
url: https://github.com/astral-sh/ruff/pull/16021
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] User-level configuration

---

_Pull request opened by @MichaReiser on 2025-02-07 14:34_

## Summary

This PR adds support for user-level configurations (`~/.config/knot/knot.toml`) to Red Knot. 

Red Knot will watch the user-level configuration file for changes but only if it exists
when the process start. It doesn't watch for new configurations, 
mainly to simplify things for now (it would require watching the entire `.config` directory because the `knot` subfolder might not exist either). 

The new `ConfigurationFile` struct seems a bit overkill for now but I plan to use it for
hierarchical configurations as well. 


Red Knot uses the same strategy as uv and Ruff by using the etcetera crate.  

## Test Plan

Added CLI and file watching test


---

_Review requested from @carljm by @MichaReiser on 2025-02-07 14:34_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-07 14:34_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-07 14:34_

---

_Label `red-knot` added by @MichaReiser on 2025-02-07 14:34_

---

_@MichaReiser reviewed on 2025-02-07 14:36_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:102 on 2025-02-07 14:36_

We may want nicer error messages here that use proper diagnostics but I decided to not tackle that for now and continue doing what we do for other errors at this stage.

---

_@MichaReiser reviewed on 2025-02-07 14:36_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/db/changes.rs`:51 on 2025-02-07 14:36_

Whoops

---

_@MichaReiser reviewed on 2025-02-07 14:37_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/db/changes.rs`:152 on 2025-02-07 14:37_

We may want to use proper diagnostics here so that LSP users get notified about this error but there are other errors in this file where we're using `tracing` -> A problem for another day

---

_@MichaReiser reviewed on 2025-02-07 14:38_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/db/changes.rs`:220 on 2025-02-07 14:38_

I got really confused when red knot started parsing my `knot.toml` file as a python file. For now, I did the simplest fix by copying this condition from `Project::discover_files`. I have to rework this code next month anyway to properly support file inclusion and exclusions.

---

_@MichaReiser reviewed on 2025-02-07 18:54_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/utils.rs`:6 on 2025-02-07 18:54_

This is actually not sufficient because cargo runs tests concurrently. So we also need some form of a lock to avoid that multiple tests hold on to the same env variable. Ugh, this is getting out of hand.

---

_Comment by @github-actions[bot] on 2025-02-08 16:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2025-02-10 08:05_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata.rs`:244 on 2025-02-10 08:05_

Haven't thought about this in detail, but in principle, it could even make sense to add configuration paths that were *not* existent when we started up, in order to pick them up if they are being created later on. Of course, this would probably be a bit more involved to implement, as we would have to watch for file-creation events in the containing directory etc.; so I'm not sure if it's worth it.

---

_@sharkdp reviewed on 2025-02-10 08:16_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/configuration_file.rs`:20 on 2025-02-10 08:16_

Minor
```suggestion
    /// Returns `None` if the file does not exist or if the concept of user-level configurations
    /// doesn't exist on `system`.
```

---

_@sharkdp reviewed on 2025-02-10 08:18_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/configuration_file.rs`:10 on 2025-02-10 08:18_

This mentions `knot.toml` explicitly because this struct does not apply to `pyproject.toml` files?

---

_@MichaReiser reviewed on 2025-02-10 08:26_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata.rs`:244 on 2025-02-10 08:26_

Yeah, I called this out as a simplification in the PR summary:

> Red Knot will watch the user-level configuration file for changes but only if it exists when the process start. It doesn't watch for new configurations,
mainly to simplify things for now (it would require watching the entire .config directory because the knot subfolder might not exist either)

I think it would be nice to have but doesn't seem a priority right now (and can easily be added later)

---

_@MichaReiser reviewed on 2025-02-10 08:26_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/configuration_file.rs`:10 on 2025-02-10 08:26_

Yes, we don't pick up `pyproject.toml` in user directories (unlike ruff, the same as uv) because `pyproject.toml`s are for projects

---

_@sharkdp reviewed on 2025-02-10 08:29_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata.rs`:244 on 2025-02-10 08:29_

Oops, I missed that.

---

_@sharkdp reviewed on 2025-02-10 08:36_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/configuration_file.rs`:39 on 2025-02-10 08:36_

Unrelated to this PR, but why does the `ValueSource::File` variant wrap the `SystemPathBuf` in an `Arc`?

---

_@sharkdp reviewed on 2025-02-10 08:48_

---

_Review comment by @sharkdp on `crates/ruff_db/src/system/os.rs`:104 on 2025-02-10 08:48_

```suggestion
        // thread local because overriding the environment variables breaks test isolation
```

---

_@MichaReiser reviewed on 2025-02-10 08:52_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/configuration_file.rs`:39 on 2025-02-10 08:52_

We don't have to, but it saves a bit of memory when deserializing large configurations. Each deserialized value from a toml file gets associated with the same file. We might then merge the configurations from multiple files (which is why we need to track the source on a value-level). 

---

_Review comment by @sharkdp on `crates/ruff_db/src/system/os.rs`:397 on 2025-02-10 08:59_

This seems unused?

---

_@sharkdp reviewed on 2025-02-10 08:59_

---

_Comment by @MichaReiser on 2025-02-10 09:05_

From @sharkdp on discord

> On your micha/user-level-configuration branch, if I run cargo test -p red_knot_python_semantic locally, it fails with

```
error[E0432]: unresolved import `ruff_db::system::OsSystem`
    --> crates/red_knot_python_semantic/src/module_resolver/resolver.rs:1274:31
     |
1274 |         use ruff_db::system::{OsSystem, SystemPath};
     |                               ^^^^^^^^
     |                               |
     |                               no `OsSystem` in `system`
     |                               help: a similar name exists in the module: `System`
     |
note: found an item that was configured out
    --> /home/shark/ruff/crates/ruff_db/src/system.rs:8:13
     |
8    | pub use os::OsSystem;
     |             ^^^^^^^^
note: the item is gated behind the `os` feature
    --> /home/shark/ruff/crates/ruff_db/src/system.rs:7:7
     |
7    | #[cfg(feature = "os")]
     |       ^^^^^^^^^^^^^^
```

---

_@sharkdp reviewed on 2025-02-10 09:08_

---

_Review comment by @sharkdp on `crates/ruff_db/src/system/os.rs`:359 on 2025-02-10 09:08_

Just to make sure I understand. We can't add something like a `#cfg(test)`-level `set_user_configuration_directory_override(…)` function to `OsSystem`, because we use the same `OsSystem` from multiple tests (and therefore, multiple threads)?

---

_@sharkdp approved on 2025-02-10 09:09_

Thank you!

---

_@MichaReiser reviewed on 2025-02-10 09:13_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:359 on 2025-02-10 09:13_

Hmm, no that was just me being dumb... I should do that instead because it fixes the single-thread limitation. It still wont work in CLI tests but that's because those tests spawn a new process

---

_@MichaReiser reviewed on 2025-02-10 15:44_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:359 on 2025-02-10 15:44_

This turned out a bit annoying because it is now required to pass around the `OsSystem` in the `file_watching` tests but it's way more robust. Thanks for suggesting this. I completely missed that we have an instance where we could store the state on.

---

_Merged by @MichaReiser on 2025-02-10 15:44_

---

_Closed by @MichaReiser on 2025-02-10 15:44_

---

_Branch deleted on 2025-02-10 15:44_

---
