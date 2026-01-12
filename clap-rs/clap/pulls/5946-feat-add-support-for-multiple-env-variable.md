```yaml
number: 5946
title: "Feat: add support for multiple env variable"
type: pull_request
state: open
author: fahri-r
labels: []
assignees: []
draft: true
base: master
head: 5925-envs
created_at: 2025-03-10T12:26:06Z
updated_at: 2025-04-16T12:18:21Z
url: https://github.com/clap-rs/clap/pull/5946
synced_at: 2026-01-12T16:14:17Z
```

# Feat: add support for multiple env variable

---

_@fahri-r_

#5925 

I added envs method that accept Vec<OsStr> for the params. 
If user has two environment variables and the first one has a value, then the first one will be picked. But if the first one not have a value, then the second one will be picked.

---
