```yaml
number: 15240
title: "Allow `uv python install --default` to work with pre-release versions"
type: pull_request
state: open
author: tdhopper
labels: []
assignees: []
base: main
head: default-with-release-candidates
created_at: 2025-08-12T15:09:16Z
updated_at: 2025-08-12T20:13:22Z
url: https://github.com/astral-sh/uv/pull/15240
synced_at: 2026-01-12T16:11:39Z
```

# Allow `uv python install --default` to work with pre-release versions

---

_@tdhopper_

_I am not a rust dev and this is my first attempted contribution. Feel free to ignore!_


## Summary

Currently the `--default` flag for `uv python install` fails silently when the users specifies 3.14; 3.14rc1 is installed but not made available on the users path. This change correctly sets the release candidate as system Python (i.e. adds `python` and `python3` binaries for 3.14 to the path).

Incorrect behavior was a result of the overly strict check that would not set the pre-release versions as a default.

Resolves https://github.com/astral-sh/uv/issues/15237

## Test Plan

* Added unit tests for this behavior. 
  *  Test behavior may be limited in that once 3.14 is no longer pre-release, the test won't cover the desired behavior (that pre-releases can be set at default). I'm not sure how to work around that.
* Tested locally to verify that `uv python install --default 3.14` works to set 3.14 as my system python.

---

_@zanieb reviewed on 2025-08-12 17:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:761 on 2025-08-12 17:00_

I'm trying to wrap my head around why all of these checks are necessary.

e.g., why do we need to check `default` here? That's overlapping with the subsequent `let targets` logic. Does the behavior change if you remove it?

Could the entire change just be `first_request.download.key() == installation.key()`? Are there edge-cases that break from that?







---

_Comment by @zanieb on 2025-08-12 17:01_

Thanks for the PR!

---

_Assigned to @zanieb by @zanieb on 2025-08-12 17:01_

---

_@tdhopper reviewed on 2025-08-12 17:34_

---

_Review comment by @tdhopper on `crates/uv/src/commands/python/install.rs`:761 on 2025-08-12 17:34_

Great questions, thanks. I'm trying that and have a few test failures. Trying to dig into those.

---

_@zanieb reviewed on 2025-08-12 17:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:761 on 2025-08-12 17:39_

Okay! It's entirely plausible we do need more logic, it's just helpful to understand the reasons. Let me know if you get stuck and I can take a look.

---

_@tdhopper reviewed on 2025-08-12 19:04_

---

_Review comment by @tdhopper on `crates/uv/src/commands/python/install.rs`:761 on 2025-08-12 19:04_

@zanieb it doesn't work with just `first_request.download.key() == installation.key()`. I _think_ we need this  logic, but my understanding of how installation actually works is a bit limited. If you could take a look at it, I'd appreciate it.

---

_Renamed from "Allow uv python install --default to work with pre-release versions" to "Allow `uv python install --default` to work with pre-release versions" by @tdhopper on 2025-08-12 20:13_

---
