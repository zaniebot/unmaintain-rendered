```yaml
number: 2715
title: "uv-cache: bump built-wheels cache version"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/bump-built-wheel-cache
created_at: 2024-03-28T18:14:14Z
updated_at: 2024-03-28T18:26:07Z
url: https://github.com/astral-sh/uv/pull/2715
synced_at: 2026-01-12T16:05:11Z
```

# uv-cache: bump built-wheels cache version

---

_@BurntSushi_

It turns out that #2712 did _not_ fix #2711. After I put up #2712, I
started trying to track down the specific change that caused the
failure. I had assumed at first that it was related to one of our `rkyv`
types, but it actually ended up being one of our msgpack caches. I think
the failure mode is still fundamentally the same idea: the cached data
changed in a way that is still valid msgpack, but got interpreted
differently after deserializing.

The specific change that caused this was the [removal of a field] from our
metadata type.

Ideally we would just undo the change and add the field back. But that
change has already been shipped out to users. So I believe the only
plausible choice at this point is to bump the `built-wheels` cache. This
will unfortunately mean that `uv` will need to re-build wheels.

Fixes #2711

[removal of a field]: https://github.com/astral-sh/uv/commit/365c2925250351e666edad68199ef06a3d83584c#diff-e42586829f9c2cdbb909bedc5cf95691cc415247f2cbc2ebeb80d887020457bbL29


---

_Review requested from @charliermarsh by @BurntSushi on 2024-03-28 18:14_

---

_Review requested from @zanieb by @BurntSushi on 2024-03-28 18:14_

---

_@zanieb approved on 2024-03-28 18:16_

---

_Comment by @BurntSushi on 2024-03-28 18:16_

Here is my test protocol. First, the failure:

```
$ rm -rf ~/.cache/uv && rm -rf .venv && uv-0.1.24 venv && uv-0.1.24 pip install -q homeassistant
Using Python 3.12.0 interpreter at: /home/andrew/.pyenv/versions/3.12.0/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ uv-0.1.25 pip install -q homeassistant --reinstall-package homeassistant
error: Failed to download and build: pyric==0.1.6.3
  Caused by: Failed to deserialize cache entry
  Caused by: expected version to start with a number, but no leading ASCII digits were found
$
```

And now a demonstration that this PR doesn't have the issue:

```
$ rm -rf ~/.cache/uv && rm -rf .venv && uv-0.1.24 venv && uv-0.1.24 pip install -q homeassistant
Using Python 3.12.0 interpreter at: /home/andrew/.pyenv/versions/3.12.0/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ uv-this-pr pip install -q homeassistant --reinstall-package homeassistant
$
```

---

_@charliermarsh approved on 2024-03-28 18:22_

---

_Comment by @charliermarsh on 2024-03-28 18:23_

You should also be able to un-bump the simple cache version.

---

_Comment by @charliermarsh on 2024-03-28 18:23_

But... I don't feel strongly. Let's just leave it bumped actually.

---

_Merged by @zanieb on 2024-03-28 18:24_

---

_Closed by @zanieb on 2024-03-28 18:24_

---

_Branch deleted on 2024-03-28 18:24_

---

_Comment by @BurntSushi on 2024-03-28 18:26_

Yeah I thought about that, but didn't want to get too cute since that bump already landed on `main`.

---
