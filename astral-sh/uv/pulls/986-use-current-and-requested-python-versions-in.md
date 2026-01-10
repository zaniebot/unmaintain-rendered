```yaml
number: 986
title: "Use current and requested Python versions in `requires-python` incompatibility errors"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/python
created_at: 2024-01-19T00:33:06Z
updated_at: 2024-01-22T06:32:04Z
url: https://github.com/astral-sh/uv/pull/986
synced_at: 2026-01-10T15:39:03Z
```

# Use current and requested Python versions in `requires-python` incompatibility errors

---

_Pull request opened by @zanieb on 2024-01-19 00:33_

Closes https://github.com/astral-sh/puffin/issues/806


---

_Label `error messages` added by @zanieb on 2024-01-19 00:33_

---

_@zanieb reviewed on 2024-01-19 00:35_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install_scenarios.rs`:2890 on 2024-01-19 00:35_

Well this message didn't get worse but what the heck is it saying? TODO create an issue tracking this bug

---

_@zanieb reviewed on 2024-01-19 00:40_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:42 on 2024-01-19 00:40_

Open to suggestions here, I tried quite a few things e.g.

"the current Python version (3.9) does not satisfy"
"the current Python==3.9 does not satisfy"
"the resolution uses Python 3.9 which does not satisfy"

---

_@charliermarsh reviewed on 2024-01-19 03:49_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:38 on 2024-01-19 03:49_

I think this actually _isn't_ true because if you use `--python-version`, we'll have both the installed and the target version in the tree, and either one could be incompatible (or both). So we might want to make it explicit that this only works if `--python-version` isn't provided (and remove the debug assert), or find a way to accommodate that.

---

_@zanieb reviewed on 2024-01-19 04:52_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:38 on 2024-01-19 04:52_

Ah gee... we should accommodate that somehow thanks for catching it.

---

_@zanieb reviewed on 2024-01-19 20:26_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:38 on 2024-01-19 20:26_

Addressed by https://github.com/astral-sh/puffin/pull/986/commits/eeadecdedef42e899ccfd41bbe79c829456727e8

---

_Renamed from "Use current Python version in `requires-python` incompatibility errors" to "Use current and requested Python versions in `requires-python` incompatibility errors" by @zanieb on 2024-01-19 21:29_

---

_Review requested from @charliermarsh by @zanieb on 2024-01-19 22:33_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:136 on 2024-01-20 12:31_

Oh yeah, that's tedious (that `PythonRequirement` uses unowned data). Perhaps we should just change it to use owned data, it's just two versions.

---

_@charliermarsh reviewed on 2024-01-20 12:31_

---

_@charliermarsh reviewed on 2024-01-20 12:36_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-20 12:36_

Are they not _exactly_ the same when not provided? I think I would've expected `--python-version 3.10` to be treated as `--python-version 3.10.0`. I'm just wondering if this will be correct if the user uses a different patch version in the resolution.

---

_@charliermarsh reviewed on 2024-01-20 12:36_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:136 on 2024-01-20 12:36_

I would be fine making it owned.

---

_@charliermarsh approved on 2024-01-20 12:37_

Looks good to me though one question about the `.take(2)` that I'd like to understand better before merging.

---

_@zanieb reviewed on 2024-01-21 17:19_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-21 17:19_

If  a target Python version is provided e.g. `--python-version 3.11`

> DEBUG puffin_resolver::resolver Solving with target Python 3.11.6 and installed Python 3.12.0

If the target Python version is inferred from the current version

> DEBUG puffin_resolver::resolver Solving with target Python 3.12 and installed Python 3.12.0

If a target Python version is provided with patch version e.g. `--python-version 3.11.3`

> DEBUG puffin_resolver::resolver Solving with target Python 3.11.3 and installed Python 3.12.0

If a target Python version is provided but not installed e.g. `--python-version 3.13`

> DEBUG puffin_resolver::resolver Solving with target Python 3.13 and installed Python 3.12.0

---

_@zanieb reviewed on 2024-01-21 17:22_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-21 17:22_

We compare the _full_ target to the first two numbers of the _installed_ so we should only have equality here when `--python-version` is not provided.

Let me know if I can make the this clearer in my comment.

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-21 18:01_

These are helpful...

(So first, I am assuming that `3.12` and `3.12.0` are considered equal when you use `==` with `Version`. I'm pretty sure this is true and per spec. In that case, we shouldn't need to do the `.take(2)` thing.)

> If a target Python version is provided e.g. `--python-version 3.11`

I initially thought this one was a bug, because 3.11 should've resolved to 3.11.0. But then I realized we do a thing whereby we assume the _highest_ known minor. That behavior kind of feels wrong in hindsight... I think it exists because `--python-version 3.7` would otherwise use `3.7.0`, and a lot of things aren't compatible with the first few patch releases in `3.7`, and that was a confusing user experience in my testing. But it might be wrong...

> If the target Python version is inferred from the current version

So these should match based on `Version` equality, I think.

> If a target Python version is provided with patch version e.g. `--python-version 3.11.3`

These should be considered unequal based on `Version` equality.

> If a target Python version is provided but not installed e.g. `--python-version 3.13`

This seems right to me, they'd be considered unequal based on version equality. But it shouldn't have to do with whether the target is installed.


---

_@charliermarsh reviewed on 2024-01-21 18:01_

---

_@charliermarsh reviewed on 2024-01-21 18:06_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-21 18:06_

Which case specifically would be wrong if we used `==` instead of `take(2)`?

---

_@zanieb reviewed on 2024-01-21 23:27_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-21 23:27_

Uhh so there's a lot here but... it's only 3.12.0 in those examples because that happens to be the patch version of Python 3.12 I have installed. So... if I had 3.12.1 installed and did _not_ specify a `--python-version` for resolution we'd see `3.12.1 != 3.12` and would incorrectly assume the user had specified a different version and display the wrong message. For example, take a look at this commit that uses the equality comparison you are suggesting: https://github.com/astral-sh/puffin/commit/044c354647607d8dbb379b8be27f546afc664a9a

> If a target Python version is provided with patch version e.g. --python-version 3.11.3

This _is_ being handled correctly even with `take(2)` because we _only_ trim the _installed_ Python version so if the user has provided a resolution version with a patch version they will be considered inequal.


---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-22 00:14_

Why would we see `3.12.1 != 3.12` though? Why is one of the versions not inclusive of the patch, when you _don't_ provide a `--python-version`? Isn't that wrong?

---

_@charliermarsh reviewed on 2024-01-22 00:14_

---

_@zanieb reviewed on 2024-01-22 01:45_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-22 01:45_

I have no idea why it's implemented that way, but yes as noted above if the target version is inferred from the current Python version it will not include the patch version. I'm all for changing that behavior if it was not intentional, but it doesn't seem like we should do it here.

Here's another commit showing the installed version alongside the target one: https://github.com/astral-sh/puffin/commit/43910a8e

---

_@charliermarsh reviewed on 2024-01-22 01:58_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-22 01:58_

Personally, I think the right order of operations is to fix the missing patch version (in a separate PR) and then rebase this on top of that. Otherwise, we're adding a workaround for a bug that we need to fix immediately. But feel free to merge, I see the issue and can fix now.

---

_@charliermarsh reviewed on 2024-01-22 02:02_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-22 02:02_

Tossed something up here 🤞 https://github.com/astral-sh/puffin/pull/1033

---

_@zanieb reviewed on 2024-01-22 06:31_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:48 on 2024-01-22 06:31_

Thanks for fixing it!

---

_Merged by @zanieb on 2024-01-22 06:32_

---

_Closed by @zanieb on 2024-01-22 06:32_

---

_Branch deleted on 2024-01-22 06:32_

---
