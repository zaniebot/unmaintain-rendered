```yaml
number: 16066
title: How to enforce runtime version consistency for project build requirements?
type: issue
state: open
author: johnwalk-spotify
labels:
  - question
assignees: []
created_at: 2025-09-29T14:36:56Z
updated_at: 2025-10-03T16:06:07Z
url: https://github.com/astral-sh/uv/issues/16066
synced_at: 2026-01-12T16:02:22Z
```

# How to enforce runtime version consistency for project build requirements?

---

_@johnwalk-spotify_

### Question

We have a project with a number of build dependencies (specifically protobuf compilation) that must be kept consistent with the runtime versions of those dependencies.  This circumstance is pretty well-documented already in the [project config page](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation), but the examples there refer only to building the project's child dependencies, not the project itself.

We can get the build to work by setting `no-build-isolation-package` referring to the top-level project, but this feels overly aggressive. When instead trying to use the annotation introduced in https://github.com/astral-sh/uv/pull/15036, the build errors due to circular reasoning trying to resolve the local project, as it does not read as having static metadata available.

For building this project, then:
1. Is there a way for `extra-build-dependencies` annotations to support the parent package rather than just its child dependencies?
2. Should we instead just consider this project as not supporting build isolation and proceed with that?

### Platform

_No response_

### Version

0.8.22

---

_Label `question` added by @johnwalk-spotify on 2025-09-29 14:36_

---

_Comment by @zsol on 2025-09-29 16:01_

> the build errors due to circular reasoning trying to resolve the local project, as it does not read as having static metadata available

Could you elaborate a little bit more, please? An error log or, even better, a minimal repro would be amazing to look at!

I've been thinking about this problem for a while, and have an alternative way of matching build requirements to runtime versions, but it's sort of stalled because it's not clear to me if #15036 is good enough for people's needs or not. Having more information about when #15036 isn't good enough for actual use cases would help motivate that work.

FWIW, disabling build isolation is a good option to unblock yourself today, but might make things more complicated down the line.

---

_Comment by @johnwalk-spotify on 2025-09-29 16:08_

Passing along from the help channel on our internal slack, but the error triggered on `uv build` is 

```shell
Building wheel...
  × Failed to build `<path to local source for package>`
  ╰─▶ Extra build requirement `grpcio-tools` was declared with `match-runtime = true`, but `<project name>` does not declare static metadata, making runtime-matching impossible
```

`pyproject.toml` for the project itself is static and should be compliant, so I suppose reading from a local source still hits the check introduced in https://github.com/astral-sh/uv/pull/15292 ?

I can see about putting together a minimal repro as well

---

_Comment by @zanieb on 2025-09-29 16:16_

Hm I think that sounds like an unintentional side-effect of https://github.com/astral-sh/uv/pull/15292, cc @charliermarsh 

---

_Comment by @johnwalk-spotify on 2025-09-29 17:07_

yeah for a minimal repro, a fresh `uv init --lib build-test` then modifying pyproject.toml to 

```toml
[project]
name = "build-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "<name>", email = "<email>" }
]
requires-python = ">=3.13"
dependencies = [
    "numpy",
]

[build-system]
build-backend = "setuptools.build_meta"
requires = [
    "setuptools>=70.3.0",
    "wheel",
    "numpy",
]

[tool.uv.extra-build-dependencies]
build-test = [
    { requirement = "numpy" , match-runtime = true },
]
```

is sufficient to trigger the above error. This also occurs with the `uv_build` backend

---

_Comment by @charliermarsh on 2025-09-29 17:09_

Thanks, I'll take a look.

---

_Comment by @charliermarsh on 2025-10-01 00:51_

"Real" support here means we have to resolve the project dependencies, which... feels like a lot. I'm wondering if we should just ignore `match-runtime = true` in `uv build`? I'm not sure.

---

_Comment by @johnwalk-spotify on 2025-10-02 13:11_

> ignore match-runtime = true in uv build

at least in our use case I think that would result in generated code (protobufs in our case) from versions out of sync with runtime, no?  Effectively the same as just not constraining that requirement via `extra-build-dependencies` and just having them both in `build-system.requires` and `project.dependencies`.

---

_Comment by @johnwalk-spotify on 2025-10-02 13:16_

(he says knowing nothing at all about uv's internals, so feel free to disregard)

given the error ☝️ was on not finding project metadata statically for the parent package... it seems like all the information needed for `tool.uv.dependency-metadata` would be present in the local pyproject.toml.  Would it be sensical to detect the local project (maybe even workspace members?) as keys in `tool.uv.extra-build-dependencies` and inject metadata from the local pyproject.toml that way?

or does that only work for project dependencies rather than build requirements? 🤔 

---

_Comment by @johnwalk-spotify on 2025-10-02 13:21_

I suppose that doesn't really get around needing to resolve the project (though wouldn't that need to happen for any dependency with `match-runtime` set?)

---

_Comment by @johnwalk-spotify on 2025-10-03 16:06_

Yeah no good, manually adding 

```toml
[tool.uv.sources]
build-test = { path = "." }

[[tool.uv.dependency-metadata]]
name = "build-test"
requires-dist = ["numpy"]
```

doesn't resolve it so that path doesn't seem sufficient anyhow

---
