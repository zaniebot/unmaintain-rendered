```yaml
number: 1682
title: "Should `PYTHONPATH` search paths have a lower priority than first-party search paths? Should `./src` be considered a non-first-party search path?"
type: issue
state: open
author: adamjstewart
labels:
  - needs-decision
  - imports
assignees: []
created_at: 2025-11-29T16:55:26Z
updated_at: 2025-12-02T19:43:32Z
url: https://github.com/astral-sh/ty/issues/1682
synced_at: 2026-01-12T15:54:25Z
```

# Should `PYTHONPATH` search paths have a lower priority than first-party search paths? Should `./src` be considered a non-first-party search path?

---

_@adamjstewart_

### Summary

Testing out ty for the first time and found that it doesn't support relative imports?

Steps to reproduce:
```console
> git clone https://github.com/torchgeo/torchgeo.git
> cd torchgeo
> pip install .
> ty check
error[unresolved-import]: Cannot resolve imported module `.agrifieldnet`
 --> torchgeo/datamodules/__init__.py:6:7
  |
4 | """TorchGeo datamodules."""
5 |
6 | from .agrifieldnet import AgriFieldNetDataModule
  |       ^^^^^^^^^^^^
7 | from .bigearthnet import BigEarthNetDataModule
8 | from .bright import BRIGHTDFC2025DataModule
  |
info: Searched in the following paths during module resolution:
info:   1. .../lib/python3.13/site-packages (extra search path specified on the CLI or in your config file)
info:   2. .../lib/python3.13/site-packages (extra search path specified on the CLI or in your config file)
info:   3. .../lib/python3.13/site-packages (extra search path specified on the CLI or in your config file)
info:   4. /Users/Adam/torchgeo (first-party code)
info:   5. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
...
```
Possibly related to https://github.com/astral-sh/ty/issues/839, although in my case, there are no invalid module names anywhere in my path all the way to root (`/Users/Adam/torchgeo`). Based on that conversation, I'm guessing that relative imports normally are expected to work most of the time, hence why I'm opening an issue.

Note that I currently have no custom configuration for ty. Let me know if a default `--project` is required for relative import support.

@isaaccorley

### Version

ty 0.0.1-alpha.29

---

_Label `imports` added by @AlexWaygood on 2025-11-29 16:58_

---

_Comment by @adamjstewart on 2025-11-29 17:17_

Figured out my problem. I had torchgeo in my local directory _and_ in my PYTHONPATH. If I remove the second copy, relative imports are found fine. So there may be a bug with relative imports when a module is found multiple times?

---

_Comment by @charliermarsh on 2025-11-29 17:19_

