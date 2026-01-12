```yaml
number: 5756
title: "Respect malformed `.dist-info` directories in tool installs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/babel
created_at: 2024-08-03T23:23:05Z
updated_at: 2024-08-05T18:23:52Z
url: https://github.com/astral-sh/uv/pull/5756
synced_at: 2026-01-12T16:06:59Z
```

# Respect malformed `.dist-info` directories in tool installs

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5749.


---

_Label `bug` added by @charliermarsh on 2024-08-03 23:23_

---

_Label `preview` added by @charliermarsh on 2024-08-03 23:23_

---

_Marked ready for review by @charliermarsh on 2024-08-03 23:23_

---

_Merged by @charliermarsh on 2024-08-03 23:43_

---

_Closed by @charliermarsh on 2024-08-03 23:43_

---

_Branch deleted on 2024-08-03 23:43_

---

_Comment by @CoolCat467 on 2024-08-05 16:11_

https://github.com/python-babel/babel/pull/1110 fixed the original issue. While inconvenient, I personally think that respecting malformed `.dist-info` installation should be behind a flag or something instead of being default

---

_Comment by @charliermarsh on 2024-08-05 16:24_

I think it's arguably standards-incompliant to reject non-normalized dist-info directories.

---

_Comment by @RomainBrault on 2024-08-05 17:27_

Putting this behavior behind a flag could be beneficial in terms of prevention of malicious typo-squatting.

Or maybe display a warning say more or less "The .dist-info is malformed. Be careful it might be malicious. If the package is legit please raise an issue to the authors/maintainers" ?

---

_Comment by @charliermarsh on 2024-08-05 18:23_

To be clear, we do error if the normalized filenames don't match. We just don't require that the name on the directory is normalized already.

---
