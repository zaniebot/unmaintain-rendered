```yaml
number: 19284
title: "[ty] Fix server version"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-server-version
created_at: 2025-07-11T13:25:11Z
updated_at: 2025-07-14T07:06:36Z
url: https://github.com/astral-sh/ruff/pull/19284
synced_at: 2026-01-12T15:56:36Z
```

# [ty] Fix server version

---

_@MichaReiser_

## Summary
Closes https://github.com/astral-sh/ty/issues/814

cargo-dist only sets the version of the ty crate. That means, the `CARGO_PKG_VERSION` is always 0.0.0 for the ty_server. 

This PR works around this by adding the `program_version` to `ruff_db` which gets initialized by the ty crate (where the version is correct)
and the server can read from. This has the nice side effect that the same version can be used in the type checking panic handler (in `ty_project`). 

An alternative would have been to pass the version to `ty_server` but we then couldn't use it in the catch handler in `ty_project`. 

I made some changes to ty's build script to take the commit from the ruff repository when not built inside the ty repository. 
This gives us at least some meaningful version identifier for debug builds.

## Test Plan

Version in `initialize` response:

```
    "serverInfo": {
        "name": "ty",
        "version": "ruff/0.12.2+87 (426fa4bb1 2025-07-11)"
    }
```

Version logged during startup

```
2025-07-11 15:19:23.122004000 DEBUG Version: ruff/0.12.2+87 (426fa4bb1 2025-07-11)
2025-07-11 15:19:23.127378000  INFO Defaulting to python-platform `darwin`
2025-07-11 15:19:23.127599000 DEBUG Discovering virtual environment in `/Users/micha/astral/test`
```

Version in panic handler


```
error[panic]: Panicked at crates/ty_python_semantic/src/types.rs:97:5 when checking `/Users/micha/astral/test/create.py`: `Test`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["target/debug/ty", "check", "../test/create.py"]
info: Version: ruff/0.12.2+88 (0f58920fc 2025-07-11)
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:474
```

---

_Review requested from @carljm by @MichaReiser on 2025-07-11 13:25_

---

_Label `server` added by @MichaReiser on 2025-07-11 13:25_

---

_Label `ty` added by @MichaReiser on 2025-07-11 13:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-11 13:25_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-11 13:25_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-11 13:25_

---

_Label `server` added by @MichaReiser on 2025-07-11 13:25_

---

_Label `ty` added by @MichaReiser on 2025-07-11 13:25_

---

_Comment by @github-actions[bot] on 2025-07-11 13:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-11 13:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2025-07-11 14:00_

Hmm... isn't showing the ruff version in the ty server even more confusing than showing 0.0.0? Sure, the commit hash is useful for us, but most users won't know that the ty codebase lives inside the ruff repo?

---

_Comment by @MichaReiser on 2025-07-11 14:18_

> Hmm... isn't showing the ruff version in the ty server even more confusing than showing 0.0.0? Sure, the commit hash is useful for us, but most users won't know that the ty codebase lives inside the ruff repo?

We only use the ruff commit version when you build ty from within the Ruff repository. Only developers should really be doing this. The builds that we release are all built from within the ty repository and, therefore, they use the version from the `dist-workspace.toml` and the commit from the ty repository.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-11 18:09_

---

_Comment by @dhruvmanila on 2025-07-14 04:05_

> cargo-dist only sets the version of the ty crate. That means, the `CARGO_PKG_VERSION` is always 0.0.0 for the ty_server.

Should we use the `CARGO_PKG_VERSION` from the `ty` crate? This is what the `ruff_server` crate does as well.

---

_Review comment by @dhruvmanila on `crates/ruff/src/lib.rs`:134 on 2025-07-14 04:09_

Why is this required? Isn't this the entrypoint for the Ruff CLI?

---

_@dhruvmanila approved on 2025-07-14 04:09_

---

_@MichaReiser reviewed on 2025-07-14 07:05_

---

_Review comment by @MichaReiser on `crates/ruff/src/lib.rs`:134 on 2025-07-14 07:05_

This is more a precaution that we don't introduce a panic in ruff when we add a new path accessing `version` (Both the diagnostics and `ruff analzye` use the `ruff_db` crate).

---

_Comment by @MichaReiser on 2025-07-14 07:06_

> Should we use the CARGO_PKG_VERSION from the ty crate? This is what the ruff_server crate does as well.

I think it's important that the server version matches the output of `ty version`. That's why I went with using that version string instead.

---

_Merged by @MichaReiser on 2025-07-14 07:06_

---

_Closed by @MichaReiser on 2025-07-14 07:06_

---

_Branch deleted on 2025-07-14 07:06_

---
