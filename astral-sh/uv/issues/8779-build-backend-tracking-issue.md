```yaml
number: 8779
title: Build backend tracking issue
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - preview
  - tracking
  - build-backend
assignees: []
created_at: 2024-11-03T18:30:04Z
updated_at: 2025-07-03T17:36:34Z
url: https://github.com/astral-sh/uv/issues/8779
synced_at: 2026-01-12T15:59:35Z
```

# Build backend tracking issue

---

_@konstin_

uv should provide its own build backend. This issue tracks this work, it will become more granular as more things are implemented.

The uv build backend is currently in preview. It is used with `uv init --lib --preview`, and has a direct build fast path in `uv build --preview`.

Documentation:
 * Configuration: https://docs.astral.sh/uv/configuration/build-backend/
 * Reference: https://docs.astral.sh/uv/reference/settings/#build-backend

## TODO

- [x] Support all pyproject.toml METADATA
- [x] Support PEP 639
- [x] Build basic wheels
- [x] Build basic source distributions https://github.com/astral-sh/uv/pull/9013
  - [x] Include files
  - [x] Static metadata
- [x] Migrate to globset https://github.com/astral-sh/uv/pull/9013
  - [x] Decide on glob syntax: PEP 639 globs with extensions
  - [x] Use walkdir plus a custom short cutting strategy to avoid e.g. traversing the .venv
