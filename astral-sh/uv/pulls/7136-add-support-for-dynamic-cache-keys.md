```yaml
number: 7136
title: Add support for dynamic cache keys
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cache
assignees: []
merged: true
base: main
head: charlie/dynamic-cache
created_at: 2024-09-06T21:04:49Z
updated_at: 2024-09-09T20:20:34Z
url: https://github.com/astral-sh/uv/pull/7136
synced_at: 2026-01-10T12:53:41Z
```

# Add support for dynamic cache keys

---

_Pull request opened by @charliermarsh on 2024-09-06 21:04_

## Summary

This PR adds a more flexible cache invalidation abstraction for uv, and uses that new abstraction to improve support for dynamic metadata.

Specifically, instead of relying solely on a timestamp, we now pass around a `CacheInfo` struct which (as of now) contains `Option<Timestamp>` and `Option<Commit>`. The `CacheInfo` is saved in `dist-info` as `uv_cache.json`, so we can test already-installed distributions for cache validity (along with testing _cached_ distributions for cache validity).

Beyond the defaults (`pyproject.toml`, `setup.py`, and `setup.cfg` changes), users can also specify additional cache keys, and it's easy for us to extend support in the future. Right now, cache keys can either be instructions to include the current commit (for `setuptools_scm` and similar) or file paths (for `hatch-requirements-txt` and similar):

```toml
[tool.uv]
cache-keys = [{ file = "requirements.txt" }, { git = true }]
```

This change should be fully backwards compatible.

Closes https://github.com/astral-sh/uv/issues/6964.

Closes https://github.com/astral-sh/uv/issues/6255.

Closes https://github.com/astral-sh/uv/issues/6860.


---

_Marked ready for review by @charliermarsh on 2024-09-06 21:11_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-06 21:11_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-06 21:11_

---

_Review requested from @konstin by @charliermarsh on 2024-09-06 21:11_

---

_Label `enhancement` added by @charliermarsh on 2024-09-06 21:18_

---

_Label `cache` added by @charliermarsh on 2024-09-06 21:18_

---

_Comment by @charliermarsh on 2024-09-06 21:18_

Going to move the https://github.com/astral-sh/uv/issues/7028 part into its own commit, since it's largely independent.

---

_Renamed from "Add support for dynamic cache keys and build settings" to "Add support for dynamic cache keys" by @charliermarsh on 2024-09-06 21:41_

---

_Review comment by @konstin on `docs/reference/settings.md`:102 on 2024-09-09 14:37_

```suggestion
```

---

_Review comment by @konstin on `docs/reference/settings.md`:91 on 2024-09-09 14:41_

Given that we always read the default three files, should this be called `extend-cache-keys`, mirroring ruff's `extend-` settings?

---

_Review comment by @konstin on `crates/uv-cache-info/src/cache_info.rs`:15 on 2024-09-09 14:43_

This struct needs a docstring what it represents, `timestamp` whose timestamp it covers

---

_Review comment by @konstin on `crates/uv-cache-info/src/cache_info.rs`:45 on 2024-09-09 14:47_

```suggestion
    /// Compute the cache info from the root of a source tree.
    pub fn from_directory(directory: &Path) -> io::Result<Self> {
```

---

_Review comment by @konstin on `crates/uv-cache-info/src/cache_info.rs`:101 on 2024-09-09 14:48_

```suggestion
    /// Compute the cache info for an sdist or wheel archive.
    pub fn from_file(path: impl AsRef<Path>) -> Result<Self, io::Error> {
```

---

_Review comment by @konstin on `crates/uv-cache-info/src/cache_info.rs`:35 on 2024-09-09 14:49_

```suggestion
    /// Compute the cache info for a path (including file) dependency. 
    pub fn from_path(path: &Path) -> io::Result<Self> {
```

---

_Review comment by @konstin on `crates/uv-cache-info/src/cache_info.rs`:126 on 2024-09-09 14:50_

Is that for backwards compatibility?

---

_Review comment by @konstin on `crates/uv-cache-info/src/cache_info.rs`:148 on 2024-09-09 14:50_

We're developing quite a sprawl of pyproject.toml subsets that we're re-reading

---