(👍 Yeah just confirming that I can't reproduce this on a fresh clone.)

---

_Comment by @AlexWaygood on 2025-11-29 17:21_

I think this is the same as https://github.com/astral-sh/ty/issues/870 / https://github.com/astral-sh/ty/issues/1539 ?

---

_Comment by @adamjstewart on 2025-11-29 17:27_

Can't tell, but happy to close this if the experts think it's a duplicate. The issue isn't that we're failing to resolve any import, but that we are resolving too many imports or in the wrong order. Local scope should always take precedence over PYTHONPATH, as that's how Python imports are ordered.

---

_Comment by @AlexWaygood on 2025-11-29 17:39_

Hmm, that's an interesting point. Currently we model `PYTHONPATH` entries the same way as entries specified using our [`extra-paths`](https://docs.astral.sh/ty/reference/configuration/#extra-paths) configuration settings -- they have a priority higher than first-party code, as specified by item (1) in the typing spec's [import resolution ordering](https://typing.python.org/en/latest/spec/distributing.html#import-resolution-ordering). But the [runtime spec](https://docs.python.org/3/library/sys_path_init.html) is clear that `PYTHONPATH` entries come between first-party code and any `site-packages` search paths.

One complicating factor, however, is that there are lots of search paths that we treat as first-party but which would at runtime only be present due to editable installations in `site-packages`. For example, we add an `src/` search path that by default, and we consider this to be a first-paty search path, but at runtime this search path would be present due to an editable installation in site-packages. At runtime the only "first-party" search path that would take precedence over `PYTHONPATH` would be the current directory (`.`), and editable installs in `site-packages` (`./src`) would still have a lower priority than any search paths added by `PYTHONPATH`.

#1539 _would_ fix the problem you're experiencing, however, even though it's not the fix you're suggesting...

---

_Renamed from "Relative imports not found" to "Should `PYTHONPATH` search paths have a lower priority than first-party search paths? Should `./src` be considered a first-party search path?" by @AlexWaygood on 2025-11-29 17:40_

---

_Renamed from "Should `PYTHONPATH` search paths have a lower priority than first-party search paths? Should `./src` be considered a first-party search path?" to "Should `PYTHONPATH` search paths have a lower priority than first-party search paths? Should `./src` be considered a non-first-party search path?" by @AlexWaygood on 2025-11-29 17:41_

---

_Label `needs-decision` added by @AlexWaygood on 2025-11-29 17:42_

---

_Comment by @adamjstewart on 2025-11-29 18:39_

Upon reading https://typing.python.org/en/latest/spec/distributing.html#import-resolution-ordering, I would personally interpret this as:

1. Type checker-specific configuration (`extra-paths` makes sense here)
2. The thing I'm trying to type check (my local directory here)
3. Stubs from the standard library
4. Stubs for installed packages (found via the usual import order)
5. Installed packages (found via the usual import order)
6. Type checker-specific vendored stubs

Therefore, `PYTHONPATH` should go into 4 and 5, after the local directory. This also matches import order. I would be curious to know how other type checkers order this, but with mypy, it always preferred the local directory as far as I can tell.

---

_Comment by @adamjstewart on 2025-11-29 18:44_

Here are the docs for [mypy's search order](https://mypy.readthedocs.io/en/stable/running_mypy.html#how-imports-are-found):

1. `MYPYPATH` env var
2. `mypy_path` config flag
3. Directory that mypy is run on
4. Installed packages
5. Typeshed repo

So this matches my interpretation that installed packages should come after the local directory.

---

_Comment by @carljm on 2025-11-30 06:17_

As far as I can tell, mypy doesn't respect `PYTHONPATH` at all, and the typing spec doesn't mention `PYTHONPATH`, so I don't think either of those provide a useful guide here. I don't think it's correct to equate `PYTHONPATH` with "installed packages" -- installed packages go into the `site-packages` directory in a Python installation or virtual environment. `PYTHONPATH` can be used to point to arbitrary code anywhere, which could be first-party or third-party, and probably isn't "installed" (if it were, it would already be in `site-packages` and `PYTHONPATH` wouldn't be needed to point to it.)

I do think the runtime behavior (that `PYTHONPATH` entries follow the "current directory or directory of the executed script") suggests that we should place `PYTHONPATH` paths after first-party code.

---

_Comment by @carljm on 2025-11-30 06:24_

The title of this issue currently asks two questions:

> Should PYTHONPATH search paths have a lower priority than first-party search paths?

I suspect the ideal answer here (based on runtime behavior) is "yes", and we should focus this issue on that change.

> Should `./src` be considered a non-first-party search path?

The answer to this seems to me to clearly be "no"; I'm a bit confused why that would even be a consideration? What would `./src` be if not first-party? It seems like basically the canonical first-party path.

EDIT: never mind, I just re-read the above comment that makes the reasoning behind this clear. I still think that "maybe `./src` should not be first-party" is the wrong framing: it must be first-party, because the most important aspect of "first-party" is checked-by-default (vs just "available as import source"), and `./src` should be checked by default.

My suspicion is that trying to mimic runtime behavior too closely might be a can of worms. We can't model all runtime behaviors exactly. For example, the first path on `sys.path` at runtime may not be the "current directory" at all, it might be the directory containing the executed script. It's not clear how we'd model that, since you don't "execute a script" with ty.

One thing we could do is add `PYTHONPATH` to `environment.roots` instead of to `extra-paths`, and (if `environment.roots` is otherwise unset) we could add the `PYTHONPATH` entries after `.` and before `./src`?

---

_Comment by @adamjstewart on 2025-11-30 09:47_

> As far as I can tell, mypy doesn't respect `PYTHONPATH` at all

My "installed" libraries are only available via `PYTHONPATH`, and mypy finds them. Although mypy may not have any explicit handling of `PYTHONPATH`, if it tries to import packages, then it respects `PYTHONPATH`.

> I don't think it's correct to equate `PYTHONPATH` with "installed packages" -- installed packages go into the `site-packages` directory in a Python installation or virtual environment.

This depends on your mode of "installation". Many from-source packages managers like Nix, Guix, Spack, and EasyBuild install all packages into unique hashed directories. This allows, e.g., installing 2 different versions of numpy with 4 different BLAS/LAPACK libraries and 3 different compilers. These package managers rely entirely on `PYTHONPATH` (often via `module load`) to make installed packages available. While pip/uv/conda are more common on single-user systems, Spack/EasyBuild are more common on multi-user supercomputing systems.

---

_Comment by @AlexWaygood on 2025-11-30 11:16_

> I still think that "maybe `./src` should not be first-party" is the wrong framing: it must be first-party, because the most important aspect of "first-party" is checked-by-default (vs just "available as import source"), and `./src` should be checked by default.

I don't see how changing `./src` to become a third-party search path would make any difference here. Even if we accept the premise that the question of which files we should check by default should be decided by whether those files are relative to any first-party search paths, all files in `./src` would still be checked by default, because `.` would still be added as a first-party search path, and all files relative to `./src` are also relative to `.`.

Complicating this issue is the fact that we currently actually give `./src` _priority_ over `.` in our search-path ordering. The reason for this is to workaround another unfortunate behaviour we currently have. Currently we resolve relative imports by resolving the relative import into an absolute module name relative to the first search path we find that works, and then doing an absolute import of that module name. If `.` is given higher priority than `./src`, that means that we often end up resolving the module names of relative imports as `src.a.b` rather than `a.b`. (It's possible that this doesn't actually cause many issues in terms of the types we infer, but I think it leads to very confusing diagnostic messages at least!) If we emulated the runtime treatment of relative imports more closely (just treat them as relative to the importing file, don't involve search paths at all), then we might be able to put the priority of `./src` below `.` (like it is at runtime), and put the `PYTHONPATH` search paths in between the two.

---

_Comment by @carljm on 2025-12-02 00:28_

> My "installed" libraries are only available via `PYTHONPATH`, and mypy finds them. Although mypy may not have any explicit handling of `PYTHONPATH`, if it tries to import packages, then it respects `PYTHONPATH`.

I see - my local test was confused by the fact that mypy also applies its `py.typed` requirement to packages found on `PYTHONPATH`. So packages on `PYTHONPATH` are only importable if they contain `py.typed`.

> Many from-source packages managers like Nix, Guix, Spack, and EasyBuild install all packages into unique hashed directories. This allows, e.g., installing 2 different versions of numpy with 4 different BLAS/LAPACK libraries and 3 different compilers. These package managers rely entirely on `PYTHONPATH`

Thanks, I didn't realize this had become a popular technique for Python packages again. It used to be popular years ago with project management tools like `zc.buildout`.

> changing `./src` to become a third-party search path

I can see an argument that _if_ we see that in fact `./src` is anyway added to `sys.path` by an editable install in the Python environment, and isn't explicitly listed in `environment.roots`, in that scenario we would avoid auto-adding it to `environment.roots`. In this case we know it ends up on `sys.path` via an editable install, so we can get more accurate results by just letting it be handled that way, and not auto-adding it to `environment.roots`.

I guess the other possibility you might be proposing is that we never auto-add `./src` to `environment.roots` and instead always auto-add it to site-package paths if it exists? That seems more iffy to me, since the fact that `src/` exists in your current directory really isn't sufficient information to tell us that it is placed on `sys.path` by an editable install. And assuming that seems extra questionable in the case where we have a Python environment and can say conclusively that it doesn't contain an editable install for `src`.

I also agree that we should bump the priority of `PYTHONPATH` below first-party code, as suggested above.




---

_Added to milestone `Stable` by @carljm on 2025-12-02 00:28_

---

_Comment by @carljm on 2025-12-02 17:36_

> If we emulated the runtime treatment of relative imports more closely (just treat them as relative to the importing file, don't involve search paths at all)

One other note here for posterity, thought it's veering off-topic for this issue: "just treat them as relative to the import file" is not an accurate description of what the runtime does when resolving relative imports. If it were, it would be impossible to get the "attempted relative import with no known parent package" error that you can get at runtime if you use too many dots for your current module's nesting level.

The _way_ that the runtime takes search paths into account is different from how we currently do, though, specifically this part:

> relative to the first search path we find that works

Instead, at runtime modules already "know" (via `__package__` or `__spec__` or `__name__`) what their own fully qualified module name is; this is set when the module is imported. This knowledge effectively "bakes in" the search path entry that was used to import the module, so the runtime doesn't need our reverse-resolution from file to module name at all. But the rest of how relative imports work at runtime is the same as what ty does: the current module's module name, plus the relative import module path, is used to resolve to an absolute module name, which is then imported as as an absolute import. There is no part of Python's import system that resolves relative imports directly via relative filesystem locations (if it did, this would break the abstraction of Python's import system, which supports arbitrary import loaders, including loading from non-filesystem locations.)

It's not clear to me if or how we can move closer to the runtime system. It seems like it would require a fairly fundamental re-architecting to allow a single File to be two different modules, with different module names (as can happen at runtime with overlapping search paths!), and analyze them separately. This doesn't seem like a good idea.

> I think it leads to very confusing diagnostic messages at least

Note also that even if we did resolve relative imports just via relative filesystem locations directly, it _still_ wouldn't help with the "confusing diagnostic messages" issue at all. Once we've resolved the file which is the target of the relative import, we would still need to decide what its module name is, and if we used our current reverse-resolution system to do that, we'd reach the same conclusion that our current relative-import system reaches.

---

_Comment by @carljm on 2025-12-02 19:43_

Created https://github.com/astral-sh/ty/issues/1730 to track the latter issue.

---
