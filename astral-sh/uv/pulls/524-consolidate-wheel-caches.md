```yaml
number: 524
title: Consolidate wheel caches
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: unzipped-wheel-cache-invalidation
created_at: 2023-12-01T12:05:59Z
updated_at: 2023-12-04T08:52:01Z
url: https://github.com/astral-sh/uv/pull/524
synced_at: 2026-01-10T15:44:44Z
```

# Consolidate wheel caches

---

_Pull request opened by @konstin on 2023-12-01 12:05_

After this change, two wheel caches remain: `built-wheels-v0` and `wheels-v0`, docs screenshots below. Each contains both the wheel metadata, cache policy and zip or unzipped wheels under the same name.

The zipped/unzipped strategy is as follows: In `pip-compile`, when we build a wheel, we store it zipped. When `pip-sync` or a source dist build in `pip-compile` need to install the wheel, we unzip it, remove the file and replace it with the unzipped wheel.

This removes `WheelCache` and `UrlIndex` in favor of `Cache` plus `WheelCache`. The non-built wheel cache now considers index urls and the url for url wheels.

I'm unsure if we need the `Unzipper` type, this could just be a function.

I move `no_index` into `IndexUrls` and started using `IndexUrl` up to the clap level.

I left a number of TODOs in the code, namely performing the actual invalidation of unzipped wheels and making the `InstallPlan` understand cache invalidation (i.e. uninstall wheels when their remote changed).

![image](https://github.com/astral-sh/puffin/assets/6826232/c4d45979-485b-4954-848d-fd3347ee2510)



---

_Comment by @konstin on 2023-12-01 12:06_

Current dependencies on/for this PR:
* main
  * **PR #519** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/519?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #524** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/524?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/524?utm_source=stack-comment).

---

_@charliermarsh reviewed on 2023-12-01 18:37_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/wheel_and_metadata.rs`:27 on 2023-12-01 18:37_

I would be fine calling this `WheelCache`, and saying that it stores both metadata and built archives.

---

_@charliermarsh reviewed on 2023-12-01 18:37_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:186 on 2023-12-01 18:37_

I find this message a little confusing. I think `Failed to download and build distributions` is clearer.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:44 on 2023-12-01 18:40_

Why was the `IndexUrls` refactor necessary to enable this? Or is it a semi-unrelated refactor that was included in this PR anyway?

---

_@charliermarsh reviewed on 2023-12-01 18:40_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/plan.rs`:107 on 2023-12-01 18:42_

Noticing that we can probably do this without leaving the `Result` monad, like `if let Ok(...) = `.

---

_@charliermarsh reviewed on 2023-12-01 18:42_

---

_@charliermarsh reviewed on 2023-12-01 18:42_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:58 on 2023-12-01 18:42_

This looks unintentional.

---

_@charliermarsh reviewed on 2023-12-01 18:44_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:53 on 2023-12-01 18:44_

Can you explain this? Why do we remove the zipped wheel? Is this like replacing `foo.whl` with the unzipped `foo` directory?

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:63 on 2023-12-01 18:44_

Can you explain this part?

---

_@charliermarsh reviewed on 2023-12-01 18:44_

---

_@charliermarsh reviewed on 2023-12-01 19:25_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:53 on 2023-12-01 19:25_

I'd probably vote to just keep both for now.

---

_@charliermarsh reviewed on 2023-12-01 20:08_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:53 on 2023-12-01 20:08_

Kept this as-is...

---

_@charliermarsh reviewed on 2023-12-01 20:09_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:63 on 2023-12-01 20:09_

Removed this, but let me know if it's necessary.

---

_Comment by @charliermarsh on 2023-12-01 20:11_

> The zipped/unzipped strategy is as follows: In pip-compile, when we build a wheel, we store it zipped. When pip-sync or a source dist build in pip-compile need to install the wheel, we unzip it, remove the file and replace it with the unzipped wheel.

@konstin - If we then run `pip-compile` again, will we read from the metadata from the unzipped wheel?


---

_Merged by @charliermarsh on 2023-12-01 20:16_

---

_Closed by @charliermarsh on 2023-12-01 20:16_

---

_Branch deleted on 2023-12-01 20:16_

---

_Comment by @charliermarsh on 2023-12-01 20:21_

Maybe another question that would help with my understanding: can you walk through the caching story for a PyPI source distribution, assuming we run `pip-compile`, then `pip-sync`, then `pip-compile` again?

---

_@konstin reviewed on 2023-12-04 08:21_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_compile.rs`:44 on 2023-12-04 08:21_

I had to move them anyway and it became a semi-related refactor

---

_Comment by @konstin on 2023-12-04 08:49_

> If we then run pip-compile again, will we read from the metadata from the unzipped wheel?

No, we read the `.json` file next to it.

> can you walk through the caching story for a PyPI source distribution, assuming we run pip-compile, then pip-sync, then pip-compile again?

Wheel unzipping happens lazily: On the first `pip-compile`, we build the wheel, store it zipped and store the metadata as json. On the `pip-sync`, we unzip the wheel, store the directory (, remove the zip file) and copy the directory. On the next `pip-compile`, we read the cached metadata from the json. `pip-compile` doesn't need or look for the actual built wheel, but it has to build the source distribution to determine the metadata so we might all we cache it. We don't eagerly unzip it to avoid the performance impact. On the next `pip-sync`, we only copy the unzipped directory. When it either command needs to invalidate the cache, it remove the entire source dist directory.

For comprehensiveness, if we first `pip-sync` and then `pip-compile`, the first `pip-sync` will still write the metadata `.json` (they share the same code path), so the second `pip-compile` runs cached.

---

_@konstin reviewed on 2023-12-04 08:52_

---

_Review comment by @konstin on `crates/puffin-installer/src/unzipper.rs`:63 on 2023-12-04 08:52_

We moved the cache dir in the previous line, so the drop of `TempDir` wouldn't find it anymore. This errors silently but i'd prefer it not to error at all.

---
