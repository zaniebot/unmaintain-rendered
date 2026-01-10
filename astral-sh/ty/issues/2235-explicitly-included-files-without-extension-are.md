---
number: 2235
title: Explicitly included files without extension are not checked
type: issue
state: closed
author: janlarres
labels:
  - configuration
assignees: []
created_at: 2025-12-27T07:14:21Z
updated_at: 2026-01-07T10:38:03Z
url: https://github.com/astral-sh/ty/issues/2235
synced_at: 2026-01-10T01:51:14Z
---

# Explicitly included files without extension are not checked

---

_Issue opened by @janlarres on 2025-12-27 07:14_

### Summary

I have a standalone script `scripts/run` in my project that I want to typecheck along with the main project code.

So I have this in my `pyproject.toml`:

```toml
[tool.ty.src]
include = ["mypackage", "scripts/run"]
```

However, the `run` script gets ignored. I'm assuming this is because of some filtering for `*.py` files in directories. I think that if the include is a file instead of a directory then it would be useful to include it as-is instead of filtering to allow this use case.

Interestingly it works fine if I explicitly run ty as `ty check scripts/run`.

### Version

ty 0.0.7

---

_Label `configuration` added by @AlexWaygood on 2025-12-27 08:35_

---

_Comment by @MichaReiser on 2025-12-27 09:45_

Hmm, yeah, this is a bit tricky. We support checking files without extension if you pass them directly to the CLI. But we currently don't support including files without a `py`, `pyi`, or `ipynb` extension when using `include`.

We could consider allowing exact matches in `include`s too. But I'm not sure this would be intuitive, and it also seems too limited if you want to check a handful of files that have no extension (or any other non-standard extension).

We could automatically change `*` to `*.{py,pyi,ipynb}` unless the glob ends with an extension (and the same for `**/*`), and, therefore, to the filtering as part of the globbing. However, this would only work for individual files. Including a set of files without an extension would not be possible.

Another alternative is to allow custom extension mappings, similar to what we have in ruff where you can map a file extension to a file type (like `custompy` -> `py` would make ty check all files ending with `.custmpy` as if they were regular python files). Adding a mapping for an empty extension would make ty check every file without an extension. That might be annoying too. 


Are you aware of any other tools that allows checking files without an extension (e.g. what other tools did you use before). Do you know how they handle the extension-less case?

---

_Comment by @MichaReiser on 2025-12-27 11:28_

I think we could append the `*.{py,pyi,ipynb}` filter if we see that the glob has no filename part (other than `*`). 

---

_Comment by @janlarres on 2025-12-27 23:45_

Pyright works for me with an identical include specification. As far as I can tell it supports simple file includes, but not wildcards that resolve to files:

https://github.com/microsoft/pyright/blob/1576956c32c8b75f3202203f3cdedd21c8d3f9f7/packages/pyright-internal/src/analyzer/sourceEnumerator.ts#L173

This behaviour got implemented here:
https://github.com/microsoft/pyright/pull/5463

Pyrefly also supports explicitly specified files (i.e. no wildcards):
https://github.com/facebook/pyrefly/blob/0cf3e5352f1be66e2fee0d038f38808b749e4108/crates/pyrefly_util/src/globs.rs#L160
https://github.com/facebook/pyrefly/blob/0cf3e5352f1be66e2fee0d038f38808b749e4108/crates/pyrefly_util/src/globs.rs#L343
https://github.com/facebook/pyrefly/blob/0cf3e5352f1be66e2fee0d038f38808b749e4108/crates/pyrefly_util/src/globs.rs#L104

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:45_

---

_Closed by @MichaReiser on 2026-01-07 10:38_

---
