```yaml
number: 11502
title: how to add non-source files to package / build backends ?
type: issue
state: closed
author: dd-ssc
labels: []
assignees: []
created_at: 2025-02-14T06:34:28Z
updated_at: 2025-12-03T19:47:02Z
url: https://github.com/astral-sh/uv/issues/11502
synced_at: 2026-01-10T03:23:53Z
```

# how to add non-source files to package / build backends ?

---

_Issue opened by @dd-ssc on 2025-02-14 06:34_

### Question

I'm in the process of migrating all of my company's python projects from poetry to uv.

I don't have a lot of experience with uv yet, but so far, the [published speed charts](https://docs.astral.sh/uv) don't do it justice - uv is actually even faster: In some projects and scenarios, it takes poetry up to half an hour to create a virtual environment and install packages; uv typically does that in less than a second 🚀🚀🚀🤩.

The changes in `pyproject.toml` to migrate are somewhat straight forward and I can successfully build a package using simply `uv build` ✅

However, I could not find a way to configure non-source files that should go into the package created with `uv build`; a `.yaml` configuration file in my case. With poetry, I used to do that with e.g.

```
include = ['some_path/my_file.yaml']
```

While researching if there's a uv equivalent, I came across [this somewhat related post](https://github.com/astral-sh/uv/issues/9800#issuecomment-2537161658) that looks promising. I naively tried adding only

```
[tool.uv.build-backend.data]
data = 'some_path/my_file.yaml'
```

but that doesn't seem to have any effect. Then I realized I don't even have a `[build-system]` section in my `pyproject.toml` file yet. When I add

```
[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"
```

`uv build` fails with

```
AttributeError: Using `uv.build_sdist` is not allowed. The uv build backend requires preview mode to be enabled, e.g., via the `UV_PREVIEW=1` environment variable.
```

When I do `UV_PREVIEW=1 uv build`, the build fails with

```
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
  × Failed to build `<path to root folder of package source code>`
  ╰─▶ Expected a Python module with an `__init__.py` at: `<my home folder>/.cache/uv/sdists-v7/.tmpcf7V8p/<my package>-<package version/src/<root folder>`
```

The folder underneath `.cache` seems to exist during the build only, not afterwards 🤷🏻‍♂️

It seems I don't have the uv background knowledge to understand what's wrong here.
I would therefore greatly appreciate if someone could give me some pointers:

1. what build backend does uv use by default, i.e. if I don't have a `[build-system]` section in my `pyproject.toml` file ?
2. what is the generally recommended build backend ?
3. what build backend supports configuring additional files that should go into the package ?

Thank you very much for any help 🙏🏻


### Platform

Darwin 23.6.0 x86_64

### Version

uv 0.5.29 (Homebrew 2025-02-06)

---

_Label `question` added by @dd-ssc on 2025-02-14 06:34_

---

_Assigned to @konstin by @konstin on 2025-02-14 12:11_

---

_Comment by @konstin on 2025-03-03 18:45_

> what is the generally recommended build backend ?
> what build backend supports configuring additional files that should go into the package ?

