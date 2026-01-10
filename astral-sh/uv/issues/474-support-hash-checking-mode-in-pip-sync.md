```yaml
number: 474
title: "Support hash-checking mode in `pip-sync`"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - security
assignees: []
created_at: 2023-11-21T13:19:41Z
updated_at: 2024-04-10T19:09:05Z
url: https://github.com/astral-sh/uv/issues/474
synced_at: 2026-01-10T05:40:31Z
```

# Support hash-checking mode in `pip-sync`

---

_Issue opened by @konstin on 2023-11-21 13:19_

See https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode

- [x] #131 
- [ ] Read hashes from `requirements.txt` format (https://pip.pypa.io/en/stable/reference/requirements-file-format/#per-requirement-options)
- [ ] Compute the sha256 when downloading a distribution (both source dist and wheel), store them in the cache (and make sure to keep in sync with cache invalidation) or check that they match the `File` description (TODO: Does this have a perf impact? If yes, do we always want to do this or only if the registry doesn't tell us the sha?)
- [ ] When installing, check the hashes
  - [ ] Ignore distribution with mismatching hashes: A better matching wheel might have been uploaded since the lockfile was created, but we have to ignore it in hash checking more and fall back to the next file. Report when there is no distribution because non matched the hashes (but would without hashes)


---

_Label `enhancement` added by @charliermarsh on 2024-01-11 17:24_

---

_Renamed from "Support Hash-checking mode in `pip-sync`" to "Support hash-checking mode in `pip-sync`" by @charliermarsh on 2024-01-11 17:27_

---

_Label `wish` added by @charliermarsh on 2024-01-11 17:30_

---

_Added to milestone `Feature complete` by @charliermarsh on 2024-01-11 17:30_

---

_Label `wish` removed by @zanieb on 2024-02-17 04:18_

---

_Label `security` added by @zanieb on 2024-02-17 04:18_

---

_Comment by @charliermarsh on 2024-04-04 20:55_

I've started to work on this. Doing something I've never done, sharing my existing notes on how it works: https://astral-sh.notion.site/Hash-checking-in-uv-fd27f5a51e8f4f5f8547a183fbf3e006

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-04 20:55_

---

_Closed by @charliermarsh on 2024-04-10 19:09_

---