- [x] Test source tree -> source dist -> wheels https://github.com/astral-sh/uv/pull/9091
- [x] Include mandatory files: Readme, licenses, (others?) https://github.com/astral-sh/uv/pull/9149
- [x] Support data directories in wheels https://github.com/astral-sh/uv/pull/9197
- [x] Editable wheels https://github.com/astral-sh/uv/pull/9230
- [x] Allow exclusions for wheels https://github.com/astral-sh/uv/pull/9525
- [x] Warn when we are traversing too many files https://github.com/astral-sh/uv/pull/9523
- [x] Study exclude and include syntax from poetry, pdm, hatchling, cargo and npm
- [x] Search for include/exclude usage in the wild
- [x] Ensure that direct wheel building can't include files we wouldn't include in the source dist https://github.com/astral-sh/uv/pull/9525
- [x] Direct build mode that doesn't go through PEP 517 if the build backend is uv for `uv build` https://github.com/astral-sh/uv/pull/9556
- [x] Add command to list files that would be included https://github.com/astral-sh/uv/pull/9610
- [x] Direct build mode that doesn't go through PEP 517 if the build backend is uv for dependencies https://github.com/astral-sh/uv/pull/9556
- [x] Add a no fast path config option https://github.com/astral-sh/uv/pull/9600
- [x] Add the uv build backend in init through `uv init --preview --build-backend uv` (https://github.com/astral-sh/uv/issues/3957#issuecomment-2518757563) https://github.com/astral-sh/uv/pull/9661
- [x] Split the build backend out into its own package `uv_build`
- [x] https://github.com/astral-sh/uv/pull/13171
- [x] Documentation
- [x] Patterns for inclusions and exclusions: This is one of the most complex parts and will evolve in iterations
	- [x] https://github.com/astral-sh/uv/pull/13313
- [x] Preview release https://github.com/astral-sh/uv/pull/12804
- [x] Support stubs packages https://github.com/astral-sh/uv/pull/13563
- [x] Namespace package support https://github.com/astral-sh/uv/pull/13833
- [x] Stable release


# Background reading

## Including and excluding files in packages

To go from the source code to an installed (or published) package, we always start with a source tree (e.g. a directory in a repo, mostly the repo root), which is identified by containing a `pyproject.toml` with a `build-system` and a `project` section. To install the project, we always have to go through a wheel. Getting to the wheel is the task of the build backend, installing the wheel is a different part of uv (a well-defined one). From the source tree, we can then either build a source distribution and from that wheel, or directly a wheel. This means that the source distribution must contain all files needs for the wheel.

The source dist has an indirection where there’s a `<name>-<version>` directory at the root and everything is below it, but that’s an implementation details. For our purposes, source dists and wheels have a root directory we can add to.

A source distribution usually contains a subset of the source tree in its root, excluding generated and cache directories (`.venv`, `.pytest_cache`, etc.) and development files (`tests`, test data, CI, etc.), while including the main python module, certain metadata files (`pyproject.toml`, readme and its images, licenses) and crucial data files and blobs (sample dataframes in pandas, manylinux json in maturin, lists of known endpoints, db schemas, headers for c projects, launcher scripts, etc.) that may either live next to the source code or in one of the dedicated data dirs below.

PEP 639 defines license file globs such as `project.license-files = ["third-party/LICEN[CS]E*", "AUTHORS*"]`, which we must support as given. These files have to be copied to the root for the source dist, and to `<name>-<version>.dist-info/licenses` in the wheel. In the source dist, we have to include the readme if linked from `project.readme`, in the wheel it becomes part of METADATA.

Our main module usually exists at `src/<name>` , or alternatively at `<name>`. For the `src/<name>` layout, it needs to move to `<name>` in the wheel (recursive directory copy). This directory may contain python source files, files used by the source files (say some json with endpoints or a db schema sql) and files that we should skip such as `.pyc` and `__pycache__`.

Wheels (but not source dists) allow data directories in `<name>-<version>.data/<type>` , five different predefined ones. We have to allow the user to define which directory/files to include here, and then also copy those to the source dist.

A special case are native modules (`.so`/`.pyd`), if we want to support them. These may exist in the source tree for development (esp. editables), but must not be copied to the source dist, but must be generated from the source dist and added to the wheel.

We may want to allow the user to include different files in source tree → source dist than in source tree (repo or unpacked from a source dist) → wheel, especially when a build or code generation step becomes involved.

At the top level, the wheel must only contain a single module (we don’t support wheels with more than one top level module), so there are no custom include patterns for wheels: The wheels contains dist info (including license files), data files and (potentially with exclusions) the root module directory.

Even through all this, the majority of projects will want three features: A Readme (potentially with a transform for pypi), license file(s) and a `src/<name>` directory. These can be covered by the right defaults, so most users shouldn’t need to change the default includes/excludes.

## Tool Review

### poetry

By default, uses gitignore. Using `include` makes it ignore gitignore.

```toml
[tool.poetry]
include = [
    { path = "tests", format = "sdist" },
    { path = "for_wheel.txt", format = ["sdist", "wheel"] }
]
```

If no format is specified, `include` defaults to only `sdist`.

In contrast, `exclude` defaults to both `sdist` and `wheel`.

### pdm

`includes` (wheel), `source-includes` and `package-dir`.

If a file is covered by both `includes` and `excludes`, the one with the more path parts and less wildcards in the pattern wins, otherwise `excludes` takes precedence if the length is the same.

For example, given the following configuration:

```toml
includes = ["src"]
excludes = ["**/*.json"]
```

`src/foo/data.json` will be **excluded** since the pattern in `excludes` has more path parts, however, if we change the configuration to:

```toml
includes = ["src", "src/foo/data.json"]
excludes = ["**/*.json"]
```

the same file will be **included** since it is covered by `includes` with a more specific path.

Test files under `tests`, if found, are included by sdist and excluded by other formats.

`*.pyc`, `__pycache__/` and `build/` are always excluded.

### hatch(ling)

Respects gitignore and hgignore by default, `ignore-vcs` to ignore.

Include, then exclude, every entry represents a [[Git-style glob pattern](https://git-scm.com/docs/gitignore#_pattern_format)](https://git-scm.com/docs/gitignore#_pattern_format), uses `pathspec.GitIgnoreSpec.from_lines` internally.

```toml
[tool.hatch.build.targets.sdist]
include = [
  "pkg/*.py",
  "/tests",
]
exclude = [
  "*.json",
  "pkg/_compat.py",
]
```

You can use the `only-include` option to prevent directory traversal starting at the project root and only select specific relative paths to directories or files. Using this option ignores any defined [`include` patterns](https://hatch.pypa.io/1.12/config/build/#patterns).

There is an `artifacts` option to include gitignored files.

There is a hardcoded set of excluded directories (`.git`, `__pycache__`, etc.) and files (`.DS_Store`).

There is a `skip-excluded-dirs` for performance and `only-include` for only traversing certain directories.

### maturin

Include and exclude are inspired by poetry.

```toml
include = [
  { path = "path/**/*", format = "sdist" },
  { path = "all", format = ["sdist", "wheel"] },
  { path = "for/wheel/**/*", format = "wheel" }
]
```

### scikit-build-core

Uses gitignore by default, you can specify sdist includes and excludes, and wheel excludes.

For packages, it supports renames (last component must match):

```toml
[tool.scikit-build.wheel.packages]
"mypackage/subpackage" = "python/src/subpackage"
```

### cargo

Include and exclude with gitignore syntax. By default, *all* files are included, not just `src`, but when specifying include manually, it will ignore `src` by default.

If `include` is not specified, then the following files will be excluded:

- If the package is not in a git repository, all “hidden” files starting with a dot will be skipped.
- If the package is in a git repository, any files that are ignored by the [[gitignore](https://git-scm.com/docs/gitignore)](https://git-scm.com/docs/gitignore) rules of the repository and global git configuration will be skipped.

Regardless of whether `exclude` or `include` is specified, the following files
are always excluded:

- Any sub-packages will be skipped (any subdirectory that contains a `Cargo.toml` file).
- A directory named `target` in the root of the package will be skipped.

The following files are always included:

- The `Cargo.toml` file of the package itself is always included, it does not need to be listed in `include`.
- A minimized `Cargo.lock` is automatically included if the package contains a binary or example target, see [`[cargo package](https://doc.rust-lang.org/cargo/commands/cargo-package.html)`](https://doc.rust-lang.org/cargo/commands/cargo-package.html) for more information.
- If a [`[license-file](https://doc.rust-lang.org/cargo/reference/manifest.html?highlight=exclude#the-license-and-license-file-fields)`](https://doc.rust-lang.org/cargo/reference/manifest.html?highlight=exclude#the-license-and-license-file-fields) is specified, it is always included.

### npm

There is a `files` list for includes with gitignore syntax, by default `.gitignore` is used but `.npmignore` takes precedence. There are mandatory includes, there are default excludes, and there are mandatory excludes.

## Ecosystem Review

A random assortment of projects and syntaxes as data points.

**boto3**

```
include CONTRIBUTING.rst
include README.rst
include LICENSE
include requirements.txt
recursive-include boto3/data *.json
```

**httpx**

```toml
[tool.hatch.build.targets.sdist]
include = [
    "/httpx",
    "/CHANGELOG.md",
    "/README.md",
    "/tests",
]
```

**charset-normalizer**

```
include LICENSE README.md CHANGELOG.md charset_normalizer/py.typed dev-requirements.txt
recursive-include data *.md
recursive-include data *.txt
recursive-include docs *
recursive-include tests *
```

**idna**

```toml
[tool.flit.sdist]
exclude = [".gitignore", ".github/"]
include = ["tests", "tools", "HISTORY.rst"]
```

**typing-extensions**

```toml
[tool.flit.sdist]
include = ["CHANGELOG.md", "README.md", "tox.ini", "*/*test*.py"]
exclude = []
```

**django**

```
include AUTHORS
include Gruntfile.js
include INSTALL
include LICENSE
include LICENSE.python
include MANIFEST.in
include package.json
include tox.ini
include *.rst
graft django
graft docs
graft extras
graft js_tests
graft scripts
graft tests
global-exclude *.py[co]
```

**pydantic**

```toml
[tool.hatch.build.targets.sdist]
# limit which files are included in the sdist (.tar.gz) asset,
# see https://github.com/pydantic/pydantic/pull/4542
include = [
    '/README.md',
    '/HISTORY.md',
    '/Makefile',
    '/pydantic',
    '/tests',
]
```

**scikit-learn**

Programmatically with meson, i think

**spacy**

```
recursive-include spacy *.pyi *.pyx *.pxd *.txt *.cfg *.jinja *.toml *.hh
include LICENSE
include README.md
include pyproject.toml
include spacy/py.typed
recursive-include spacy/cli *.yml
recursive-include licenses *
recursive-exclude spacy *.cpp
```

**auditwheel**

```
include README.rst
include LICENSE
include CHANGELOG.md
include src/auditwheel/policy/*.json
include src/auditwheel/_vendor/wheel/LICENSE.txt

graft tests

exclude .coveragerc
exclude .gitignore
exclude .git-blame-ignore-revs
exclude .pre-commit-config.yaml
exclude .travis.yml
exclude noxfile.py

prune .github
prune scripts
prune tests/**/__pycache__
prune tests/**/*.egg-info
prune tests/**/build

global-exclude *.so .DS_Store
```

**ripgrep**

```toml
exclude = [
  "HomebrewFormula",
  "/.github/",
  "/ci/",
  "/pkg/brew",
  "/benchsuite/",
  "/scripts/",
]
```

**alphafold3**

```toml
[tool.scikit-build]
wheel.exclude = [
    "**.pyx",
    "**/CMakeLists.txt",
    "**.cc",
    "**.h"
]
sdist.include = [
    "LICENSE",
    "OUTPUT_TERMS_OF_USE.md",
    "WEIGHTS_PROHIBITED_USE_POLICY.md",
    "WEIGHTS_TERMS_OF_USE.md",
]
```

**watchfiles**

---

_Label `enhancement` added by @konstin on 2024-11-03 18:30_

---

_Label `preview` added by @konstin on 2024-11-03 18:30_

---

_Assigned to @konstin by @konstin on 2024-11-03 18:30_

---

_Label `tracking` added by @samypr100 on 2024-11-04 02:16_

---

_Comment by @hauntsaninja on 2024-11-09 23:10_

Check MANIFEST.in could also be nice! https://github.com/mgedmin/check-manifest

---

_Comment by @cthoyt on 2024-12-04 13:44_

@konstin I was able to get my first builds working using this on the 0.5.5 release (also w/ tox and tox-uv)! There were a few tricks and rough edges I had to get around to make it work.

Are you interested in feedback at this point? I totally understand if not - you are probably aware of a lot of things already and I don't want to bog you down.

If so, what's the preferred mechanism? I am happy to open up issues with minimum reproducibility scripts where appropriate.

Grüssie aus Bonn

---

_Comment by @konstin on 2024-12-04 16:23_

With #9621, everything that's needed for alpha testing has now landed on main. I've updated the original post with some basic documentation.

tl;dr: Set `UV_PREVIEW=1` and add:

```
[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"
```

---

The functionality for the basic build backend and its integration has landed (all behind preview), the biggest item to still change is the include/exclude syntax, here we want to sync up with red knot and optimize towards typical project usage without loss of generality (see also the ecosystem review in the original post).

Currently not part of the design is generating files or any part of build step, such as cython compilation, code generation from schema, but also compiling native modules in C, C++ or Rust. The includes allow including extra files in the source dist for a future compilation or plugin step.

---

@cthoyt It's great to have early adopters! If you have a bug or specific feature request, just open a new issue, minimal reproductions are of course always greatly appreciated :) For general discussion on what the build backend should (or shouldn't) do and other design discussions, I'd use #3957, while I'll post general updates in this issue. Please share your feedback, i love hearing from users!

---

_Comment by @uwu-420 on 2024-12-13 12:04_

Really excited to see progress on uv's own build backend <3

I know this is currently out of scope, but I'd be interested if there are plans to eventually extend the build backend to handle building for multiple python versions in one go. Currently one would have to use tools like tox/no, cibuildwheel or a custom build script but I think it would be wonderful to only run `uv build` and call it a day. Of course with some previous configuration e.g. specifying the desired target versions.

Or am I missing the point what a build backend should handle or if there is already an idiomatic way to do this?

Edit: Okay I could call `uv build` multiple times with the `--python=...` argument which is good enough for the moment I guess :) But the question still remains.

---

_Label `build-backend` added by @konstin on 2025-01-14 08:44_

---

_Comment by @konstin on 2025-03-07 13:58_

In the latest release, we've split the build backend out into a separate `uv_build` package. The `uv` package itself doesn't contain the build backend anymore, I've updated the documentation in #8779 accordingly.

---

_Comment by @cthoyt on 2025-03-07 14:04_

@konstin thank you for the hard work! It seems that it's not so easy to just update the configuration, for example in my PR https://github.com/biopragmatics/pyobo/pull/364, I'm getting the error

```
Error: Unknown subcommand: build-backend (cli: ArgsOs { inner: ["/Users/cthoyt/dev/pyobo/.tox/.pkg/bin/uv-build", "build-backend", "build-sdist", "/Users/cthoyt/dev/pyobo/.tox/.pkg/dist"] })
Traceback (most recent call last):
  File "/Users/cthoyt/Library/Application Support/uv/tools/tox/lib/python3.13/site-packages/pyproject_api/_backend.py", line 93, in run
    outcome = backend_proxy(parsed_message["cmd"], **parsed_message["kwargs"])
  File "/Users/cthoyt/Library/Application Support/uv/tools/tox/lib/python3.13/site-packages/pyproject_api/_backend.py", line 34, in __call__
    return getattr(on_object, name)(*args, **kwargs)
           ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^
  File "/Users/cthoyt/dev/pyobo/.tox/.pkg/lib/python3.13/site-packages/uv_build/__init__.py", line 64, in build_sdist
    return call(args, config_settings)
  File "/Users/cthoyt/dev/pyobo/.tox/.pkg/lib/python3.13/site-packages/uv_build/__init__.py", line 48, in call
    sys.exit(result.returncode)
    ~~~~~~~~^^^^^^^^^^^^^^^^^^^
SystemExit: 1
Backend: run command build_sdist with args {'sdist_directory': '/Users/cthoyt/dev/pyobo/.tox/.pkg/dist', 'config_settings': None}
Backend: Wrote response {'code': 1, 'exc_type': 'SystemExit', 'exc_msg': '1'} to /var/folders/6k/r2gmmc213vl6h7bv20x624xw0000gn/T/pep517_build_sdist-uyoantdq.json
```

could this be because on PyPI, there's only uv_build 0.6.3 and not 0.6.5?


The following does work locally, but I'm wondering if it's because uv_build 0.6.5 is available because I have uv 0.6.5 installed?

```console
$ uv --preview init uvbbt --build-backend uv
Initialized project `uvbbt` at `/Users/cthoyt/Desktop/uvbbt`
$ cd uvbbt
$ cat pyproject.toml
[project]
name = "uvbbt"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "Charles Tapley Hoyt", email = "cthoyt@gmail.com" }
]
requires-python = ">=3.13"
dependencies = []

[project.scripts]
uvbbt = "uvbbt:main"

[build-system]
requires = ["uv_build>=0.6.5,<0.7"]
build-backend = "uv_build"
$ uv run uvbbt
Using CPython 3.13.1
Creating virtual environment at: .venv
      Built uvbbt @ file:///Users/cthoyt/Desktop/uvbbt
Installed 1 package in 3ms
Hello from uvbbt!
```

---

_Comment by @konstin on 2025-03-07 22:15_

Sorry, the package had the old Python shim still! Fixed in https://github.com/astral-sh/uv/pull/12058.

---

_Comment by @cthoyt on 2025-03-07 22:26_

> Sorry, the package had the old Python shim still! Fixed in [#12058](https://github.com/astral-sh/uv/pull/12058).

woohoo! sorry I also opened a dedicated bug report with a MRE in #12059 before I saw your comment. Hopefully that can help testing 🚀 

---

_Comment by @johnthagen on 2025-03-08 15:37_

@konstin Would it be clearer if the `[build-system]` docs/examples used `uv-build` rather than `uv_build`?

The name on PyPI is `uv-build`: https://pypi.org/project/uv-build/

From a Python-consumer perspective, I think using `uv-build` could remove some confusion.

---

_Comment by @konstin on 2025-03-10 09:05_

We considered a number of different ways of configuring the build backend (see also https://github.com/astral-sh/uv/pull/11446#issuecomment-2689508497). In the end, we stylized the package name in the documentation as `uv_build`, so that `build-system.requires` and `build-sytem.build-backend` use the same separator between `uv` and `build`. We want to avoid confusing users about why both `requires = ["uv_build>=0.6,<0.7"]` and `requires = ["uv-build>=0.6,<0.7"]` are allowed, but then `build-backend = "uv_build"` works while `build-backend = "uv-build"` fails with a `ModuleNotFoundError` by using the underscore everywhere. You are of course free to use the normalized version with the underscore. PyPI also implements the normalization, so https://pypi.org/project/uv_build/ redirects to https://pypi.org/project/uv-build/.

```toml
[build-system]
requires = ["uv_build>=0.6,<0.7"]
build-backend = "uv_build"
```

---

_Comment by @konstin on 2025-03-10 11:39_

To avoid splitting the conversation between two threads, I'll lock this issue in favor of #3957. I'll post update the top level post and post updates here, but all the discussion (that is not in separate issues) should happen in #3957.

---

_Locked by @astral-sh on 2025-03-10 11:40_

---

_Comment by @konstin on 2025-05-08 07:36_

Update from v0.7.3: The uv build backend now has both configuration and references documentation and is the default in preview (https://github.com/astral-sh/uv/pull/12804). Builds should now be reproducible between machines of the same platform and in most cases also independent of the  build platform (https://github.com/astral-sh/uv/pull/13171). We also added a way to escape characters in globs, extending over the PEP 639 syntax we previously used (https://github.com/astral-sh/uv/pull/13313).

---

_Comment by @konstin on 2025-06-13 12:30_

In 0.7.13, we added support for namespaces. For packages with a single root module, you can set `module-name` for `src/cloud/sdk/db/__init__.py`:

```
[tool.uv.build-backend]
module-name = "cloud.sdk.db"
```

You can also set `namespace = true` for complex namespace packages with more than one root module.

The build backend now also supports stubs packages, including for namespace packages:

```
[tool.uv.build-backend]
module-name = "cloud-stubs.sdk.db"
```

Check the documentation for details: https://docs.astral.sh/uv/concepts/build-backend/

The core of the build backend should be feature complete with those additions and unless we find problems with the configuration, ready for stabilization. Stabilization here doesn't mean that we won't add more feature, but that all core features are present and ready for general use.

---

_Comment by @konstin on 2025-07-03 17:36_

The uv build backend is now stable - ready for production - as of 0.7.19! See the docs on how to gets started: https://docs.astral.sh/uv/concepts/build-backend/

We will follow up with with problems and feature requests (please open issues!). We don't cover everything a uv build system could do in this release, you can feature requests under the [issues with the build backend tag](https://github.com/astral-sh/uv/issues?q=state%3Aopen%20label%3A%22build-backend%22), and, as usual, find new features in our release notes.

One change that's in the pipeline that I can already announce is that we'll make the uv build backend the default in `uv init` (which is currently still hatchling as it's a behavior change).

---

_Closed by @konstin on 2025-07-03 17:36_

---
