```yaml
number: 12237
title: Error on lockfiles with inconsistent source distribution versions
type: pull_request
state: closed
author: konstin
labels:
  - bug
assignees: []
draft: true
base: main
head: konsti/reject-incoherent-source-dist
created_at: 2025-03-17T11:39:53Z
updated_at: 2025-10-09T13:48:34Z
url: https://github.com/astral-sh/uv/pull/12237
synced_at: 2026-01-12T16:10:11Z
```

# Error on lockfiles with inconsistent source distribution versions

---

_@konstin_

Reject all cases where a source distribution builds into a wheel of a different version than was locked in the lockfile.

As an example:

```toml
[[package]]
name = "sniffio"
version = "2.3.4"
source = { url = "https://files.pythonhosted.org/packages/a2/87/a6771e1546d97e7e041b6ae58d80074f81b7d5121207425c964ddf5cfdbd/sniffio-1.3.1.tar.gz" }
sdist = { hash = "sha256:f4324edc670a0f49750a81b895f35c3adb843cca46f0530f79fc1babb23789dc" }
```

This now fails with:

```
  × Failed to download and build `sniffio @
  │ https://files.pythonhosted.org/packages/a2/87/a6771e1546d97e7e041b6ae58d80074f81b7d5121207425c964ddf5cfdbd/sniffio-1.3.1.tar.gz`
  ╰─▶ Package metadata version `1.3.1` does not match given version `2.3.4`
  help: `sniffio` was included because `foo` (v0.1.0) depends on `sniffio`
```

The potential clash could be with git dependencies that use a version-from-git integration. I.e., with the change, we're enforcing coherence where we've been previously lenient. It is on the other hand the only good option for catching errors such as https://github.com/astral-sh/uv/issues/12164 for source distributions.

Needs tests.

---

_Label `bug` added by @konstin on 2025-03-17 11:39_

---

_Review requested from @charliermarsh by @konstin on 2025-03-17 11:39_

---

_Review requested from @jtfmumm by @konstin on 2025-03-17 11:39_

---

_Review comment by @jtfmumm on `crates/uv-installer/src/plan.rs`:267 on 2025-03-17 13:02_

These will be false if we don't know the sdist version, right? Should we instead maintain the old behavior in that case?

Something like:
```
if wheel.filename.name == sdist.name
  && dist.version().map_or(true, |version| version == &wheel.filename.version)
```

---

_@jtfmumm reviewed on 2025-03-17 13:03_

---

_Renamed from "Konsti/reject incoherent source dist" to "Error on lockfiles with inconsistent source distribution versions" by @zanieb on 2025-03-17 22:46_

---

_Closed by @konstin on 2025-10-09 13:48_

---
