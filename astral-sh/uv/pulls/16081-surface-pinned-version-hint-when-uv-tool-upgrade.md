```yaml
number: 16081
title: "Surface pinned-version hint when `uv tool upgrade` can’t move the tool"
type: pull_request
state: merged
author: terror
labels: []
assignees: []
merged: true
base: main
head: hint-for-pinned-upgrade
created_at: 2025-10-01T01:26:49Z
updated_at: 2025-10-07T16:18:40Z
url: https://github.com/astral-sh/uv/pull/16081
synced_at: 2026-01-12T16:12:06Z
```

# Surface pinned-version hint when `uv tool upgrade` can’t move the tool

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/15665

`uv tool upgrade` already respects version pins, but when a tool was installed with an exact version the command quietly becomes a no-op even though users expect it to upgrade the executable. This change tweaks the upgrade flow to detect that situation by inspecting the stored receipt and, whenever the tool stays pinned, emit a concise hint explaining why the version didn’t move and how to reinstall without the pin. 

The message still appears if the run only refreshed supporting packages, so users aren’t misled by dependency churn that leaves the tool itself untouched. 

I also added an integration test for the scenario end to end by installing `babel==2.6.0`, attempting an upgrade, and asserting that the hint is shown alongside the dependency updates.


---

_Marked ready for review by @terror on 2025-10-01 01:51_

---

_Assigned to @zanieb by @zanieb on 2025-10-01 03:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:215 on 2025-10-01 20:12_

```suggestion
            "hint: `{}` is pinned to `{}` (installed with an exact version pin); reinstall with `{}` to upgrade to a new version",
```

---

_@zanieb reviewed on 2025-10-01 20:12_

---

_@zanieb reviewed on 2025-10-01 20:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:211 on 2025-10-01 20:13_

I think you can remove some of the whitespace here, it's a bit much :)

---

_@zanieb reviewed on 2025-10-01 20:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:439 on 2025-10-01 20:15_

I find this change in signature a little awkward. I think I'd expect the version to be attached to the `UpgradeOutcome` variant rather than as a part of the signature of this function?

---

_@zanieb reviewed on 2025-10-01 20:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:439 on 2025-10-01 20:23_

(especially if it's only possible for a subset of the variants)

---

_Review comment by @terror on `crates/uv/src/commands/tool/upgrade.rs`:439 on 2025-10-01 20:23_

Yeah agreed, I'll change this up!

---

_@terror reviewed on 2025-10-01 20:23_

---

_@terror reviewed on 2025-10-01 20:48_

---

_Review comment by @terror on `crates/uv/src/commands/tool/upgrade.rs`:439 on 2025-10-01 20:48_

After implementing it still looked a bit questionable, so I've lifted out a general `UpgradeReason` (we could probably name this to something better) I put onto an `UpgradeReport`. This makes it so we can easily extend `UpgradeReason` for other things. What do you think?

---

_@zanieb reviewed on 2025-10-07 13:30_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:487 on 2025-10-07 13:30_

It'd be weird, but wouldn't this miss a pin mixed with other constraints, e.g., `>=0.1, ==0.2`? Would it make more sense to search for a `Operator::Equal` in the set of specifiers?

---

_@zanieb reviewed on 2025-10-07 13:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:232 on 2025-10-07 13:32_

I think we'd normally just shadow the existing variable instead of appending `_str` to the name.

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:208 on 2025-10-07 13:33_

I think we need to print a newline here if there are any collected constraints so there's a newline between the content and the first `hint: `

---

_@zanieb reviewed on 2025-10-07 13:33_

---

_@zanieb reviewed on 2025-10-07 13:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:256 on 2025-10-07 13:35_

There's still something awkward about this abstraction, but 🤷‍♀️ I'm not having any great ideas so we can leave it until we have another use-case that helps us refactor.

@konstin might have an idea.

---

_@zanieb reviewed on 2025-10-07 13:36_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:256 on 2025-10-07 13:36_

Previous conversation at https://github.com/astral-sh/uv/pull/16081#discussion_r2395865728

---

_@zanieb approved on 2025-10-07 13:36_

Overall, this looks good — I just have some nits left. Thanks!

---

_Review comment by @terror on `crates/uv/src/commands/tool/upgrade.rs`:487 on 2025-10-07 15:04_

Ah yeah interesting, I was able to break it by specifying `babel>=2.0,==2.6.0`, although not sure you see this much in the wild 😅

---

_@terror reviewed on 2025-10-07 15:04_

---

_@konstin reviewed on 2025-10-07 16:17_

---

_Review comment by @konstin on `crates/uv/src/commands/tool/upgrade.rs`:256 on 2025-10-07 16:17_

I don't have a better idea either.

---

_Merged by @zanieb on 2025-10-07 16:18_

---

_Closed by @zanieb on 2025-10-07 16:18_

---
