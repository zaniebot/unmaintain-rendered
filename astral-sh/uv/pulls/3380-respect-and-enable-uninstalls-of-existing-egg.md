```yaml
number: 3380
title: "Respect and enable uninstalls of existing `.egg-info` packages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/egg
created_at: 2024-05-05T23:07:23Z
updated_at: 2024-05-06T13:47:29Z
url: https://github.com/astral-sh/uv/pull/3380
synced_at: 2026-01-12T16:05:36Z
```

# Respect and enable uninstalls of existing `.egg-info` packages

---

_@charliermarsh_

## Summary

Users often find themselves dropped into environments that contain `.egg-info` packages. While we won't support installing these, it's not hard to support identifying them (e.g., in `pip freeze`) and _uninstalling_ them.

Closes https://github.com/astral-sh/uv/issues/2841.
Closes #2928.
Closes #3341.

## Test Plan

Ran `cargo run pip freeze --python /opt/homebrew/Caskroom/miniforge/base/envs/TEST/bin/python`, with an environment that includes `pip` as an `.egg-info` (`/opt/homebrew/Caskroom/miniforge/base/envs/TEST/lib/python3.12/site-packages/pip-24.0-py3.12.egg-info`):

```
cffi @ file:///Users/runner/miniforge3/conda-bld/cffi_1696001825047/work
pip==24.0
pycparser @ file:///home/conda/feedstock_root/build_artifacts/pycparser_1711811537435/work
setuptools==69.5.1
wheel==0.43.0
```

Then ran `cargo run pip uninstall`, verified that `pip` was uninstalled, and no longer listed in `pip freeze`.


---

_Label `enhancement` added by @charliermarsh on 2024-05-05 23:07_

---

_Label `compatibility` added by @charliermarsh on 2024-05-05 23:07_

---

_Marked ready for review by @charliermarsh on 2024-05-05 23:07_

---

_Review requested from @konstin by @charliermarsh on 2024-05-05 23:15_

---

_Comment by @samypr100 on 2024-05-06 00:45_

Also closes #2841

---

_Comment by @charliermarsh on 2024-05-06 01:04_

Thanks! Added.

---

_@hauntsaninja reviewed on 2024-05-06 01:12_

Should the `metadata` method read from `PKG-INFO`?

---

_Comment by @charliermarsh on 2024-05-06 01:18_

Yes good call, thanks.

---

_@konstin approved on 2024-05-06 09:41_

---

_Merged by @charliermarsh on 2024-05-06 13:47_

---

_Closed by @charliermarsh on 2024-05-06 13:47_

---

_Branch deleted on 2024-05-06 13:47_

---