_Review comment by @konstin on `crates/uv/tests/pip_install.rs`:3119 on 2024-09-09 15:01_

What's the invalid part here?

---

_Review comment by @konstin on `crates/install-wheel-rs/Readme.md`:3 on 2024-09-09 15:03_

I'd drop this file, it has changed a lot from just being a reimplementation

---

_Review comment by @konstin on `crates/uv-cache-info/src/commit_info.rs`:3 on 2024-09-09 15:05_

It's always the 40 char hex representation, right?

---

_@konstin approved on 2024-09-09 15:11_

---

_@zanieb reviewed on 2024-09-09 17:02_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3119 on 2024-09-09 17:02_

I think it's that the cache is invalidated, not that anything is invalid.

---

_@zanieb reviewed on 2024-09-09 17:03_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3162 on 2024-09-09 17:03_

Nit: Can we say like "Installing again"? Reinstall implies `--reinstall` to me.

---

_@zanieb reviewed on 2024-09-09 17:05_

---

_Review comment by @zanieb on `docs/reference/settings.md`:91 on 2024-09-09 17:05_

I don't feel strongly about this, but we don't use `extend-` at all in uv yet and you won't be able to change the base cache keys so I don't think it's necessary here.

---

_@zanieb approved on 2024-09-09 17:06_

---

_@konstin reviewed on 2024-09-09 18:04_

---

_Review comment by @konstin on `crates/uv/tests/pip_install.rs`:3119 on 2024-09-09 18:04_

ah i misread invalidate as invalid

---

_@konstin reviewed on 2024-09-09 18:32_

---

_Review comment by @konstin on `docs/reference/settings.md`:91 on 2024-09-09 18:32_

I'm not particular on the specific naming, but i think it's an odd design that we expose a `cache-keys` setting that then doesn't let you configure the cache keys, but add files/git to our immutable defaults.

---

_Comment by @codspeed-hq[bot] on 2024-09-09 18:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/dynamic-cache)

### Merging #7136 will **not alter performance**

<sub>Comparing <code>charlie/dynamic-cache</code> (d366c7e) with <code>main</code> (9a7262c)</sub>



### Summary

`✅ 14` untouched benchmarks






---

_@charliermarsh reviewed on 2024-09-09 18:38_

---

_Review comment by @charliermarsh on `docs/reference/settings.md`:91 on 2024-09-09 18:38_

What if we make our current settings a default, and this overrides them?

---

_@konstin reviewed on 2024-09-09 18:44_

---

_Review comment by @konstin on `docs/reference/settings.md`:91 on 2024-09-09 18:44_

I'd find that more intuitive. It's a bit annoying that you have to re-specify `pyproject.toml` and/or `setup.py`, which i assumed that's what you wanted to avoid with that design; but personally i like just specifying all relevant files.

---

_@zanieb reviewed on 2024-09-09 18:45_

---

_Review comment by @zanieb on `docs/reference/settings.md`:91 on 2024-09-09 18:45_

Certainly an option, but are there cases where you would actually want to ignore changes to those other files?

---

_Comment by @sbidoul on 2024-09-09 18:53_

Ooh, this is interesting. ❤️ 
Could file accept a glob pattern?

---

_Comment by @charliermarsh on 2024-09-09 18:56_

I don't see why not, though it will get slower as you add more files hah.

---

_Comment by @charliermarsh on 2024-09-09 18:56_

Can you give an example of how you'd use it? Just curious.

---

_Comment by @sbidoul on 2024-09-09 19:08_

In Odoo projects (using the hatch-odoo build backend), we have dynamic dependencies computed from Odoo addons manifests. So I'd use `odoo/addons/*/__manifest__.py`.


---

_@charliermarsh reviewed on 2024-09-09 19:10_

---

_Review comment by @charliermarsh on `docs/reference/settings.md`:91 on 2024-09-09 19:10_

I guess one upside is that we could document the default more clearly...? I'm torn here though, I guess I don't feel strongly either way.

---

_Merged by @charliermarsh on 2024-09-09 20:19_

---

_Closed by @charliermarsh on 2024-09-09 20:19_

---

_Branch deleted on 2024-09-09 20:19_

---
