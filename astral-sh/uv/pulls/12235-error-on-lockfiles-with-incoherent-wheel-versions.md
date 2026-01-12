```yaml
number: 12235
title: Error on lockfiles with incoherent wheel versions
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/locked-version-coherence
created_at: 2025-03-17T10:10:16Z
updated_at: 2025-03-18T13:21:02Z
url: https://github.com/astral-sh/uv/pull/12235
synced_at: 2026-01-12T16:10:11Z
```

# Error on lockfiles with incoherent wheel versions

---

_@konstin_

Reject lockfiles where the package version and the wheel versions are incoherent. This implicitly checks that all wheel files have the same version.

It does not check for the source dist version, since a source dist may not contain a version in the filename and attempting to deserialize source dist filenames we may not need is a performance overhead for something that's already slow in `uv run`.

Fixes #12164
See also https://github.com/astral-sh/uv/issues/12254

---

_Label `bug` added by @konstin on 2025-03-17 10:10_

---

_Review comment by @jtfmumm on `crates/uv-resolver/src/lock/mod.rs`:2883 on 2025-03-17 10:20_

We should always have at least one wheel enabling us to catch the inconsistency?

---

_Review comment by @jtfmumm on `crates/uv/tests/it/sync.rs`:8557 on 2025-03-17 10:25_

Would a user find this surprising? I think I'd assume uv will create a correct lockfile when running `uv lock`. And it might not be clear to a user what action they should take next if they see this error.

I guess it partially depends on what "If a lockfile is present, its contents will be used as preferences for the resolution" from the docs for `uv lock` means. Does that mean we try to use those preferences, but override them if necessary?

---

_@jtfmumm reviewed on 2025-03-17 10:27_

---

_@konstin reviewed on 2025-03-17 10:42_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:2883 on 2025-03-17 10:42_

Not necessarily, for example:

```toml
dependencies = [
    "sniffio",
]

[tool.uv.sources]
sniffio = { path = "sniffio.tar.gz" }
```

`uv.lock`:

```toml
[[package]]
name = "sniffio"
version = "1.3.1"
source = { path = "sniffio.tar.gz" }
sdist = { hash = "sha256:f4324edc670a0f49750a81b895f35c3adb843cca46f0530f79fc1babb23789dc" }
```

In this case, it's not possible to determine from the lockfile alone whether there is an inconsistency.

---

_@konstin reviewed on 2025-03-17 10:49_

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:8557 on 2025-03-17 10:49_

Currently, we fail on all kinds of parsing errors with lockfile. For example, an empty `uv.lock` fails:

```
$ uv lock
error: Failed to parse `uv.lock`
  Caused by: TOML parse error at line 1, column 1
  |
1 | 
  | ^
missing field `version`
```

As does a lockfile where I removed the source line:

```
$ uv lock
error: Failed to parse `uv.lock`
  Caused by: TOML parse error at line 16, column 1
   |
16 | [[package]]
   | ^^^^^^^^^^^
missing field `source`
```

We can try to be better about recovering from a broken lockfile, but that's outside of the scope of the PR (uv would never write such a lockfile, and with this change, uv will refuse to sync a broken dependabot PR, failing CI). For comparison, in cases where the lockfile is not invalid but doesn't fully match in a non-version anymore, e.g., the environments changed, we for example only reuse the versions as preference but not the full lockfile (`ValidatedLock` enumerates the cases).

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:2883 on 2025-03-17 11:30_

I've added another commit that catches all mismatches between locked source dist version and the built wheel versions at build time. This would enforce stricter guarantees than we require right now, CC @charliermarsh 

---

_@konstin reviewed on 2025-03-17 11:30_

---

_@konstin reviewed on 2025-03-17 11:32_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:2883 on 2025-03-17 11:32_

As an example:

```toml
[[package]]
name = "sniffio"
version = "2.3.4"
source = { url = "https://files.pythonhosted.org/packages/a2/87/a6771e1546d97e7e041b6ae58d80074f81b7d5121207425c964ddf5cfdbd/sniffio-1.3.1.tar.gz" }
sdist = { hash = "sha256:f4324edc670a0f49750a81b895f35c3adb843cca46f0530f79fc1babb23789dc" }
```

