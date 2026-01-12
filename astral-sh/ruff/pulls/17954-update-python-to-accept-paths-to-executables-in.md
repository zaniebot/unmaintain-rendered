```yaml
number: 17954
title: "Update `--python` to accept paths to executables in environments"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: zb/resolve-python
created_at: 2025-05-08T15:31:36Z
updated_at: 2025-05-13T14:18:56Z
url: https://github.com/astral-sh/ruff/pull/17954
synced_at: 2026-01-12T15:56:08Z
```

# Update `--python` to accept paths to executables in environments

---

_@zanieb_

## Summary

Updates the `--python` flag to accept Python executables in virtual environments. Notably, we do not query the executable and it _must_ be in a canonical location in a virtual environment. This is pretty naive, but solves for the trivial case of `ty check --python .venv/bin/python3` which will be a common mistake (and `ty check --python $(which python)`)

I explored this while trying to understand Python discovery in ty in service of https://github.com/astral-sh/ty/issues/272, I'm not attached to it, but figure it's worth sharing.

As an alternative, we can add more variants to the `SearchPathValidationError` and just improve the _error_ message, i.e., by hinting that this looks like a virtual environment and suggesting the concrete alternative path they should provide. We'll probably want to do that for some other cases anyway (e.g., `3.13` as described in the linked issue)

This functionality is also briefly mentioned in https://github.com/astral-sh/ty/issues/193

Closes https://github.com/astral-sh/ty/issues/318

## Test Plan

e.g.,

```
uv run ty check --python .venv/bin/python3
```

needs test coverage still


---

_Label `ty` added by @zanieb on 2025-05-08 15:31_

---

_@zanieb reviewed on 2025-05-08 15:32_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:265 on 2025-05-08 15:32_

We could be more strict here, and require `python.exe` on Windows and `python` | `python3` | `python3.XX` on Unix. The last case is a bit annoying. We use a regular expression in uv, but I think that's probably not worth moving here yet? We might end up needing it here for nice error messages for other cases regardless though.

---

_Comment by @github-actions[bot] on 2025-05-08 15:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @zanieb on 2025-05-11 15:57_

I'm going to mark this as ready for review, for consensus on an approach. I'll add test coverage and update documentation before merging.

---

_Marked ready for review by @zanieb on 2025-05-11 15:57_

---

_Review requested from @carljm by @zanieb on 2025-05-11 15:57_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-11 15:57_

---

_Review requested from @sharkdp by @zanieb on 2025-05-11 15:57_

---

_Review requested from @dcreager by @zanieb on 2025-05-11 15:57_

---

_Comment by @AlexWaygood on 2025-05-11 16:03_

(I haven't reviewed yet — currently in an airport on my phone — but I'm on board with the idea!)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:258 on 2025-05-12 08:12_

Should we also test if the file has the executable bit set (`path_metadata` instead of `is_file`)

---

_@MichaReiser approved on 2025-05-12 08:12_

This looks good to me. We should update the CLI and settings documentation

---

_@zanieb reviewed on 2025-05-12 20:11_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:258 on 2025-05-12 20:11_

Seems reasonable, but is a little more painful cross-platform for little gain since we're just going to fail downstream anyway if we can't find site-packages

---

_Review comment by @AlexWaygood on `crates/ty/src/args.rs`:65 on 2025-05-12 20:19_

maybe

```suggestion
    /// ty will search in the resolved installation's `site-packages` directories for type information and
```

?

---

_Review comment by @AlexWaygood on `crates/ty/src/args.rs`:54 on 2025-05-12 20:23_

```suggestion
    /// Path to the Python environment ty should use to resolve type information and third-party
```

---

_@AlexWaygood approved on 2025-05-12 20:23_

---

_Review comment by @zanieb on `crates/ty/src/args.rs`:65 on 2025-05-12 20:24_

I don't want to use "installation" because it includes virtual environments, but I can use "resolved environment's"

---

_@zanieb reviewed on 2025-05-12 20:24_

---

_@zanieb reviewed on 2025-05-12 20:25_

---

_Review comment by @zanieb on `crates/ty/src/args.rs`:54 on 2025-05-12 20:25_

I was trying to avoid changing the wording here, but can do.

---

_@zanieb reviewed on 2025-05-12 20:25_

---

_Review comment by @zanieb on `crates/ty/src/args.rs`:54 on 2025-05-12 20:25_

Generally, I like shorter descriptions for the top-level line.

---

_@zanieb reviewed on 2025-05-12 20:26_

---

_Review comment by @zanieb on `crates/ty/src/args.rs`:54 on 2025-05-12 20:26_

(so.. if you want me to change it I'll change it more hehe)

---

_Review comment by @AlexWaygood on `crates/ty/src/args.rs`:65 on 2025-05-12 20:27_

that works for me!

---

_@AlexWaygood reviewed on 2025-05-12 20:27_

---

_@AlexWaygood reviewed on 2025-05-12 20:28_

---

_Review comment by @AlexWaygood on `crates/ty/src/args.rs`:54 on 2025-05-12 20:28_

heh, I'm up for more invasive changes! Agree that a shorter top line would be great

---

_Merged by @zanieb on 2025-05-12 20:39_

---

_Closed by @zanieb on 2025-05-12 20:39_

---

_Branch deleted on 2025-05-12 20:39_

---

_Renamed from "Update `--python` to accept paths to executables in virtual environments" to "Update `--python` to accept paths to executables in environments" by @zanieb on 2025-05-12 20:39_

---

_@zanieb reviewed on 2025-05-12 20:39_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:258 on 2025-05-12 20:39_

Let me know if you think it's worth pursuing regardless.

---

_Label `cli` added by @dhruvmanila on 2025-05-13 14:18_

---
