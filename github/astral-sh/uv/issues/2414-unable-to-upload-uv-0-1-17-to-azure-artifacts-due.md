---
number: 2414
title: "Unable to upload `uv>=0.1.17` to Azure Artifacts due to `Metadata-Version: 2.3`"
type: issue
state: closed
author: olivierlefloch
labels: []
assignees: []
created_at: 2024-03-13T14:34:23Z
updated_at: 2024-03-17T21:38:39Z
url: https://github.com/astral-sh/uv/issues/2414
synced_at: 2026-01-07T13:12:17-06:00
---

# Unable to upload `uv>=0.1.17` to Azure Artifacts due to `Metadata-Version: 2.3`

---

_Issue opened by @olivierlefloch on 2024-03-13 14:34_

`uv==0.1.17`'s wheel seems to have switched to `Metadata-Version: 2.3`, which Azure Artifacts does not support:

(same issue with `ruff`) https://github.com/astral-sh/ruff/issues/10378

It's not clear whether this will get resolved promptly by Azure: https://developercommunity.visualstudio.com/t/Uploading-ruff032-to-Azure-Repos-for/10614507

```
Bad Request - Metadata version '2.3' is unsupported. Supported versions
         are: 1.0,1.1,1.2,2.0,2.1.
```

---

_Comment by @zanieb on 2024-03-13 14:38_

Thanks for the report!

This is unfortunate, I'm not sure what our best option is here. Can you build the wheels yourself?

---

_Referenced in [PyO3/maturin#1993](../../PyO3/maturin/issues/1993.md) on 2024-03-14 16:34_

---

_Comment by @olivierlefloch on 2024-03-14 16:36_

After further investigation, it seems `maturin` recently updated the value they use for `Metadata-Version` https://github.com/PyO3/maturin/issues/1993

Other than perhaps considering pinning `maturin` in your build pipeline, so that such issues are under your control rather than potentially due to changes to your build pipeline that you don't control, I don't think there's anything for `astral-sh` to do!

---

_Comment by @charliermarsh on 2024-03-17 21:38_

We could pin back to Maturin 0.14.0 or whichever it was that preceded Metadata 2.2 (and Metadata 2.3), but Metadata 2.2 and 2.3 are actually good, and soon Poetry and Hatch and others will build source distributions with Metadata 2.2 by default.

---

_Comment by @charliermarsh on 2024-03-17 21:38_

I'm gonna say for now that we won't roll this back, but if it continues to be a problem for an extended period of time let me know and we can reconsider.

---

_Closed by @charliermarsh on 2024-03-17 21:38_

---
