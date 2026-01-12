```yaml
number: 16676
title: "Add support for `--upgrade` in `uv python install`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/install-upgrade
created_at: 2025-11-10T23:37:31Z
updated_at: 2025-11-13T15:55:49Z
url: https://github.com/astral-sh/uv/pull/16676
synced_at: 2026-01-12T16:12:23Z
```

# Add support for `--upgrade` in `uv python install`

---

_@zanieb_

This allows us to suggest `uv python install --upgrade 3.14` as the canonical way to get the latest patch version of a given Python regardless of whether it is installed already. Currently, you can do `uv python upgrade 3.14` and it will install it, but I'd like to remove that behavior as I find it very surprising.

---

_Label `cli` added by @zanieb on 2025-11-10 23:37_

---

_Marked ready for review by @zanieb on 2025-11-11 17:19_

---

_Label `preview` added by @zanieb on 2025-11-11 17:20_

---

_Comment by @codspeed-hq[bot] on 2025-11-11 17:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Finstall-upgrade?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16676 will **not alter performance**

<sub>Comparing <code>zb/install-upgrade</code> (7b05e36) with <code>main</code> (e28dc62)</sub>



### Summary

`✅ 6` untouched  





---

_Review requested from @konstin by @zanieb on 2025-11-12 13:55_

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:3789 on 2025-11-13 11:13_

Upgrade without an explicit version has a quirk: When pinning a patch version in `.python_versions`, it errors. I expect that some users will do `uv python install --upgrade` by default because they want to have the latest patch version, and will get confused by this too, especially when they don't realize that there's a `.python_versions`.

```
$ uv python install --upgrade
error: `uv python install --upgrade` only accepts minor versions, got: 3.14.0
```

There's a workflow that's between `uv python install` and `uv python upgrade`: I want to install Python at the version that the project requires, upgraded to the latest patch version if not already at that, unless the project pins a patch version. Trying to emulate that with `uv python install --upgrade` can lead to the error above.

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:309 on 2025-11-13 11:25_

Review note: This is reachable by creating an empty `.python_versions` file (unlike the branch above, which by default looks at installed Pythons)

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:3797 on 2025-11-13 11:40_

Another quirk: The output log for plain without any pin is the same with and without upgrade. I'd have expected something like "latest patch version Python 3.14.0 is already installed".

```
$ uv python install
Python is already installed. Use `uv python install <request>` to install another version.
$ uv python install --upgrade
Python is already installed. Use `uv python install <request>` to install another version.
```

Similar to what an explicit request tells you:

```
$ uv python install --upgrade 3.14
Python 3.14 is already on the latest supported patch release
```

It's pre-existing, but it would also be nice if `uv python install` and `uv python install --upgrade 3.14` would tell me the version that is installed that they consider latest.

---

_@konstin approved on 2025-11-13 11:45_

---

_@zanieb reviewed on 2025-11-13 14:02_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:3789 on 2025-11-13 14:02_

Ah thanks. I'll look into that.

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:3789 on 2025-11-13 14:02_

I think it might just need to be a hint when the request comes from a `.python-version` file?

---

_@zanieb reviewed on 2025-11-13 14:02_

---

_@zanieb reviewed on 2025-11-13 14:03_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:3797 on 2025-11-13 14:03_

I think I might expect this to error asking for a request? but you're right the message should change if we don't.

---

_@konstin reviewed on 2025-11-13 14:21_

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:3789 on 2025-11-13 14:21_

A hint would do it.

---

_@konstin reviewed on 2025-11-13 14:22_

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:3797 on 2025-11-13 14:22_

Banning `uv python install --upgrade` without a python input from anywhere sounds good to me.

---

_@zanieb reviewed on 2025-11-13 14:30_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:3789 on 2025-11-13 14:30_

I think we can do even better, but I'll plan to do that in a follow-up.

---

_Merged by @zanieb on 2025-11-13 15:55_

---

_Closed by @zanieb on 2025-11-13 15:55_

---

_Branch deleted on 2025-11-13 15:55_

---
