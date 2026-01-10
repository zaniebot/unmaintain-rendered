```yaml
number: 7429
title: "Best practices for `requires-python` "
type: issue
state: open
author: TheRealBecks
labels:
  - projects
  - needs-design
assignees: []
created_at: 2024-09-16T14:26:24Z
updated_at: 2025-07-25T18:04:49Z
url: https://github.com/astral-sh/uv/issues/7429
synced_at: 2026-01-10T03:32:44Z
```

# Best practices for `requires-python` 

---

_Issue opened by @TheRealBecks on 2024-09-16 14:26_

I had some thoughts about version handling and documentation in #7352, but that was out of scope, so I created this issue.

* ~I used the wrong version identifier in the first place which resulted to `3.12.0`~ -> #7426
* I did not expect `uv` to be stuck at `3.12.0` when defining `requires-python = "==3.12"` and changing it to `~=3.12`
* I think it's best to change the identifier in the documentation to `~=3.12` (or `==3.12.*`?) and describing what that means instead of using `>=3.12` which could lead to `3.13.x`
* Maybe pointing the interested user in a notice field to the [`Version specifier`](https://packaging.python.org/en/latest/specifications/version-specifiers/) documentation?



---

_Comment by @zanieb on 2024-09-16 14:29_

Using `>=3.12` is best practice for `requires-python` — it's generally not great to add upper version pins to the Python requirement of the project. Instead, you should use `uv python pin 3.12` to pin the version.

---

_Comment by @edmorley on 2024-09-16 20:56_

@zanieb So this actually overlaps with a topic that affects us quite a bit (both in general, and it's also come up again with the recent addition of Poetry support).

For libraries (or tools/software that is going to be distributed to a variety of end-user machines that needs to support multiple Python versions, such as CLIs, ...) supporting a range of Python versions in `requires-python` absolutely makes sense. 

However, there are a large number of projects (and I would posit a much larger group than the library/... case), for which the project only needs to support, and will likely only ever be capable of supporting (due to lack of CI coverage of all Python versions in a CI matrix), a single major Python version. 

For example, when deploying a Django app to a PaaS, the build system needs to pick a single version of Python for the project to install into the container and use to boot the app at launch. 

But what version should be picked if there is no `.python-version` file, and the `requires-python` field contains say `>=3.8`? Do we:
1. Always pick the latest GA Python (3.12)? However, that's not ideal, since the app could break when Python 3.13 is released.
2. On the first build, pick the current GA Python (3.12), store that in the per-app build cache, and then forever use the same major Python version with that app? However, that's not ideal, since it's hidden state/magic (ie: requires more explanation when the version reaches EOL and you have to tell the user to change a version they didn't even pick/set), and means inconsistency between local development, review apps, staging app and production apps, given state isn't shared.
3. Pick the lowest Python in the range (so 3.8)? However, that's still not ideal, since it's likely an old Python (particularly since people unlikely to remember to bump `requires-python` over time).
4. Fail the build until they add a `.python-version` file. However, errors aren't ideal for an onboarding experience.

Plus for all of the above apart from (4), there is no guarantee that the Python version picked by the build system will match the end users machine - or even that two developers from the same team are using the same Python version. This then leads to "well it works for me locally, but not on your platform" type support tickets.

The fact that `uv init` creates a `.python-version` file (with our preferred, major-version-only syntax) and also validates that the `.python-version` file's version is compatible with the range in `requires-python` (when running other commands) is already a big improvement over e.g. Poetry.

However, I imagine there will still be a number of uv-using projects that didn't use `uv init` and so don't have a `.python-version` file. 

As such, it would be great to either:
- Find ways to increase the adoption of `.python-version`. For example,  when locking warn if the file doesn't exist or even automatically create the file (ie: lock the Python version at the same time as the dependencies, which conceptually seems like it might fit?). There would of course need to be a way to opt out, and perhaps it could be limited to when the project was using "app" mode (disabled if `package = true` etc).
- Or, encourage the use of "safer" (and deploy/dev-prod parity friendly) `requires-python` values for "app" use-cases - such `==3.12.*` instead of `>=3.12`. (Whilst technically `~=3.12.0` would also count as "safe" from a "doesn't pull in major version changes" point of view, IMO the `~=` operator is too surprising, given that `~=3.12` and `~=3.12.0` are not equivalent - and so I would prefer to steer end-users away from it.)

Lastly, imagine the scenario where:
- someone runs `uv init` now for their project of type "app", giving a `.python-version` containing `3.12` and a `pyproject.toml` with a `requires-python` of `>=3.12`
- over the years they bump their `.python-version` version, but don't think about needing to update the `requires-python` version
- eventually Python 3.12 reaches EOL
- as the user later adds or updates packages, uv's resolver has to find packages that are still compatible with Python 3.12 (to meet the `requires-python` range), but this becomes increasingly hard over time
- this causes outdated packages, which is confusing for the user since they've requested a modern Python version in their `.python-version` file and may not even be aware about `requires-python` and the impact it has on package resolution.

In that scenario, it seems using a wide-version-range value for `requires-python` (such as the default of `>=3.X`) is a net-negative for UX for the "app" project type, even if a `.python-version` file is present. As such, perhaps nudging "app" project types towards a `requires-python` of e.g. `==3.12.*` would still be preferred? (Though an alternative might be for uv to output a warning if the `requires-python` range includes EOL Python versions perhaps?)

(See also https://github.com/heroku/buildpacks-python/issues/260 and https://github.com/python-poetry/poetry/issues/9668)

---

_Renamed from "Python Version Identification and Documentation" to "Best practices for `requires-python` " by @zanieb on 2024-09-16 22:10_

---

_Label `projects` added by @zanieb on 2024-09-16 22:11_

---

_Label `needs-design` added by @zanieb on 2024-09-16 22:11_

---

_Comment by @zanieb on 2024-09-16 22:25_

Thanks for the thorough comment @edmorley — I basically agree there are problems with using `>=` for unpublished packages. I think it'd be reasonable to use `==X.y.*` instead, but I am curious to hear from more of the uv team though.

See also https://github.com/astral-sh/uv/issues/4970

---

_Comment by @2-5 on 2024-09-17 11:10_

I have a related issue, and I'm not sure what's the best approach.

I have a modular monolith, where the same Python package/code can run as multiple roles - webserver or worker for example. Each role has different dependencies, and I'm using `project.optional-dependencies` for that:

```toml
[project]
requires-python = ">=3.11"
dependencies = [
    "prometheus-client>=0.20.0",
]

[project.optional-dependencies]
webserver = [
    "aiohttp>=3.9.5",
]
worker = [
    "orjson>=3.10.7",
]
```

If I want to run a particular checkout using the `worker` role I initialize the `.venv` like this: `uv sync --frozen --extra worker`

The two pain points I encountered:

1. there is no way to make `uv` remember how the `.venv` was initialized. If after the above command I run `uv sync`, it will not take into account that it was run previously with `--extra worker`, and it will change the installed packages
2. the different roles can have different Python version requirements. `webserver` is only compatible with 3.11, while `worker` also works with 3.12. This means I can't use a Git commited `.python-version`, since it would have to be different between the roles. So instead I do this `uv sync --frozen --extra webserver --python 3.11`

So I'm wondering if there are some best practices on how to work with separate sets of somewhat incompatible optional dependencies.

---

_Comment by @Seazs on 2024-09-23 12:39_

I have a similar issue with the usage of the following type of command:

```bash
uv python install '>=3.11'
```
With the latest pre-release Python version 3.13.0rc2 being available, if no other version is installed, uv will install 3.13.0rc2.

Would it not be better if uv installed the latest stable version (3.12.6) in that scenario?

As explained in [PEP440](https://peps.python.org/pep-0440/#version-specifiers), section **Handling of pre-release**,
"Pre-releases of any kind, including developmental releases, are implicitly excluded from all version specifiers, unless they are already present on the system, explicitly requested by the user, or if the only available version that satisfies the version specifier is a pre-release."

Also, I am new to creating issues or requests, so let me know if I am doing something wrong.

---

_Comment by @zanieb on 2024-09-23 13:25_

@Seazs thanks for the report, that's different than this issue — that's a bug. We'll track it in https://github.com/astral-sh/uv/issues/7637

---

_Comment by @tkukushkin on 2024-12-14 06:04_

Hi! I have a similar issue.

When developing libraries, I prefer to use minimal supported Python version. It's easier to control that I'm not using features of newer Python versions.

I also use tox with tox-uv to test libraries against all supported Python versions.

When I use .python-version with minimal supported version, tox-uv stops working properly. As I can see from the logs, uv recreates test environment because it doesn't match the pinned python version.

---

_Comment by @zanieb on 2024-12-14 15:26_

Hi @tkukushkin perhaps that's a tox-uv bug? It sounds like they should be supplying an explicit Python version to use during testing?

---

_Comment by @tkukushkin on 2024-12-14 16:10_

I saw the tox-uv issue about it (https://github.com/tox-dev/tox-uv/issues/110) but I just decided to show my use case here as well. Ideally I would not like to have .python-version at all. It just duplicates `requires-python` and I have to remember to keep them in sync.

---

_Comment by @zanieb on 2024-12-14 17:31_

Except it doesn't duplicate it — it serves a distinct purpose, e.g., see discussion in https://github.com/astral-sh/uv/issues/8247

You don't need to have a `.python-version` file if you don't want to, but then we'll use the _first_ Python version we find that is compatible with your `requires-python` range unless you request a different version.

---

_Comment by @tkukushkin on 2024-12-14 17:39_

I meant it duplicates in my use case. I never want them to be different.

Also I don't really understand what "first" means. Could you please clarify it? I've seen it in documentation, but it's not clear for me.

---

_Comment by @tkukushkin on 2024-12-14 17:49_

https://github.com/astral-sh/uv/issues/5609 looks like what I need.

---

_Comment by @ofer-pd on 2025-01-26 21:52_

Sorry to intrude on this issue, but I have a related noob question:

If I have a private project (company web app), isn't it redundant to have a `.python-version` file **and** a `pyproject.toml` file with the `project.requires-python` field defined?  Wouldn't it make sense to drop the `requires-python` field and simply rely on the `.python-version` file?

(I assume in the case of a public project, like Django, one would want to keep the `requires-python` field defined for the benefit of package managers.)

---

_Comment by @zanieb on 2025-01-28 17:50_

@ofer-pd that's addressed a few comments up https://github.com/astral-sh/uv/issues/7429#issuecomment-2543194925

If you're publishing a package, then you should definitely still use a `requires-python` field.

---

_Comment by @ofer-pd on 2025-01-28 19:16_

@zanieb Thanks for the response - and for linking to #8247.  I found this comment helpful:

> The .python-version file is not quite obsolete with requires-python -- they're subtly different. requires-python is the range of versions supported by your project. .python-version is the exact version you want to use when developing. For example, if you're working on a library, your requires-python might be >= "3.10". But when developing on your machine, you might want to use 3.12.4 by default -- so you'd set that in a .python-version.

https://github.com/astral-sh/uv/issues/8247#issuecomment-2416680597

---

_Comment by @ssbarnea on 2025-07-25 18:04_

I need to tell uv to do something like use, python 3.9 `>=3.9.17` or 3.10 `>= 3.10.13` or just `>=3.11` but I am afraid that there is no way to tell it this right now.

Note that this might not be the version needed by the package, is the version of python I want uv to install as I want to be sure that some of the known problematic outdated versions are to be avoided during testing. Any ideas?

---