If you're looking for something that just works, I'd recommend hatchling, our default with `uv init`. It has extensive configuration options (https://hatch.pypa.io/latest/config/build/).

For the uv build backend, what is project layout like and how does the rest of the `pyproject.toml` look like?

I'll quickly describe the two ways to add extra files to a wheel with the uv build backend.

The first option is to add them next to your Python files. This is the easiest option, you have your `src/foo` with an `__init__.py` and you also put the yaml file there. Being in this directory it is still scoped to the project when installing, and you can find its location as `Path(__file__).parent.join("my_file.yaml")` from the `__init__.py`.

The other option is to add a data directory. The data directories are special in that they get unpacked into the venv separately, in locations specified by the interpreter. The data/data directory gets unpacked into the root of the venv, it can e.g. be used for sharing files cross packages. The `[tool.uv.build-backend.data]` entries must point to directories, we should show at least a warning when they don't.

Note that both structures require a Python module structure: The missing `__init__.py` error is about the build backend not finding source code.

---

_Label `question` removed by @konstin on 2025-03-03 18:45_

---

_Comment by @dd-ssc on 2025-03-06 18:10_

Thank you very much for your response, that is certainly helpful information 👍

Knowing what build backend uv uses as default IMO is absolutely crucial; getting further info from the hatch docs then is trivial.

Two observations:

I have created two test projects with
1. `uv init`
2. `uv init --build-backend hatch`

which to my surprise did not result in identical projects.

Also, even when run with `--verbose`, `uv init` does not mention the build backend at all.

I don't think any of that info is documented anywhere, so I have amended the documentation: <https://github.com/astral-sh/uv/pull/12011>. (On a side note: I am a little confused: The file I changed is actually a `.md` MarkDown file, but it largely uses HTML (`<p>`, `<code>`, etc.) ?!? 🤷🏻‍♂️).

If that PR gets merged, we can probably close this issue.

---

_Comment by @dd-ssc on 2025-03-06 18:16_

OK, just got a notification that the PR checks failed; I had a feeling I'm probably modifying a generated file 🙈. I'll look into it.

---

_Comment by @dd-ssc on 2025-03-06 19:23_

I messed up the first PR https://github.com/astral-sh/uv/pull/12011; second attempt: https://github.com/astral-sh/uv/pull/12017.

---

_Unassigned @konstin by @konstin on 2025-03-07 14:31_

---

_Comment by @dd-ssc on 2025-03-09 10:57_

A quick summary for anyone coming here in the future:

1. Check if your `pyproject.toml` contains a `[build-system]` section; if it does not, add one:

```
[build-system]
build-backend  = 'hatchling.build'
requires       = ['hatchling']
```

2. See the [hatch docs](https://hatch.pypa.io/latest/config/build/#file-selection) for how to include files in the package.

We tried to use the [`force-include` option](https://hatch.pypa.io/latest/config/build/#forced-inclusion), but adding something like

```
[tool.hatch.build.targets.wheel.force-include]
../../local_path/my_file.yaml = 'path_in_package/my_file.yaml'
```

failed with

```
FileNotFoundError: Forced include not found: <home folder>/.cache/uv/sdists-v8/local_path/my_file.yaml
× Failed to build `<path to my project>`
```

so instead, we copy the additional files to underneath the source code root folder before `uv build` and delete them afterwards - which is what @konstin [describes as the first option](https://github.com/astral-sh/uv/issues/11502#issuecomment-2695259348).

---

_Closed by @dd-ssc on 2025-03-09 10:57_

---

_Comment by @justRishi on 2025-06-18 17:49_

For including `json -files`  I use something like this for `uv`, works fine:

```yaml
[tool.setuptools]
include-package-data = true

[tool.setuptools.package-data]
# add extensions or files that need to be in the package
"some-package" = ["*.json"]
# or define path in package where json 's are allowed.
#"some-package.util.resources = ["*.json"] 

[project]
name = "some-package-name"
....
source: https://setuptools.pypa.io/en/latest/userguide/datafiles.html#package-data

---

_Comment by @slhck on 2025-11-03 11:10_

For me it was enough to do:

```toml
[tool.uv_build]
src-layout = true
package-data = {"my_package" = ["data/**/*.json"]}
```

E.g.:

```
$ uv build                       
Building source distribution...
Building wheel from source distribution...
Successfully built dist/my_package-1.35.0.tar.gz
Successfully built dist/my_package-1.35.0-py3-none-any.whl

$ tar -tvf dist/my_package-1.35.0.tar.gz | grep data                                                                               
drwxr-xr-x  0 0      0           0 Jan  1  1970 my_package-1.35.0/src/my_package/data
drwxr-xr-x  0 0      0           0 Jan  1  1970 my_package-1.35.0/src/my_package/data/presets
-rw-r--r--  0 0      0          96 Jan  1  1970 my_package-1.35.0/src/my_package/data/presets/foo.json
```

---

_Comment by @shiftleft1 on 2025-11-10 19:50_

Simply follow https://setuptools.pypa.io/en/latest/userguide/datafiles.html#package-data


pyproject.toml
```toml
[tool.setuptools.package-data]
mypkg = ["schemas/**/*.json"]
```

---

_Comment by @davidbrochart on 2025-11-15 08:30_

From the [documentation](https://docs.astral.sh/uv/reference/settings/#build-backend_source-include):
```toml
[tool.uv.build-backend]
source-include = ["tests/**"]
```

---

_Comment by @gbrandt1 on 2025-12-03 10:28_

none of the above solutions work for me when using workspaces and the top-level is a virtual package.
ie. if i have the following the structure, copied and modified from the documentation:

albatross
├── packages
│   ├── bird-feeder
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── bird_feeder
│   │           ├── __init__.py
│   │           ├── bird-feeder-catalogue.json
│   │           └── foo.py
│   ...
├── pyproject.toml
├── README.md
└── uv.lock

the top-level pyproject.toml uses the current version of uv_build as build-system.
i've tried all suggestions to have the `bird-feeder-catalogue.json` file included in the bird-feeder wheel but nothing works.
what are the magic words for this case?

---

_Comment by @konstin on 2025-12-03 16:41_

@gbrandt1 Which `pyproject.toml` has a `[project]` and/or `[build-system]` table? It might be a problem with the rendering, but the `__init__.py` and `bird-feeder-catalogue.json` should be under `src/bird_feeder`, and bird feeder should be what is built, not the top level `pyproject.toml`.

---

_Comment by @gbrandt1 on 2025-12-03 19:47_

yes this source tree did not render nicely. the jsons were in the correct place. however:
the bird-feeder package did not have a build backend defined. it was my assumption that for workspaces the build backend should only be defined in the top level package. adding it now to bird-feeder as well solved the problem. 
of course you are right, what is getting built is bird-feeder. however, i made the top level virtual package depend on bird-feeder (and the other packages in the monorepo) and then tried to install (and thus transiently build) them by way of installing the top level package. this still does not work. but i'm not sure it is expected to, and it is a separate issue from this topic.

---
