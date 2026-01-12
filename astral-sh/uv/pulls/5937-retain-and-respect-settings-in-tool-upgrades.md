```yaml
number: 5937
title: Retain and respect settings in tool upgrades
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/index-receipt
created_at: 2024-08-08T21:32:00Z
updated_at: 2024-08-09T18:23:59Z
url: https://github.com/astral-sh/uv/pull/5937
synced_at: 2026-01-12T16:07:07Z
```

# Retain and respect settings in tool upgrades

---

_@charliermarsh_

## Summary

We now persist the `ResolverInstallerOptions` when writing out a tool receipt. When upgrading, we grab the saved options, and merge with the command-line arguments and user-level filesystem settings (CLI > receipt > filesystem).


---

_Review requested from @zanieb by @charliermarsh on 2024-08-08 21:32_

---

_Review requested from @konstin by @charliermarsh on 2024-08-08 21:32_

---

_Label `enhancement` added by @charliermarsh on 2024-08-08 21:32_

---

_Label `preview` added by @charliermarsh on 2024-08-08 21:32_

---

_@charliermarsh reviewed on 2024-08-08 21:32_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:2520 on 2024-08-08 21:32_

I don't know, I guess? So the receipt represents the "most recent" settings.

---

_@charliermarsh reviewed on 2024-08-08 21:32_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:2504 on 2024-08-08 21:32_

Hard to say what the right behavior is here. We could also consider discarding the tool whenever settings differ.

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:1252 on 2024-08-08 21:43_

We go through some effort to avoid adding `--upgrade` to tool receipts. Does that seem right? Otherwise, `uv tool install black --upgrade` saves `upgrade` in the receipt...

---

_@charliermarsh reviewed on 2024-08-08 21:43_

---

_Review comment by @konstin on `crates/uv/tests/tool_install.rs`:2520 on 2024-08-09 08:00_

fwiw for the lockfile we only write non-default values

---

_@konstin approved on 2024-08-09 08:00_

---

_Comment by @zanieb on 2024-08-09 13:42_

I will review this morning.

---

_Comment by @charliermarsh on 2024-08-09 13:44_

Would most appreciate review of the correct / desired behaviors.

---

_@zanieb reviewed on 2024-08-09 14:00_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:2434 on 2024-08-09 14:00_

You mean `flask`?
```suggestion
    // Install `flask`
```

---

_@zanieb reviewed on 2024-08-09 14:01_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:2470 on 2024-08-09 14:01_

Aside: We should probably open an issue to define a macro for this, since it's non-trivial and repeated.

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:2470 on 2024-08-09 17:20_

Yeah good idea. I can do it.

---

_@charliermarsh reviewed on 2024-08-09 17:20_

---

_Comment by @zanieb on 2024-08-09 17:23_

Sorry got half way through this then had meetings and forgot — reading now.

---

_@zanieb reviewed on 2024-08-09 17:25_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:1252 on 2024-08-09 17:25_

Yes that would be weird.

---

_@zanieb reviewed on 2024-08-09 17:27_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:2520 on 2024-08-09 17:27_

I think updating here makes sense.

---

_@zanieb reviewed on 2024-08-09 17:28_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:2504 on 2024-08-09 17:28_

This is really tough. It seems okay to change after some user feedback. I'm leaning towards consistent `--upgrade` / `--reinstall` semantics where it is required to change the versions. This matches `uv pip install`, yes?

---

_@charliermarsh reviewed on 2024-08-09 17:36_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:2504 on 2024-08-09 17:36_

Yeah

---

_@charliermarsh reviewed on 2024-08-09 17:37_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:2504 on 2024-08-09 17:37_

Now that we have `upgrade`, I'm sort of open to making `install` more aggressive about replacing though.

---

_@zanieb reviewed on 2024-08-09 17:42_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:2504 on 2024-08-09 17:42_

That makes sense too.

---

_@zanieb approved on 2024-08-09 17:43_

---

_@charliermarsh reviewed on 2024-08-09 17:59_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:2504 on 2024-08-09 17:59_

So we could make it replace whenever the settings or versions differ...

---

_Merged by @charliermarsh on 2024-08-09 18:21_

---

_Closed by @charliermarsh on 2024-08-09 18:21_

---

_Branch deleted on 2024-08-09 18:21_

---

_Comment by @codspeed-hq[bot] on 2024-08-09 18:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/index-receipt)

### Merging #5937 will **degrade performances by 11.18%**

<sub>Comparing <code>charlie/index-receipt</code> (4cf3560) with <code>main</code> (4df0fe9)</sub>



### Summary

`❌ 2` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/index-receipt)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/index-receipt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_tag_compatibility[flyte-long-incompatible]` | 1.4 µs | 1.6 µs | -11.15% |
| ❌ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 926.9 ns | 1,043.6 ns | -11.18% |


---
