```yaml
number: 16521
title: "[red-knot] Case sensitive module resolver"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/case-sensitive-module-resolver
created_at: 2025-03-05T17:14:20Z
updated_at: 2025-03-14T19:19:45Z
url: https://github.com/astral-sh/ruff/pull/16521
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Case sensitive module resolver

---

_@MichaReiser_

## Summary

This PR implements the first part of https://github.com/astral-sh/ruff/discussions/16440. It ensures that Red Knot's module resolver is case sensitive on all systems. 

This PR combines a few approaches:

1. It uses `canonicalize` on non-case-sensitive systems to get the real casing of a path. This works for as long as no symlinks or mapped network drives (the windows `E:\` is mapped to `\\server\share` thingy). This is the same as what Pyright does
2. If 1. fails, fall back to recursively list the parent directory and test if the path's file name matches the casing exactly as listed in by list dir. This is the same approach as CPython takes in its module resolver. The main downside is that it requires more syscalls because, unlike CPython, we Red Knot needs to invalidate its caches if a file name gets renamed (CPython assumes that the folders are immutable). 

It's worth noting that the file watching test that I added that renames `lib.py` to `Lib.py` currently doesn't pass on case-insensitive systems. Making it pass requires some more involved changes to `Files`. I plan to work on this next. There's the argument that landing this PR on its own isn't worth it without this issue being addressed. I think it's still a good step in the right direction even when some of the details on how and where the path case sensitive comparison is implemented.

## Test plan

I added multiple integration tests (including a failing one). I tested that the `case-sensitivity` detection works as expected on Windows, MacOS and Linux and that the fast-paths are taken accordingly. 

---

_Label `red-knot` added by @MichaReiser on 2025-03-05 17:14_

---

_Comment by @github-actions[bot] on 2025-03-05 17:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2025-03-07 14:26_

---

_Review requested from @carljm by @MichaReiser on 2025-03-07 14:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-07 14:26_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-07 14:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:1884 on 2025-03-07 20:16_

This seems to rely on creating a parent directory, so why isn't it `write_file_all`?

---

_Review comment by @carljm on `crates/ruff_db/src/system/os.rs`:643 on 2025-03-08 00:45_

Does this mean that if someone on a case-sensitive file system happens to run red-knot from an all-caps CWD, they may observe a significant slowdown?

That's not likely to ever happen... but it's pretty weird/unpleasant behavior if it does happen.

Would an alternative be to try lower-casing in that scenario? Either all-lower or all-upper should be different from the the original path.

---

_@carljm approved on 2025-03-08 00:45_

What a headache! Thank you for taking this on.

---

_@MichaReiser reviewed on 2025-03-08 18:02_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:643 on 2025-03-08 18:02_

> What a headache!

I hope you referred to file systems and not my PR ;)

Yes. Another example is if you run Red Knot from a directory named `/1234` where the upper and lower-case names are identical. The reason why I think this is extremely unlikely is because the code lower-cases the entire path and not just the last component. So `/Users/micha/1234` would be converted to `/USERS/MICHA/1234` and the test would still succeed. 

We could extend the logic to also try `current_exe` for which the casing should differ unless someone renames the `red_knot` binary to `1234`... 🤷 The only downside this has is that Red Knot might be installed in your home directory (let's say that's case sensitive) and your project runs on a case-insensitive network drive. However, I'm not sure how much we should worry about this case because the best we can do here ultimately is an approximation (or make everything slower than it has to be in most projects). 


---

_@carljm reviewed on 2025-03-11 00:20_

---

_Review comment by @carljm on `crates/ruff_db/src/system/os.rs`:643 on 2025-03-11 00:20_

> I hope you referred to file systems and not my PR ;)

Yes, file systems!

> We could extend the logic to also try `current_exe` for which the casing should differ unless someone renames the `red_knot` binary to `1234`... 🤷 The only downside this has is that Red Knot might be installed in your home directory (let's say that's case sensitive) and your project runs on a case-insensitive network drive.

The same issue could occur if you run knot from a cwd on one filesystem and use `--project` to check files on another one. In theory you could even have your project on one filesystem and your venv/site-packages on a different one.

Ultimately I am fine with however you want to handle this.


---

_@MichaReiser reviewed on 2025-03-14 19:14_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:643 on 2025-03-14 19:14_

I'll leave it as is for now and plan to revisit it as part of the file watching on case insensitive systems work because, depending on which approach we take, falling back to `Unknown` might no longer be an option (in which case a more precise inference is important).

---

_Merged by @MichaReiser on 2025-03-14 19:16_

---

_Closed by @MichaReiser on 2025-03-14 19:16_

---

_Branch deleted on 2025-03-14 19:16_

---

_@MichaReiser reviewed on 2025-03-14 19:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:1884 on 2025-03-14 19:16_

See https://github.com/astral-sh/ruff/pull/16518#discussion_r1981891383

Happy to change the naming if you prefer the consistency of having the `_all` prefix everywhere.

---

_Comment by @github-actions[bot] on 2025-03-14 19:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---
