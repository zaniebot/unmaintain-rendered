```yaml
number: 732
title: Finish install-many with pypi 10k most dependents
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: mkl-fft
created_at: 2023-12-26T16:55:28Z
updated_at: 2023-12-27T14:42:57Z
url: https://github.com/astral-sh/uv/pull/732
synced_at: 2026-01-12T16:04:09Z
```

# Finish install-many with pypi 10k most dependents

---

_@konstin_

This PR combines three small changes to finish up the install-many testing.

* Download pypi_10k_most_dependents.txt in script I'd like to have the setup process of the large scale checks automated.
* Some install-many dev script improvements 
* Fix mkl_fft-1.3.6-58-cp310-cp310-manylinux2014_x86_64.whl: mkl_fft-1.3.6-58-cp310-cp310-manylinux2014_x86_64.whl has multiple Wheel-Version entries, we have to ignore that like pip

Apart from the mkl-fft fix the only other errors i've seen showing up are https://github.com/astral-sh/puffin/issues/520#issuecomment-1869625642.

---

_@charliermarsh approved on 2023-12-27 14:42_

---

_Merged by @charliermarsh on 2023-12-27 14:42_

---

_Closed by @charliermarsh on 2023-12-27 14:42_

---

_Branch deleted on 2023-12-27 14:42_

---

_Label `internal` added by @charliermarsh on 2023-12-27 14:42_

---
