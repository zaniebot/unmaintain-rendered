```yaml
number: 693
title: "Support for `executionEnvironments` in monorepos"
type: issue
state: open
author: tim-x-y-z
labels:
  - configuration
assignees: []
created_at: 2025-06-24T09:14:14Z
updated_at: 2025-12-17T20:58:38Z
url: https://github.com/astral-sh/ty/issues/693
synced_at: 2026-01-10T01:53:59Z
```

# Support for `executionEnvironments` in monorepos

---

_Issue opened by @tim-x-y-z on 2025-06-24 09:14_

Hi team,

First of all, thank you for the excellent work on `ty`!

In our monorepo setup, we rely on `executionEnvironments` in `pyrightconfig.toml` to isolate type checking per service and control `extraPaths` in a scoped, declarative way. Here's a simplified version of our config:

```toml
[tool.pyright]
include = ["."]
extraPaths = ["shared/lib"]
executionEnvironments = [
  { root = "service-a", extraPaths = ["service-a/src", "shared/lib"] },
  { root = "service-b", extraPaths = ["service-b/src", "shared/lib"] },
  { root = "frontend", extraPaths = ["frontend/src", "service-a/src", "shared/lib"] }
]
```
This pattern makes it easy to work in a monorepo with multiple isolated services while keeping path resolution clean and explicit.
It would be incredibly helpful if `ty` supported something similar!

---

_Comment by @MichaReiser on 2025-06-24 10:34_

Thanks for the kind words. 

Can you tell us a bit more about your setup? 

* Do you use a single or multiple virtual environments?
* Is the main motivation to restrict imports between services (e.g., that `service-b` can't import a dependency that it shouldn't see?) and that the IDE doesn't suggest imports from packages that shouldn't be visible?
* Are there other reasons why you use this setup?

We've thought about this a bit in combination with uv's workspace support, but it may be useful to provide this also for users that don't use uv.



---

_Label `configuration` added by @MichaReiser on 2025-06-24 10:34_

---

_Comment by @tim-x-y-z on 2025-06-24 10:48_

Thanks for the follow-up!

I don't have the full context here but let me try to answer your questions:
- We do use uv, but we work mainly inside containers. That means we manipulate the PYTHONPATH directly for each service to get things working as expected.
- yes - it serves as a soft boundary mechanism; but then when building each service we only copy files from relevant packages.
- some services may have overlapping module names (e.g., both service-a and service-b define their own constants.py / models.py), so being able to control extraPaths per service lets us isolate resolution. This allows from constants import ... to work correctly within the context of each service.


---

_Comment by @MichaReiser on 2025-06-24 11:17_

> some services may have overlapping module names (e.g., both service-a and service-b define their own constants.py / models.py), so being able to control extraPaths per service lets us isolate resolution. This allows from constants import ... to work correctly within the context of each service.

Okay, that's interesting. Because it means it requires a different resolution priority for each service (based on what packages are on the search path). This is a bit more tricky because it means that resolving the module name `foo` can result in different files depending on in which file the import is.

Technically: What this requires is to parametrize `resolve_module` and `file_to_module` with the `search_path_settings` combined with some configuration that allows overriding the search path per package. 

---

_Comment by @AlexWaygood on 2025-06-24 11:35_

> Okay, that's interesting. Because it means it requires a different resolution priority for each service (based on what packages are on the search path). This is a bit more tricky because it means that resolving the module name `foo` can result in different files depending on in which file the import is.
>
> Technically: What this requires is to parametrize `resolve_module` and `file_to_module` with the `search_path_settings` combined with some configuration that allows overriding the search path per package.

