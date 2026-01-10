```yaml
number: 6701
title: "--require-hashes option does not work as with pip"
type: issue
state: closed
author: thierryba
labels:
  - compatibility
assignees: []
created_at: 2024-08-27T16:54:04Z
updated_at: 2024-08-28T01:13:24Z
url: https://github.com/astral-sh/uv/issues/6701
synced_at: 2026-01-10T04:45:09Z
```

# --require-hashes option does not work as with pip

---

_Issue opened by @thierryba on 2024-08-27 16:54_

so I was trying to install my requirements.txt with uv pip install --require-hashes requirements.txt

My file contains urls with hashes. Example: borgbackup @ https://3rdparty.s3.amazonaws.com/borgbackup-1.2.8-cp312-cp312-macosx_11_0_universal2.whl#sha256=aca0a62c0e359b7663ebd4ee65e7bf7cdcbaecc8a001838eb49e8c886948ae6d

But uv is complaining saying that there is no hash. Is this expected?

---

_Comment by @charliermarsh on 2024-08-27 16:56_

What does the `requirements.txt` contain exactly, in the above command?

---

_Comment by @thierryba on 2024-08-27 19:27_

@charliermarsh, this:
 `borgbackup @ https://3rdparty.s3.amazonaws.com/borgbackup-1.2.8-cp312-cp312-macosx_11_0_universal2.whl#sha256=aca0a62c0e359b7663ebd4ee65e7bf7cdcbaecc8a001838eb49e8c886948ae6d`

---

_Comment by @charliermarsh on 2024-08-27 19:36_

Right now you need to do:

```
borgbackup @ https://3rdparty.s3.amazonaws.com/borgbackup-1.2.8-cp312-cp312-macosx_11_0_universal2.whl --hash sha256:aca0a62c0e359b7663ebd4ee65e7bf7cdcbaecc8a001838eb49e8c886948ae6d
```

But I thought we supported reading hashes from the fragment directly. I'll look into it.

---

_Label `compatibility` added by @charliermarsh on 2024-08-27 20:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-27 20:03_

---

_Closed by @charliermarsh on 2024-08-28 00:03_

---

_Closed by @charliermarsh on 2024-08-28 00:03_

---

_Comment by @charliermarsh on 2024-08-28 01:13_

Fixed in the next release.

---
