```yaml
number: 19529
title: "[ty] Implement stdlib stub mapping"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/sysroot
created_at: 2025-07-24T13:18:14Z
updated_at: 2025-08-08T19:52:17Z
url: https://github.com/astral-sh/ruff/pull/19529
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Implement stdlib stub mapping

---

_Pull request opened by @Gankra on 2025-07-24 13:18_

by using essentially the same logic for system site-packages, on the assumption that system site-packages are always a subdir of the stdlib we were looking for.

This requires some more testing, but at least on my macbook it lets me jump to e.g. `warnings.deprecated`.

---

_Review requested from @carljm by @Gankra on 2025-07-24 13:18_

---

_Review requested from @AlexWaygood by @Gankra on 2025-07-24 13:18_

---

_Review requested from @sharkdp by @Gankra on 2025-07-24 13:18_

---

_Review requested from @dcreager by @Gankra on 2025-07-24 13:18_

---

_Review requested from @MichaReiser by @Gankra on 2025-07-24 13:18_

---

_Comment by @github-actions[bot] on 2025-07-24 13:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@Gankra reviewed on 2025-07-24 13:23_

---

_Review comment by @Gankra on `crates/ty_project/src/metadata/options.rs`:173 on 2025-07-24 13:23_

As implemented this will redo a lot of work the site-packages path does -- for now I'm leaving them separate in case the implementations should diverge more, but I can merge them together once we're more confident.

---

_@Gankra reviewed on 2025-07-24 13:24_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:415 on 2025-07-24 13:24_

Need to iterate on this part.

---

_@Gankra reviewed on 2025-07-24 13:24_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:587 on 2025-07-24 13:24_

Whoops, forgot to delete this.

---

_@Gankra reviewed on 2025-07-24 13:27_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/site_packages.rs`:505 on 2025-07-24 13:27_

I don't think we should be using this? But I'm not sure.

---

_@Gankra reviewed on 2025-07-24 13:28_

---

_Review comment by @Gankra on `crates/ty_test/src/lib.rs`:286 on 2025-07-24 13:28_

(Note: this is in testing code)

---

_@AlexWaygood reviewed on 2025-07-24 13:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:415 on 2025-07-24 13:28_

hmm, I'm not sure it's correct for this to come after e.g. the `site-packages` search path. I think this would mean that if e.g. the user had the moribund [`asyncio` package from PyPI](https://pypi.org/project/asyncio/) installed into their venv, they had `import asyncio` in a file, and they did go-to-definition on the `asyncio` symbol, we'd jump to the module in the venv's site-packages directory. But at runtime, the module in the venv's site-packages directory will be totally ignored: the stdlib takes precedence in module resolution over third-party packages, so `import asyncio` will resolve to the stdlib `asyncio` module.

---

_Label `ty` added by @ntBre on 2025-07-24 13:28_

---

_@AlexWaygood reviewed on 2025-07-24 13:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:415 on 2025-07-24 13:29_

oh sorry, I posted this comment before I saw your comment above

---

_@AlexWaygood reviewed on 2025-07-24 13:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:505 on 2025-07-24 13:30_

I _think_ `base_executable_home_path` should always be the same for the parent environment? It would be pretty weird if not? But you're the uv expert here ;)

---

_@Gankra reviewed on 2025-07-24 13:33_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:415 on 2025-07-24 13:33_

No this is still a great insight, thanks!

---

_Comment by @AlexWaygood on 2025-07-24 13:37_

Here's the things I'd want to manually double-check that this works with:
- A uv-managed Python installation
- A Brew-installed Python installation
- A pyenv installation of Python
- GraalPy
- PyPy

(If it doesn't work with all of them right now, obviously fine to TODO some of them and leave them for followups!)

---

_Label `server` added by @AlexWaygood on 2025-07-24 13:38_

---

_Review request for @carljm removed by @carljm on 2025-07-25 01:12_

---

_Comment by @Gankra on 2025-08-07 14:55_

Hmm ok so this works great with uv-managed cpython/pypy/graalpy but need to work harder for homebrew at least. It looks like they go out of their way to make site-packages follow this format, but the stdlib is somewhere else. Notably the python binary it refers to is symlinked, and if we follow that symlink then this logic once again works (the stdlib is in fact in `../Frameworks/Python.framework/Versions/3.13/lib/python3.13/`).

---

_Comment by @Gankra on 2025-08-07 16:35_

Newest commit handles homebrew and the macos system python.

---

_Comment by @github-actions[bot] on 2025-08-07 16:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:174 on 2025-08-08 08:06_

I think it would be helpful to lg any error at least with `debug` severity. It might otherwise be difficult to pin-point why go to definition etc. isn't working for users (but it's working fine for everyone of the team because we all use uv and not some other bespoke distribution)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:330 on 2025-08-08 08:11_

Curious to hear @AlexWaygood's thoughts on this. I suspect that ty's results will be fairly bonkers when the standard library is shadowed 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:1125 on 2025-08-08 08:16_

Nit: If a small preference to use a `match` or `let` here for `python_version` to avoid the unwrap. It's, unfortunately, a bit more verbose without if let chains (only one more month before we can use them)


```suggestion
        match (implementation, python_version) {
            (PythonImplementation::CPython, Some(python_version)) if python_version.free_threaded_build_available() => {
                let alternative_path =
                sys_prefix_path.join(format!("lib/python{}t", python_version.unwrap()));
                if system.is_directory(&alternative_path) {
                    return Ok(alternative_path);
                }
            }
        }
        if matches!(implementation, PythonImplementation::CPython)
            && python_version.is_some_and(PythonVersion::free_threaded_build_available)
        {
            
        }
    }
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:1291 on 2025-08-08 08:18_

Is this check redundant, given that we skip if `entry.file_type().is_directory()` or should this come after canonicalize?

---

_@MichaReiser approved on 2025-08-08 08:20_

Nice! 

Would it be very involved to write a test for this similar to the other tests we have in `site_packages`?

---

_@Gankra reviewed on 2025-08-08 13:42_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/site_packages.rs`:1125 on 2025-08-08 13:42_

note that this is copy-pasted logic from the equivalent site-packages logic (prioritized keeping them the same where i can)

---

_@Gankra reviewed on 2025-08-08 13:44_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:330 on 2025-08-08 13:44_

They mentioned https://github.com/astral-sh/ty/issues/523

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/site_packages.rs`:1291 on 2025-08-08 13:48_

Ah good catch.

---

_@Gankra reviewed on 2025-08-08 13:48_

---

_Comment by @Gankra on 2025-08-08 19:51_

I am going to land this, I'm reasonably confident in how safe the implementation is. I expect more bugs will come up with us failing to find Fun stdlibs but this is still a big win.

---

_Merged by @Gankra on 2025-08-08 19:52_

---

_Closed by @Gankra on 2025-08-08 19:52_

---

_Branch deleted on 2025-08-08 19:52_

---
