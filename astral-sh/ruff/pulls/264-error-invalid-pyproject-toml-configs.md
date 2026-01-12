```yaml
number: 264
title: error invalid pyproject.toml configs
type: pull_request
state: merged
author: adriangb
labels: []
assignees: []
merged: true
base: main
head: error-invalid-config
created_at: 2022-09-23T22:04:21Z
updated_at: 2022-09-26T11:26:24Z
url: https://github.com/astral-sh/ruff/pull/264
synced_at: 2026-01-12T15:55:04Z
```

# error invalid pyproject.toml configs

---

_@adriangb_

Fixes #262 

---

_@charliermarsh reviewed on 2022-09-23 22:10_

---

_Review comment by @charliermarsh on `src/pyproject.rs`:22 on 2022-09-23 22:10_

Can we get rid of this case, and just replace it with `parse_pyproject_toml(pyproject).map(...)?` or similar? (Right now, all this buys us is the `println!("Failed to load pyproject.toml: {:?}", e);`, but the programs gonna fail anyway.)

---

_Comment by @cclauss on 2022-09-23 23:19_

https://github.com/abravalheri/validate-pyproject can run in pre-commit or CI to find such problems.

---

_Merged by @charliermarsh on 2022-09-24 01:16_

---

_Closed by @charliermarsh on 2022-09-24 01:16_

---

_Comment by @adriangb on 2022-09-24 01:18_

Thanks for cleaning it up I was trying to fix it from my 📞 

---

_Branch deleted on 2022-09-24 01:20_

---

_Comment by @charliermarsh on 2022-09-24 01:24_

No prob! Thanks for the PR!


> On Sep 23, 2022, at 9:18 PM, Adrian Garcia Badaracco ***@***.***> wrote:
> 
> ﻿
> Thanks for cleaning it up I was trying to fix it from my 📞
> 
> —
> Reply to this email directly, view it on GitHub, or unsubscribe.
> You are receiving this because you modified the open/close state.


---

_Comment by @amotl on 2022-09-26 11:26_

The improvement works excellently. Thanks a stack!

- https://github.com/earthobservations/wetterdienst/pull/729

---
