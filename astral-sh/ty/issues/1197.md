```yaml
number: 1197
title: Ty fails on non-existing extra-paths
type: issue
state: closed
author: GarbenTangheVintecc
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-09-17T12:42:52Z
updated_at: 2025-10-10T15:55:37Z
url: https://github.com/astral-sh/ty/issues/1197
synced_at: 2026-01-10T02:06:25Z
```

# Ty fails on non-existing extra-paths

---

_Issue opened by @GarbenTangheVintecc on 2025-09-17 12:42_

### Summary

### Context

I have a project which might run in 1 of 2 Docker containers:

* Ubuntu 22 with ROS2 Humble & Python 3.10
* Ubuntu 24 with ROS2 Jazzy & Python 3.12

The relevant part of my configuration is as follows:

```toml
# pyproject.toml
[tool.ty.environment]
extra-paths = [
    "/opt/ros/humble/lib/python3.10/site-packages",
    "/opt/ros/humble/local/lib/python3.10/dist-packages",
    "/opt/ros/jazzy/lib/python3.12/site-packages",
    "/opt/ros/jazzy/local/lib/python3.12/dist-packages",
]
```

Depending on the image, the first or the last 2 paths will exist inside the container.

### Problem

Ty fails because at least 1 of these paths does not exist. I think it would make more sense to print a warning and continue searching in the remaining directories.

### Minimal reproducible example

```bash
ty check --extra-search-path "non_existing/path"
# ty failed
#   Cause: non_existing/path does not point to a directory
```

_I am also a bit confused as to why the name of the argument is `extra-search-path` in the CLI & `extra-paths` in the configuration file._

### Version

ty 0.0.1a20

---

_Comment by @carljm on 2025-09-17 15:01_

Yes, I agree that having a nonexistent extra-path in your config should not be a fatal error, just a warning.

---

_Label `bug` added by @carljm on 2025-09-17 15:01_

---

_Label `configuration` added by @carljm on 2025-09-17 15:01_

---

_Comment by @MichaReiser on 2025-09-17 15:08_

I'd like to hear @AlexWaygood opinion here before we make any change. The concern here is that invalid configurations may be destructive when ty starts applying fixes. 

It also seems somewhat annoying to have warnings that are "okay" to ignore.

---

_Comment by @GarbenTangheVintecc on 2025-09-17 21:04_

@MichaReiser, I agree that my suggested warning could be suppressed.

I'm not quite sure how ty would apply destructive fixes here. Doesn't it only touch project src modules, and not the ones defined in the environment by extra-paths?

To clarify, the Python modules under the ros folders are third-party dependencies of my project.


---

_Comment by @AlexWaygood on 2025-09-17 21:17_

As a user, I don't think I'd want ty to guess about my intention if I made a mistake in my configuration. I think I'd want to have ty immediately exit and tell me that I probably made a typo.

I'm confused about two things in this issue report:
1. Why is the `extra-paths` setting being used to point to `site-packages`? Extra paths come before even the vendored stdlib stubs in module resolution; you shouldn't generally use `--extra-path` to point to the `site-packages` directory in your virtual environment. It would be better to use `--python` to add these paths to ty's list of search paths.
2. Why is it necessary to have these extra search paths be encoded in ty's persistent configuration (pyproject.toml)? That seems like an odd choice if you need the extra paths to be different in different invocations of ty -- normally you'd use CLI flags or environment variables for the "dynamic" bits of your configuration that can vary depending on the situation in which a tool is invoked. Would a viable solution here be to pass these extra search paths to ty using CLI flags -- could you use `--extra-search-path="/opt/ros/humble/local/lib/python3.10/dist-packages"` in the dockerfile for one image and `--extra-search-path="/opt/ros/jazzy/local/lib/python3.12/dist-packages"` in the dockerfile if you're using the other image?

---

_Label `bug` removed by @MichaReiser on 2025-09-18 06:39_

---

_Label `needs-decision` added by @MichaReiser on 2025-09-18 06:39_

---

_Comment by @GarbenTangheVintecc on 2025-09-18 15:05_

@AlexWaygood, I did not set `--python` but `VIRTUAL_ENV` is pointing explicitly to the virtual environment, according to:
> If [`--python` is] not specified, ty will attempt to infer it from the `VIRTUAL_ENV` or `CONDA_PREFIX` environment variables, or discover a `.venv` directory in the project root or working directory.

It is not possible to add multiple paths to the `--python` argument, right?

The ROS packages are installed in the Docker image before adding uv and install project dependencies, so they do not show up with `uv pip list`. At runtime they can be found because the site/dist-packages are in `PYTHONPATH`.

---

_Comment by @AlexWaygood on 2025-09-18 15:10_

> It is not possible to add multiple paths to the `--python` argument, right?

Not currently, but I'd be willing to consider that feature request! However...

> The ROS packages are installed in the Docker image before adding uv and install project dependencies, so they do not show up with `uv pip list`. At runtime they can be found because the site/dist-packages are in `PYTHONPATH`.

We're just about to add support for reading `PYTHONPATH` (https://github.com/astral-sh/ruff/pull/20441) so maybe that helps your use case?

---

_Comment by @GarbenTangheVintecc on 2025-09-18 15:26_

That would help my case! 🎆 

_Please note that the existence of a directory in the `PYTHONPATH` is [optional](https://github.com/astral-sh/ruff/pull/20441/files#diff-be0c7f11f9a2dc135ba797638026917e6316512e0a456a3a300370f99b0eec19R298), while for `extra_paths` it is required. So even though my environment variable only lists the 2 correct folders, it could contain all 4 and still succeed._

---

_Comment by @AlexWaygood on 2025-10-06 15:46_

We've now added support for `PYTHONPATH`, and we've also updated the `extra-paths` documentation to be clearer that it's not intended for you to point to a virtual environment using this option. I'd still be open to allowing `python` to take a list of paths, but IIUC adding support for `PYTHONPATH` fixes this specific issue. So let's wait on that until there's a need for it.

We still need to improve our docs for `PYTHONPATH`, but that's tracked in https://github.com/astral-sh/ty/issues/1219

---

_Closed by @AlexWaygood on 2025-10-06 15:46_

---

_Comment by @GarbenTangheVintecc on 2025-10-07 13:02_

If I want to test this, do I need to wait for the next release (`ty==v0.0.1.a22`), or is this available with the latest Ruff version (`ruff==v0.13.3`)? I'm confused because ty is being developed in the ruff submodule.

---

_Comment by @AlexWaygood on 2025-10-07 13:09_

Sorry for the confusion. You'll need to wait for the next release -- our last release was on September 19th, and support for `PYTHONPATH` was merged on September 20th. We should definitely be cutting a release this week -- sorry that it's been a while!

---

_Comment by @GarbenTangheVintecc on 2025-10-07 13:24_

No problem! I will keep an eye on the releases and come back to this issue if needed.

---

_Comment by @GarbenTangheVintecc on 2025-10-10 15:54_

@AlexWaygood, works like a charm with `ty==v0.0.1.a22`!
I could remove all `extra-path` entries, since the ROS modules are found through `PYTHONPATH`.

---

_Comment by @AlexWaygood on 2025-10-10 15:55_

Great to hear @GarbenTangheVintecc, thanks!

---
