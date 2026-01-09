---
number: 16084
title: "filter option for `uv tree` to easily check a subset of dependencies and their versions"
type: issue
state: open
author: phinate
labels:
  - enhancement
assignees: []
created_at: 2025-10-01T12:49:22Z
updated_at: 2025-10-01T12:49:22Z
url: https://github.com/astral-sh/uv/issues/16084
synced_at: 2026-01-07T13:12:19-06:00
---

# filter option for `uv tree` to easily check a subset of dependencies and their versions

---

_Issue opened by @phinate on 2025-10-01 12:49_

### Summary

I've had some success with filtering the output of `uv tree` using `sed`. Would you welcome a feature addition for this as a kwarg, e.g. `uv tree --filter "torch"`? Something that could preserve the tree structure where present is also another idea that might be useful. 

Happy to look into contributing this if of interest, but didn't want to make a PR without a vibe-check on if this is useful/within scope :) 


### Example

Here's what I've currently run:

```
$ uv tree --dev \
| sed -nE '/torch/I s/.*[├└]──[[:space:]]*([^[:space:]]+)[[:space:]]+v([^[:space:])]+).*$/\1 \2/p' \
| sort -u

Resolved 340 packages in 29ms
pytorch-lightning 2.5.5
torch 2.8.0
torch-geometric 2.6.1
torch-harmonics 0.8.0
torchinfo 1.8.0
torchmetrics 1.8.2
torchvision 0.23.0
```

Which could be packaged up as 

```
$ uv tree --dev --filter torch

Resolved 340 packages in 29ms
pytorch-lightning 2.5.5
torch 2.8.0
torch-geometric 2.6.1
torch-harmonics 0.8.0
torchinfo 1.8.0
torchmetrics 1.8.2
torchvision 0.23.0
```

---

_Label `enhancement` added by @phinate on 2025-10-01 12:49_

---
