```yaml
number: 13833
title: "Build backend: Support namespace packages"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-namespaces
created_at: 2025-06-04T13:25:29Z
updated_at: 2025-06-19T10:54:12Z
url: https://github.com/astral-sh/uv/pull/13833
synced_at: 2026-01-12T16:10:53Z
```

# Build backend: Support namespace packages

---

_@konstin_

Unlike regular packages, specifying all `__init__.py` directories for a namespace package would be very verbose There is e.g. https://github.com/python-poetry/poetry/tree/main/src/poetry, which has 18 modules, or https://github.com/googleapis/api-common-protos which is inconsistently nested. For both the Google Cloud SDK, there are both packages with a single module and those with complex structures, with many having multiple modules due to versioning through `<module>_v1` versioning. The Azure SDK seems to use one module per package (it's not explicitly documented but seems to follow from the process in https://azure.github.io/azure-sdk/python_design.html#azure-sdk-distribution-packages and https://github.com/Azure/azure-sdk-for-python/blob/ccb0e03a3de748f3aabf44be94776ba37e55791f/doc/dev/packaging.md).

For simplicity with complex projects, we add a `namespace = true` switch which disabled checking for an `__init__.py`. We only check that there's no `<module_root>/<module_name>/__init__.py` and otherwise add the whole `<module_root>/<module_name>` folder. This comes at the cost of `namespace = true` effectively creating an opt-out from our usual checks that allows creating an almost entirely arbitrary package.

For simple projects with only a single module, the module name can be dotted to point to the target module, so the build still gets checked:

```toml
[tool.uv.build-backend]
module-name = "poetry.core"
```

## Alternatives

### Declare all packages

We could make `module-name` a list and allow or require declaring all packages:

```toml
[tool.uv.build-backend]
module-name = ["cloud_sdk.service.storage", "cloud_sdk.service.storage_v1", "cloud_sdk.billing.storage"]
```

Or for Poetry:

```toml
[tool.uv.build-backend]
module-name = [
    "poetry.config",
    "poetry.console",
    "poetry.inspection",
    "poetry.installation",
    "poetry.json",
    "poetry.layouts",
    "poetry.masonry",
    "poetry.mixology",
    "poetry.packages",
    "poetry.plugins",
    "poetry.publishing",
    "poetry.puzzle",
    "poetry.pyproject",
    "poetry.repositories",
    "poetry.toml",
    "poetry.utils",
    "poetry.vcs",
    "poetry.version"
]
```

### Support multiple namespaces

We could also allow namespace packages with multiple root level module:

```toml
[tool.uv.build-backend]
module-name = ["cloud_sdk.my_ext", "local_sdk.my_ext"]
```

For lack of use cases, we delegate this to creating a workspace with one package per module.

## Implementation

Due to the more complex options for the module name, I'm moving verification on deserialization later, dropping the source span we'd get from serde. We also don't show similarly named directories anymore.


---

_Label `enhancement` added by @konstin on 2025-06-04 13:25_

---

_Label `preview` added by @konstin on 2025-06-04 13:25_

---

_Comment by @zanieb on 2025-06-04 14:15_

I sort of like the idea of allowing people to enumerate all the expected members if `namespace = true` can be dangerous. I wonder if we should require it to start? I don't feel strongly though.

---

_Comment by @konstin on 2025-06-05 18:35_

From checking projects that use namespaces, no other build backend requires enumerating your modules, they allow pointing to the root and include whatever directory you've given, and I don't want to make the migration harder. I'm fine doing this more lenient approach for namespace packages particularly since you already opt-in to less coherence by using a namespace. I haven't done exhaustive testing, but with the validation we're performing for `tool.uv.build-backend.module-name` with a dotted path, I think we're already stricter and catch more problems than the other popular build backends.

---

_Marked ready for review by @konstin on 2025-06-05 18:35_

---

_Review requested from @BurntSushi by @konstin on 2025-06-05 18:35_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/settings.rs`:93 on 2025-06-06 16:17_

```suggestion
    /// Use this option when the namespace package contains multiple root `__init__.py`, for namespace
```

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/settings.rs`:136 on 2025-06-06 16:20_

So, this makes sense to me:

> I'm fine doing this more lenient approach for namespace packages particularly since you already opt-in to less coherence by using a namespace.

But one thing I'm a little iffy on here is coupling the notion of "this is a namespace package" and "we will disable most coherence checks." What if someone doesn't want to disable those coherent checks and list out the modules explicitly? I'm not necessarily asking for that to be available in this PR, but rather, is there a plausible path to that?

Maybe we'd keep this setup as-is, and you'd need to opt into a `coherent-checks = false` option to list out the modules? Or if a list is provided, then the coherent checks are automatically re-enabled?

So I think maybe this is all fine, but I just wanted to poke about this to check if this path was considered.

---

_Review comment by @BurntSushi on `docs/concepts/build-backend.md`:66 on 2025-06-06 16:21_

Is it feasible to give an example of something that will be accepted here that normally wouldn't be? Reading this with my beginner eyes on (as if I didn't just read this PR), I become immediately curious what kinds of checks have been disabled on me. :-) Should I be concerned? What if I want those checks enabled?

---

_@BurntSushi approved on 2025-06-06 16:25_

LGTM. I have some concerns mostly around the UX here. Specifically, I think the docs could be beefed up. And I'm unsure of the implications of coupling "declaration of this package as a namespace package" and "coherence checks are disabled."

My initial reaction matched @zanieb's here, but I'm swayed by your follow-up in terms of being more lenient (to make migration less painful and since this is what other build backends already do).

---

_@konstin reviewed on 2025-06-11 18:38_

---

_Review comment by @konstin on `crates/uv-build-backend/src/settings.rs`:136 on 2025-06-11 18:38_

I've improved the framing. The coherence checks I'm mentioning here are really just two things: Validating that the `module-name` matches our expectations for either either stubs or regular package, and checking that `__init__.py[i]` are absent above and present inside the root module. Both of those aren't super elaborate, but also something that other build backends usually don't do. With `namespace = true`, we turn those two checks off, so you can have arbitrary namespace structures. Setuptools, hatchling and poetry all support these kind of packages by default by having `include` declarations, we make this opt-in with `namespace = true`, and support a smaller, better checked subset suitable for many cases without it instead.

---

_@konstin reviewed on 2025-06-11 18:55_

---

_Review comment by @konstin on `docs/concepts/build-backend.md`:66 on 2025-06-11 18:55_

I've put a comparison into the reference docs. I don't want to go deep on `namespace = true` in the concept docs since I don't want to encourage complex namespaces.

---

_Merged by @konstin on 2025-06-12 17:23_

---

_Closed by @konstin on 2025-06-12 17:23_

---

_Branch deleted on 2025-06-12 17:23_

---

_@alanthian reviewed on 2025-06-19 03:25_

ok

---