I don't think this would produce consistent or accurate results when typechecking. If each package requires separate search-path settings, I think the only way to accurately type-check them would be to invoke ty separately on each package with different search-path settings. See also @erictraut's explanation [here](https://github.com/microsoft/pyright/discussions/7979#discussioncomment-9527473) of why he regrets adding the `executionEnvironments` feature to pyright.

I think the best way to solve this issue would be to provide a uv integration where uv invokes ty separately for each package in the monorepo, with different search-path settings for each package.

---

_Comment by @MichaReiser on 2025-06-24 11:49_

> I don't think this would produce consistent or accurate results when typechecking. If each package requires separate search-path settings, I think the only way to accurately type-check them would be to invoke ty separately on each package with different search-path settings. See also @erictraut's explanation https://github.com/microsoft/pyright/discussions/7979#discussioncomment-9527473 of why he regrets adding the executionEnvironments feature to pyright.

Isn't this different? The author isn't asking to allow customization of the Python version or platform. Only the extra-paths. 


> I think the best way to solve this issue would be to provide a uv integration where uv invokes ty separately for each package in the monorepo, with different search-path settings for each package.

That works for the CLI, but it's a bit trickier in the LSP use case. 

---

_Comment by @AlexWaygood on 2025-06-24 12:00_

> Isn't this different? The author isn't asking to allow customization of the Python version or platform. Only the extra-paths.

I think the principle is exactly the same! Extra paths are given top priority (higher priority than first-party, standard-library or third-party code) when it comes to module resolution. The presence or absence of extra paths could mean that an `import a` statement in a shared module `b` would be resolved to a totally different file. That could mean that all the types in module `b` are resolved totally differently depending on whether those extra paths are present or absent, if classes and functions from `a` are used heavily in `b`.

If package `foo` is meant to be checked with some extra paths and package `bar` isn't meant to be checked with those extra paths but they both import `b`, there's no way we could provide an accurate type-checking result for both `foo` and `bar` in a single run of ty with our current model, I don't think. We'd essential have to resolve all types for `b` twice, once "from the perspective of `foo`" (with the extra paths) and once "from the perspective of `bar`" (without the extra paths).

---

_Comment by @MichaReiser on 2025-06-24 12:13_

I agree that it can lead to inconsistencies. 

I think the main challenge here is how to enable this in the LSP and the CLI while giving a consistent experience. 

One option is that users should configure ty at the project level instead and that the LSP finds all ty projects in a workspace (unlike today, where it finds at most one). This requires a mechanism for inheriting configurations. 

Regarding the CLI: uv could run ty on every project but that's very wasteful for projects that don't need this isolation (we end up checking the same modules over and over again). So I think we havet to do better than this.

Related to https://github.com/astral-sh/ty/issues/2410 





---

_Comment by @AlexWaygood on 2025-06-24 12:20_

> Regarding the CLI: uv could run ty on every project but that's very wasteful for projects that don't need this isolation (we end up checking the same modules over and over again). So I think we havet to do better than this.

I agree not all monorepos require per-package isolation. But features such as per-package extra-paths or per-package Python version configuration only make sense in the context of per-package isolation, I think. Ideally CLI users would be able to choose between a fast type-checking experience where ty checks all packages all at once (without per-package isolation) and an experience where they're able to provide per-package configuration options like these (which I think necessitates a separate invocation of ty for each package if we want to provide accurate, consistent type-checking).

---

_Comment by @MichaReiser on 2025-06-24 12:23_

This definetely needs some design ;)

What I find important to point out is that we do want per-package isolation per-default when it comes to third party imports for users using uv and this doesn't require multiple runs because: ty can do an extra check *after* module resolution whether this import is allowed in this package (because it has a dependency on that package). If not, then ty can raise a specific diagnostic code saying that the module was found but the package isn't allowed to depend on it.

---

_Comment by @AlexWaygood on 2025-06-24 12:28_

Not sure I understand your second sentence there, but I definitely agree with your first 😄

---

_Renamed from "Support for `executionEnvironments` in monorepo's" to "Support for `executionEnvironments` in monorepos" by @AlexWaygood on 2025-06-25 14:57_

---

_Comment by @Mokto on 2025-12-17 20:58_

Not sure it helps but the way it's configured with `pyright` is the `executionEnvironments` option. 

```

executionEnvironments = [
    { root = "clustering/src", pythonVersion = "3.12", extraPaths = [ "extra/common" ] },
    { root = "search/src", pythonVersion = "3.12", extraPaths = [ "extra/common" ] }
]
```

---
