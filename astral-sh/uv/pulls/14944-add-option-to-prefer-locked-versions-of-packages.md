```yaml
number: 14944
title: Add option to prefer locked versions of packages in build environments
type: pull_request
state: closed
author: zanieb
labels:
  - enhancement
  - preview
assignees: []
draft: true
base: main
head: zb/lock-extra-build-dependencies
created_at: 2025-07-28T19:04:52Z
updated_at: 2025-08-07T13:50:11Z
url: https://github.com/astral-sh/uv/pull/14944
synced_at: 2026-01-10T06:44:33Z
```

# Add option to prefer locked versions of packages in build environments

---

_Pull request opened by @zanieb on 2025-07-28 19:04_

Adds a new `build-dependency-strategy = latest | prefer-locked` setting. `latest` is the current behavior. `prefer-locked` will prefer to use locked versions of runtime dependencies for build dependencies when they match. This is intended to work with #14735 to allow adding `torch` as an extra build dependency for `flash-attn` and prefer using the same version as you would at runtime (that's what `flash-attn` requires, but it's usually achieved by disabling build isolation).

The name is `prefer-locked` because I want to add a future option... like `require-locked` which will _error_ if the versions matching the runtime versions cannot be used. The naming here is actually a little awkward, because it'll be confusing when we introduce locking of build dependencies _separately_ from runtime requirements (which is on the roadmap). We might want to do... `latest | try-match-runtime | match-runtime`? I'm not sure. I also expect that we'll want a package-level variant of this setting, as `match-runtime` might be too aggressive for most packages. I also find this setting a little awkward alongside #14735, I wonder if we want a dedicated build dependencies section?

---

_Renamed from "Prefer locked versions of packages in build environments" to "Add option to prefer locked versions of packages in build environments" by @zanieb on 2025-07-28 21:24_

---

_Label `enhancement` added by @zanieb on 2025-07-28 21:24_

---

_Label `preview` added by @zanieb on 2025-07-28 21:24_

---

_Review requested from @konstin by @zanieb on 2025-07-29 02:20_

---

_Review requested from @charliermarsh by @zanieb on 2025-07-29 02:20_

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:11557 on 2025-07-29 09:38_

Can you add a case where the build requires conflict with the lockfile version, so we ignore the preference?

---

_Review comment by @konstin on `crates/uv-settings/src/settings.rs`:521 on 2025-07-29 09:46_

We can make this stronger by saying that the locked version will be used, if a single locked version exists in the lockfile, and if it doesn't cause any conflicts during resolution. (Though the latter isn't about the version being unfulfillable, but about whether it conflicts with higher priority packages in the resolution, which I hope is a detail users won't run into with build deps, and for which `require-locked` will help)

---

_@konstin approved on 2025-07-29 09:46_

---

_Comment by @konstin on 2025-07-29 09:46_

I like the `prefer-locked` name, it captures what it does concisely.

---

_@zanieb reviewed on 2025-07-29 16:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:11557 on 2025-07-29 16:26_

Yes great idea

---

_Marked ready for review by @zanieb on 2025-07-29 18:13_

---

_@zanieb reviewed on 2025-07-29 18:13_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:521 on 2025-07-29 18:13_

I tried to capture this a little in https://github.com/astral-sh/uv/pull/14944/commits/9a91e91f78590e7d58bf4cf3da7b54c4734dc191

---

_@zanieb reviewed on 2025-07-29 18:13_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:11557 on 2025-07-29 18:13_

https://github.com/astral-sh/uv/pull/14944/commits/adfbe02e85bef5091a8d6772a9c8064d0d7846a3

---

_Comment by @zanieb on 2025-07-29 21:33_

@geofft has pointed out an important concern, that since these preferences are not taken into account in the wheel cache key, you won't automatically get a rebuild of a package when your locked version of a build dependency changes; or, worse, if you have another project with a different locked version, they'll share the same cached wheel :/

---

_Converted to draft by @zanieb on 2025-07-29 23:26_

---

_Comment by @zanieb on 2025-07-29 23:27_

This is still "preferring" the locked version, but it sounds problematic. I'm not sure what the workaround is yet.

---

_Comment by @konstin on 2025-07-30 08:49_

> @geofft has pointed out an important concern, that since these preferences are not taken into account in the wheel cache key, you won't automatically get a rebuild of a package when your locked version of a build dependency changes; or, worse, if you have another project with a different locked version, they'll share the same cached wheel :/

This is the same logic we apply today without preferences: We build wheels once, and then cache them forever, even if the resolution of the package's build deps changes. For supporting caching with different version ranges, we need to support multiple builds with the same tag for a package in the cache, attach the build deps versions to it and select only fitting ones installation. A criteria could be whether the versions fit `tool.uv.extra-build-dependencies`.

---

_Comment by @charliermarsh on 2025-08-02 15:38_

> This is the same logic we apply today without preferences: We build wheels once, and then cache them forever, even if the resolution of the package's build deps changes. For supporting caching with different version ranges, we need to support multiple builds with the same tag for a package in the cache, attach the build deps versions to it and select only fitting ones installation.

Yeah, I guess it doesn't seem that much worse than what we have today. Is it worth? We could look at caching separate builds based on build dependencies as a second step, right?

---

_Comment by @zanieb on 2025-08-07 13:50_

Preferring https://github.com/astral-sh/uv/pull/15036

---

_Closed by @zanieb on 2025-08-07 13:50_

---
