```yaml
number: 11959
title: Update top-pypi-packages filename
type: pull_request
state: merged
author: hugovk
labels:
  - internal
assignees: []
merged: true
base: main
head: 30-days
created_at: 2025-03-04T19:08:34Z
updated_at: 2025-03-05T06:36:08Z
url: https://github.com/astral-sh/uv/pull/11959
synced_at: 2026-01-10T11:10:39Z
```

# Update top-pypi-packages filename

---

_Pull request opened by @hugovk on 2025-03-04 19:08_


<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

To stay within quota, it now has just under 30 days of data, so the filename has been updated. Both will be available for a while. See https://github.com/hugovk/top-pypi-packages/pull/46.


## Test Plan

<!-- How was it tested? -->

```sh
curl https://hugovk.github.io/top-pypi-packages/top-pypi-packages-30-days.min.json | jq -r ❯ 
curl https://hugovk.github.io/top-pypi-packages/top-pypi-packages.min.json | jq -r ".rows | .
diff pypi_8k_downloads_original.txt pypi_8k_downloads_new.txt
```


---

_Comment by @zanieb on 2025-03-04 19:10_

Thanks!

---

_Review requested from @konstin by @zanieb on 2025-03-04 19:10_

---

_Assigned to @konstin by @zanieb on 2025-03-04 19:10_

---

_@charliermarsh approved on 2025-03-04 23:22_

---

_Merged by @charliermarsh on 2025-03-04 23:23_

---

_Closed by @charliermarsh on 2025-03-04 23:23_

---

_Label `internal` added by @charliermarsh on 2025-03-04 23:23_

---

_Branch deleted on 2025-03-05 06:36_

---