With this last commit fails with:

```
  × Failed to download and build `sniffio @
  │ https://files.pythonhosted.org/packages/a2/87/a6771e1546d97e7e041b6ae58d80074f81b7d5121207425c964ddf5cfdbd/sniffio-1.3.1.tar.gz`
  ╰─▶ Package metadata version `1.3.1` does not match given version `2.3.4`
  help: `sniffio` was included because `foo` (v0.1.0) depends on `sniffio`
```

The potential clash could be with git dependencies that use a version-from-git integration.

---

_@konstin reviewed on 2025-03-17 11:40_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:2883 on 2025-03-17 11:40_

Moved this to a separate PR: https://github.com/astral-sh/uv/pull/12237

---

_Comment by @charliermarsh on 2025-03-17 13:17_

Does this mean that Dependabot updates will lead to hard uv errors? (I understand that, at least the linked issue, what they're doing today may not be sound.)

---

_Comment by @konstin on 2025-03-17 13:20_

I think so, at least making PRs with this logic without updating source dist and wheels accordingly will lead to broken lockfiles: https://github.com/dependabot/dependabot-core/blob/1cd6fc59d7dae25d46255550528dd056c97c46d8/uv/lib/dependabot/uv/file_updater/lock_file_updater.rb#L111-L120. We currently accept those lockfile changes, but they don't do semantically what the PR diff would suggest (they don't actually upgrade the version).

Other commenters at https://github.com/dependabot/dependabot-core/issues/10478 have noted that dependabot can produce lockfiles broken under `--locked` already (https://github.com/dependabot/dependabot-core/issues/10478#issuecomment-2722750493)

---

_Comment by @shauneccles on 2025-03-17 21:29_

As a user and developer I want uv to explode if dependabot maims my lockfile so I know about it and so I can raise issues with dependabot.

---

_Comment by @urschrei on 2025-03-17 21:36_

> Other commenters at [dependabot/dependabot-core#10478](https://github.com/dependabot/dependabot-core/issues/10478) have noted that dependabot can produce lockfiles broken under `--locked` already ([dependabot/dependabot-core#10478 (comment)](https://github.com/dependabot/dependabot-core/issues/10478#issuecomment-2722750493))

Yep that's my lockfile, and they certainly do currently break them (tysm for `--locked`: it does what it says on the tin)

---

_@zanieb reviewed on 2025-03-17 21:38_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:2880 on 2025-03-17 21:38_

Can we show the versions in the error here? Like "packaged version {} does not match {} from wheel {}"?

---

_@zanieb reviewed on 2025-03-17 22:02_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:2880 on 2025-03-17 22:02_

https://github.com/astral-sh/uv/pull/12249

---

_@zanieb approved on 2025-03-17 22:09_

---

_Review requested from @charliermarsh by @zanieb on 2025-03-17 22:11_

---

_Renamed from "Reject lockfiles with incoherent versions" to "Error on lockfiles with incoherent wheel versions" by @zanieb on 2025-03-17 22:32_

---

_Merged by @zanieb on 2025-03-17 22:33_

---

_Closed by @zanieb on 2025-03-17 22:33_

---

_Branch deleted on 2025-03-17 22:33_

---

_Comment by @kleinhenz on 2025-03-18 13:19_

This seems to have caused some problems with the flash-attn wheels.

```
#pyproject.toml
...

[tool.uv]
environments = ["sys_platform == 'darwin'", "sys_platform == 'linux'"]
constraint-dependencies = ["torch==2.5.1"]

[tool.uv.sources]
flash_attn = [
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp310-cp310-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.10'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp311-cp311-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.11'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp312-cp312-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.12'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp313-cp313-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.13'"}
]
```

Now causes `uv sync` to error with
```
error: Failed to parse `uv.lock`
  Caused by: The entry for package `flash-attn` v2.7.3 has wheel `flash_attn-2.7.3+cu12torch2.5cxx11abifalse-cp310-cp310-linux_x86_64.whl` with inconsistent version: v2.7.3+cu12torch2.5cxx11abifalse
  ```
  
  Not sure if there is a better way to configure usage of these wheels.

---

_Comment by @charliermarsh on 2025-03-18 13:21_

Ah thanks, we can fix that.

---
