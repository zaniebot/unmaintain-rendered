---
number: 13508
title: installing a workspace with workspace dependency groups
type: issue
state: closed
author: kapilt
labels:
  - question
assignees: []
created_at: 2025-05-17T15:31:11Z
updated_at: 2025-05-18T13:02:36Z
url: https://github.com/astral-sh/uv/issues/13508
synced_at: 2026-01-10T01:25:34Z
---

# installing a workspace with workspace dependency groups

---

_Issue opened by @kapilt on 2025-05-17 15:31_

### Question

I'm trying to use a workspace to install a set of packages for the purpose of running ci/unittests, but It doesn't seem like we can install dependency groups like dev for the workspace dependencies, when doing so, because that only applies to the workspace, not to its transitive dependencies that are in the workspace. 

is there a way to specify that we want optional dependency groups when a dependency via a workspace installing?



### Platform

_No response_

### Version

uv 0.7.3 (Homebrew 2025-05-07)

---

_Label `question` added by @kapilt on 2025-05-17 15:31_

---

_Renamed from "installing a workspace with workspace groups" to "installing a workspace with workspace dependency groups" by @kapilt on 2025-05-17 15:34_

---

_Comment by @kapilt on 2025-05-17 15:37_

if its helpful here is the workspace definition I'm using basically its a virtual workspace
https://github.com/cloud-custodian/cloud-custodian/pull/10140/files#diff-953ec993435fd232c21d7c3ee0204c12a1a877c64dde528a1d07f30163aa5674

```
layout
   /top_level_package (c7n, part of workspace)
      /workspace ( virtual workspace with dependencies on all workspace packages)
      /tools
        /c7n_gcp (add-on package for c7n, part of workspace)
```

all the packages define dev dependency groups that need to be installed for ci to run, trying to understand how to make this work with uv. it seems uv sync is optionated to automatically remove packages in the virtualenv that are not related to the current package's sync operation, so we need someway to encapsulate installing all packages and their dev group dependencies to be able to run ci from a top level command (makefile target ~ make test)
 

---

_Referenced in [cloud-custodian/cloud-custodian#10140](../../cloud-custodian/cloud-custodian/pulls/10140.md) on 2025-05-17 15:40_

---

_Comment by @kapilt on 2025-05-18 13:02_

using --all-packages and --group dev seems to do the right thing (tm) ;-)

---

_Closed by @kapilt on 2025-05-18 13:02_

---
