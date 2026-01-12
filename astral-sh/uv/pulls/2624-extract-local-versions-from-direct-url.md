```yaml
number: 2624
title: Extract local versions from direct URL requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/locals-url
created_at: 2024-03-22T21:18:08Z
updated_at: 2024-03-22T21:34:16Z
url: https://github.com/astral-sh/uv/pull/2624
synced_at: 2026-01-12T16:05:08Z
```

# Extract local versions from direct URL requirements

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/2623.

## Test Plan

`cargo run pip install -r requirements.txt`, with:

```
 whisperX @ https://github.com/m-bain/whisperX/archive/befe2b242eb59dcd7a8a122d127614d5c63d36e9.zip
 pyannote-audio @ git+https://github.com/pyannote/pyannote-audio@11b56a137a578db9335efc00298f6ec1932e6317
 torch @ https://download.pytorch.org/whl/cu118/torch-2.2.1%2Bcu118-cp311-cp311-linux_x86_64.whl#sha256=84328a35621cc6a67a182a327baaab67e5f5869981c4b1677ed05f92c15cceb1
 torchaudio @ https://download.pytorch.org/whl/cu118/torchaudio-2.2.1%2Bcu118-cp311-cp311-linux_x86_64.whl#sha256=cd3b1c3582b17792c6d7a367dea0459b123e54d7a4242809ea87ccc10fa220e5
 pytorch_triton @ https://download.pytorch.org/whl/nightly/pytorch_triton-2.1.0%2B7d1a95b046-cp311-cp311-linux_x86_64.whl
```

---

_Marked ready for review by @charliermarsh on 2024-03-22 21:18_

---

_Label `bug` added by @charliermarsh on 2024-03-22 21:18_

---

_@zanieb approved on 2024-03-22 21:33_

Nice!

---

_Merged by @charliermarsh on 2024-03-22 21:34_

---

_Closed by @charliermarsh on 2024-03-22 21:34_

---

_Comment by @zanieb on 2024-03-22 21:34_

Sounds close to an enhancement to me. 

---

_Branch deleted on 2024-03-22 21:34_

---
