```yaml
number: 2714
title: Test cache against latest release in CI
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/cache-test
created_at: 2024-03-28T17:35:13Z
updated_at: 2024-03-28T18:49:55Z
url: https://github.com/astral-sh/uv/pull/2714
synced_at: 2026-01-10T14:49:08Z
```

# Test cache against latest release in CI

---

_Pull request opened by @zanieb on 2024-03-28 17:35_

Detect cache incompatibility issues like #2711 by testing against the last version of uv continuously

---

_Label `testing` added by @zanieb on 2024-03-28 17:35_

---

_@BurntSushi approved on 2024-03-28 17:39_

I think this is a decent sanity check. I wonder if it might be better to install something bigger like `homeassistant` to get a bigger sampling of packages.

This won't catch silent errors, but I think it will at least catch some subset of errors that `rkyv` validation doesn't catch.

---

_Comment by @zanieb on 2024-03-28 17:40_

I'll test homeassistant and choose something prudent for test suite runtime

---

_Comment by @BurntSushi on 2024-03-28 17:43_

Yeah, it looks like getting a broader sampling here matters. Things are hunky dory with `anyio`:

```
$ uv cache clean && rm -rf .venv
Clearing cache at: /home/andrew/.cache/uv
Removed 41594 files (246.2MiB)

$ uv-0.1.24 venv
Using Python 3.12.0 interpreter at: /home/andrew/.pyenv/versions/3.12.0/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

$ uv-0.1.24 pip install anyio
Resolved 3 packages in 80ms
Downloaded 3 packages in 23ms
Installed 3 packages in 5ms
 + anyio==4.3.0
 + idna==3.6
 + sniffio==1.3.1

$ uv-0.1.25 pip install anyio --reinstall-package anyio
Resolved 3 packages in 25ms
Installed 1 package in 6ms
 - anyio==4.3.0
 + anyio==4.3.0
```

But `homeassistant` lets me repro:

```
$ uv cache clean && rm -rf .venv
Clearing cache at: /home/andrew/.cache/uv
Removed 41598 files (246.2MiB)

$ uv-0.1.24 venv
Using Python 3.12.0 interpreter at: /home/andrew/.pyenv/versions/3.12.0/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

$ uv-0.1.24 pip install -q homeassistant

$ uv-0.1.25 pip install -q homeassistant --reinstall-package homeassistant
error: Failed to download and build: pyric==0.1.6.3
  Caused by: Failed to deserialize cache entry
  Caused by: expected version to start with a number, but no leading ASCII digits were found
```

---

_Review requested from @BurntSushi by @zanieb on 2024-03-28 18:36_

---

_@BurntSushi approved on 2024-03-28 18:42_

This LGTM. 

---

_Merged by @zanieb on 2024-03-28 18:49_

---

_Closed by @zanieb on 2024-03-28 18:49_

---

_Branch deleted on 2024-03-28 18:49_

---
