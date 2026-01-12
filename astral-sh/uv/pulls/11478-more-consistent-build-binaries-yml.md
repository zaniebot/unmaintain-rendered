```yaml
number: 11478
title: "More consistent `build-binaries.yml`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/refactor-build-binaries
created_at: 2025-02-13T12:55:56Z
updated_at: 2025-02-18T17:36:30Z
url: https://github.com/astral-sh/uv/pull/11478
synced_at: 2026-01-12T16:09:51Z
```

# More consistent `build-binaries.yml`

---

_@konstin_

For uv-build, we need to duplicate a lot of the `build-binaries.yml` logic to build another source distribution and wheel. In preparation for that I tried to make the invocations more consistent, to make it easier to review the changes when adding the `uv-build` builds on top.

Split out from #11446

---

_Label `internal` added by @konstin on 2025-02-13 12:55_

---

_Review requested from @zanieb by @konstin on 2025-02-13 12:55_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:55 on 2025-02-13 14:49_

```suggestion
          # We can't use `--find-links` here, since we need maturin, which means no `--no-index`, and without that option
```

---

_@charliermarsh reviewed on 2025-02-13 14:49_

---

_@charliermarsh reviewed on 2025-02-13 14:49_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:469 on 2025-02-13 14:49_

Did you test this change?

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:469 on 2025-02-13 15:10_

No, i only made them for consistency, in case we happen to reactivate it. In that case it would be nice if the re-added code was following the same pattern.

---

_@konstin reviewed on 2025-02-13 15:10_

---

_@zanieb reviewed on 2025-02-13 17:01_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:125 on 2025-02-13 17:01_

Why prefer `--find-links` over installing the wheel directly? 

---

_@zanieb reviewed on 2025-02-13 17:01_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:469 on 2025-02-13 17:01_

It'd be great to fix #11231 :(

---

_@zanieb reviewed on 2025-02-13 17:02_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:592 on 2025-02-13 17:02_

Generally against quoting these, is there a particular motivation for that?

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:618 on 2025-02-13 17:02_

What's the failure mode?

---

_@zanieb reviewed on 2025-02-13 17:02_

---

_@konstin reviewed on 2025-02-14 12:25_

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:125 on 2025-02-14 12:25_

It works independent of the shell used so we can use it for testing on all platforms

---

_@konstin reviewed on 2025-02-14 12:27_

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:592 on 2025-02-14 12:27_

All other uses of `name:` in this file are using quotes, so this change makes them consistent

---

_@konstin reviewed on 2025-02-14 12:32_

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:618 on 2025-02-14 12:32_

A standard `FileNotFoundError`: https://github.com/astral-sh/uv/actions/runs/13288751779/job/37103886648?pr=11446. It's unclear what's the difference between platforms here so I opted for documenting them.

---

_@zanieb reviewed on 2025-02-14 12:51_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:125 on 2025-02-14 12:51_

Can't we just request `shell: sh` and retain the glob? This feels more complicated.

---

_@zanieb reviewed on 2025-02-14 12:52_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:592 on 2025-02-14 12:52_

Sorry you're right, that does seem to be what we do elsewhere. 👍 

---

_@zanieb reviewed on 2025-02-14 12:54_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:618 on 2025-02-14 12:54_

Could you clarify that in the comment? Like, 

```
# The following fails to discover the uv binary via `find_uv_bin`
# TODO: Enable this test on all platforms
```


---

_@konstin reviewed on 2025-02-14 13:26_

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:125 on 2025-02-14 13:26_

Currently on main, we have 6x `--find-links` and 4x `${{ env.PACKAGE_NAME }}-*` in `build-binaries.yml`. I prefer the `--find-links` as I can run it locally on any platform and shell, it doesn't rely on shell expansions.

---

_Comment by @konstin on 2025-02-18 16:34_

This PR should be ready to merge

---

_@zanieb approved on 2025-02-18 17:36_

---

_Merged by @zanieb on 2025-02-18 17:36_

---

_Closed by @zanieb on 2025-02-18 17:36_

---

_Branch deleted on 2025-02-18 17:36_

---
