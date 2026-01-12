```yaml
number: 7828
title: AST import traversal to generate requirements
type: issue
state: open
author: nmichlo
labels:
  - wish
assignees: []
created_at: 2024-10-01T07:42:21Z
updated_at: 2025-09-27T12:32:35Z
url: https://github.com/astral-sh/uv/issues/7828
synced_at: 2026-01-12T15:59:17Z
```

# AST import traversal to generate requirements

---

_@nmichlo_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

UV is in an amazing place to solve the python import traversal and scanning issue for larger projects. There are existing projects that have tried to do this (and I've recently implemented one of them myself that tries to fix some of the issues, but there are still usability issues that I think UV is in a great place to solve).

**Existing Projects**

Projects that I know of that have tried to do this include:
- https://github.com/rmad17/pyreqs
- https://github.com/nmichlo/pydependence
- _more that I can't remember at the moment._

**Problems**

The main problem with past projects that generate dependencies from imports are that they usually have some combination of the following:
- naively find all imports in a package and try to use them, WITHOUT, traversal correct traversal between modules
  * use all files in a folder instead of correctly traversing, or respecting python path, or discovering modules.
- possibly hardcoded based on the current environment
- have hardcoded import to package maps that cannot be configured
  * e.g. `PIL` to `pillow`, or `cv2` to `opencv-python` and/or `opencv-python-contrib`
  * rather have default list for popular packages that can be overridden
- only generate requirements.txt, no support for modifying pyproject.toml or generating optional dependency lists e.g. from some specific starting point by tracing code from that point instead of using the entire module
  * very useful for generating optional extras e.g. have main dependencies, and then specific starting points or including lazy imports can get you extras.
- support for lazy imports, as well as custom hooks for lazy imports (usually imports inside functions)
  * same as above e.g. you have a machine learning library of models, each that depends on specific frameworks, but you only use some of them in practice. You can generate extras for each module and trace it through the system.
  * e.g. `lazy_import("torch.nn")` << custom but could support with hooks
  * e.g. `def my_fn(): import torch.nn`
  * e.g. `if TYPE_CHECKING: import torch.nn`
- limited configurability or customisability or overridability
  * overriding import mappings, e.g. `tests` could be in multiple repos, or `google.protobuf` not `google` is installed by something specific, or packages that provide multiple modules
- limiting or extending the scope of dependency resolution, including across multiple repos and projects
  * useful if you have multiple repos in a private org and want to generate global minimal requirements
- pre-commit hooks
  * automatically run when you commit
- overrides / extra imports for specific outputs

**Partially resolved**

I tried to resolve many of these issues with `pydependence` above, but there are still problems that I think UV could solve:
- cannot automatically fetch dependencies that it wants to resolve against.
  * e.g. files need to be present on system before resolving (not super important, usually never used as you're doing this on your own code)
- relatively fast, but could be faster, astral has great rust python parsing support
- hardcoded versions
  * if imports are found and mapped to requirements, this could automatically be used with UV dependency resolution to generate a lockfile that could be used instead of maintaining this yourself.
- a little too complex, too much configuration needed to get it working initially, could be simplified a lot.
- examples:
  * generated dependencies (can also manually define):
    + [pyproject.toml deps](https://github.com/nmichlo/pydependence/blob/7c02eb671b7ad25c6dbb67c1c27c2e314cb0da33/pyproject.toml#L107-L120)
    + [pyproject.toml extras](https://github.com/nmichlo/pydependence/blob/7c02eb671b7ad25c6dbb67c1c27c2e314cb0da33/pyproject.toml#L131-L231)
  * config for the above (could be simplified):
    + [pyproject.toml config](https://github.com/nmichlo/pydependence/blob/7c02eb671b7ad25c6dbb67c1c27c2e314cb0da33/pyproject.toml#L33-L75)


**Why in UV**:
- difficult to maintain minimal needed requirements for projects as teams grow and context is lost
- want to generate or install minimal needed requirements for specific code paths e.g. CLI or service contained in same repo
  * useful for dockerfiles or pyinstaller (can even pipe UV output into pyinstaller)

------

Is this something that UV would ever be open to and maybe a design discussion could be started? Possibly with a reduced set of features to start?

**Possible Implementation** (based on pydependence)

The fact that UV can resolve deps separately from the import scanning means that a lot of the complexity is removed from such  project as you just need to, UV also has much of this support functionality already stable and used in other parts of the codebase as far as I can tell. The general algorithm from `pydependence` is:
1. define where to find source files (can be PYTHONPATH-like, exact module paths, from the env, could even support git repos)
2. construct a graph of imports based on source files
3. traverse graph to find used imports (from configurable starting points, default would be all files)
4. map imports to requirements (default would be common packages, but can override or add more)
5. use existing functionality to generate requirements files OR modify pyproject.toml with deps/extras


**Help on implementation**

Its possible I could assist in an implementation.


---

_Renamed from "import traversal/scanning to dependencies " to "AST import traversal to generate requirements" by @nmichlo on 2024-10-01 07:42_

---

_Label `wish` added by @charliermarsh on 2024-10-08 22:29_

---

_Comment by @nmichlo on 2024-10-13 13:33_

Would happily help contribute towards this if I know where to start and desired scope. Although probably most beneficial for larger companies struggling to maintain deps across projects rather than standard users.

`pydependence` is probably the most flexible implementation that I know of currently, and can aim to replicate some of its functionality, possibly with a more simplified interface to start. e.g. just single repo resolution, and can then extend towards different starting scopes and multiple project resolution?


---

_Comment by @nmichlo on 2025-09-27 12:32_

This issue is probably better suited to ruff now especially since it has 'ruff analyze graph' / ruff_graph crate 

---
