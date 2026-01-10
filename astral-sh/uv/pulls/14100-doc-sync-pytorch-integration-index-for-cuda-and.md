```yaml
number: 14100
title: "doc: Sync PyTorch integration index for CUDA and ROCm versions from PyTorch website."
type: pull_request
state: merged
author: FishAlchemist
labels:
  - enhancement
assignees: []
merged: true
base: main
head: doc_sync_pytorch_install_index
created_at: 2025-06-17T08:00:40Z
updated_at: 2025-06-17T14:08:10Z
url: https://github.com/astral-sh/uv/pull/14100
synced_at: 2026-01-10T11:10:42Z
```

# doc: Sync PyTorch integration index for CUDA and ROCm versions from PyTorch website.

---

_Pull request opened by @FishAlchemist on 2025-06-17 08:00_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Just to sync the documentation with PyTorch's officially recommended installation method.
![image](https://github.com/user-attachments/assets/7be0aea6-b51b-4083-acae-fbe8aa79c363)
**Change:**
* CUDA 12.1 to CUDA 12.6
* CUDA 12.4 to CUDA 12.8
* ROCm 6.2 to ROCm 6.3
## Test Plan
Run doc server in local.
Result:
<details><summary>CUDA 12.6</summary>
<p>

![image](https://github.com/user-attachments/assets/962a1058-3922-4709-a57f-200939d0a397)
![image](https://github.com/user-attachments/assets/0dae693f-2dc1-4d3e-9186-d26aa84c7d73)

</p>
</details> 
<details><summary>CUDA 12.8</summary>
<p>

![image](https://github.com/user-attachments/assets/dc12d09b-f8f2-41d3-b185-cb1e4e8b062b)
![image](https://github.com/user-attachments/assets/1b5490e3-6265-4bad-8af2-eab0a207adab)

</p>
</details> 
<details><summary>ROCm6</summary>
<p>

![image](https://github.com/user-attachments/assets/3b17a24f-3210-4a99-a81b-f8f2f5578b73)
![image](https://github.com/user-attachments/assets/c7baae55-14e5-45d0-94a0-164ab31bbffc)

</p>
</details> 




---

_Comment by @FishAlchemist on 2025-06-17 08:02_

I considered changing name to ROCm 6.3, but then I decided against it.
let's keep the name ``pytorch-rocm`` to maintain consistency with the past documentation style.


---

_Renamed from "doc: Sync PyTorch integration index for CUDA and ROCm versions" to "doc: Sync PyTorch integration index for CUDA and ROCm versions from PyTorch website." by @FishAlchemist on 2025-06-17 08:03_

---

_Review requested from @charliermarsh by @konstin on 2025-06-17 10:48_

---

_Label `enhancement` added by @konstin on 2025-06-17 10:48_

---

_@charliermarsh approved on 2025-06-17 13:54_

---

_Merged by @charliermarsh on 2025-06-17 13:55_

---

_Closed by @charliermarsh on 2025-06-17 13:55_

---

_Branch deleted on 2025-06-17 14:08_

---
