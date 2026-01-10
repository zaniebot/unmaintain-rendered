```yaml
number: 5997
title: Use upgrade-specific output for tool upgrade
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/upgrade-output
created_at: 2024-08-10T17:30:49Z
updated_at: 2024-08-11T00:24:52Z
url: https://github.com/astral-sh/uv/pull/5997
synced_at: 2026-01-10T13:31:54Z
```

# Use upgrade-specific output for tool upgrade

---

_Pull request opened by @charliermarsh on 2024-08-10 17:30_

## Summary

Closes https://github.com/astral-sh/uv/issues/5949.


---

_Marked ready for review by @charliermarsh on 2024-08-10 17:30_

---

_Label `cli` added by @charliermarsh on 2024-08-10 17:30_

---

_Label `preview` added by @charliermarsh on 2024-08-10 17:30_

---

_@zanieb reviewed on 2024-08-10 17:34_

---

_Review comment by @zanieb on `crates/uv/tests/tool_upgrade.rs`:335 on 2024-08-10 17:34_

Unfortunately I find this a little confusing. At first, I thought the other things being modified were _other_ tools. I think what I'd expected to see is

```
Updated babel v2.6.0 -> v2.13.1
- babel==2.6.0
+ babel==2.13.1
- pytz==2018.5
+ setuptools==69.2.0
```

Or maybe some other way of nesting the environment modifications separately from the main tool itself? This would help in the `upgrade --all` case as well.

Do we need to show the environment changes by default? It seems like a verbose output thing.


---

_@charliermarsh reviewed on 2024-08-10 17:37_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_upgrade.rs`:335 on 2024-08-10 17:37_

And if the tool itself isn't updated?

---

_@charliermarsh reviewed on 2024-08-10 17:38_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_upgrade.rs`:222 on 2024-08-10 17:38_

I'm not really a big fan of showing this when we didn't update anything.

---

_@charliermarsh reviewed on 2024-08-10 17:38_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_upgrade.rs`:335 on 2024-08-10 17:38_

(It's fine, it's very easy to change.)

---

_@charliermarsh reviewed on 2024-08-10 17:43_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_upgrade.rs`:133 on 2024-08-10 17:43_

I wonder if we should _not_ show this for the target package...? Since it's already summarized above?

---

_Review requested from @zanieb by @charliermarsh on 2024-08-10 17:57_

---

_@zanieb reviewed on 2024-08-10 18:33_

---

_Review comment by @zanieb on `crates/uv/tests/tool_upgrade.rs`:335 on 2024-08-10 18:33_

Ah I forgot about dependency updates. That does make it a little trickier to summarize.

I guess the summary line could be "Updated babel environment"? My only concern is that they may be confused as to why the tool itself wasn't updated. Seems verbose to clarify though.

I also considered "Updated babel dependencies" but this includes `--with` so that's not quite correct.

---

_@zanieb reviewed on 2024-08-10 18:34_

---

_Review comment by @zanieb on `crates/uv/tests/tool_upgrade.rs`:133 on 2024-08-10 18:34_

I was on the fence in my suggestion, I don't have a strong preference. It sort of grounds the changes we're showing?

---

_@charliermarsh reviewed on 2024-08-10 18:44_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_upgrade.rs`:133 on 2024-08-10 18:44_

Yeah I don't feel strongly. Seems ok to leave it for now.

---

_@charliermarsh reviewed on 2024-08-10 18:48_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_upgrade.rs`:335 on 2024-08-10 18:48_

Sounds good. I think your format is good and neatly solves for multiple tool updates.

---

_Merged by @charliermarsh on 2024-08-11 00:24_

---

_Closed by @charliermarsh on 2024-08-11 00:24_

---

_Branch deleted on 2024-08-11 00:24_

---
