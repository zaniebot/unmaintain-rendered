```yaml
number: 10885
title: Add Windows aarch64 to the release binaries
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/aarch64
created_at: 2025-01-23T04:49:36Z
updated_at: 2025-01-24T15:31:08Z
url: https://github.com/astral-sh/uv/pull/10885
synced_at: 2026-01-10T11:45:16Z
```

# Add Windows aarch64 to the release binaries

---

_Pull request opened by @zanieb on 2025-01-23 04:49_

Following test coverage from #10540 
Closes https://github.com/astral-sh/uv/issues/1141

---

_Label `releases` added by @zanieb on 2025-01-23 04:49_

---

_Marked ready for review by @zanieb on 2025-01-23 16:49_

---

_Review requested from @Gankra by @zanieb on 2025-01-23 16:49_

---

_Review requested from @charliermarsh by @zanieb on 2025-01-23 23:32_

---

_@charliermarsh reviewed on 2025-01-24 00:55_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:168 on 2025-01-24 00:55_

Wait why not?

---

_@zanieb reviewed on 2025-01-24 01:00_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:168 on 2025-01-24 01:00_

It's only used to install Python, I think?

---

_@zanieb reviewed on 2025-01-24 01:00_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:168 on 2025-01-24 01:00_

```
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ env.PYTHON_VERSION }}
          architecture: ${{ matrix.platform.arch }}
 ```

---

_Review comment by @Gankra on `Cargo.toml`:302 on 2025-01-24 14:44_

...right yeah this shouldn't need a cargo-dist init...

---

_@Gankra approved on 2025-01-24 14:44_

---

_@charliermarsh reviewed on 2025-01-24 15:01_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:168 on 2025-01-24 15:01_

But why don't we need the ARM Python? Haha

---

_@zanieb reviewed on 2025-01-24 15:04_

---

_Review comment by @zanieb on `Cargo.toml`:302 on 2025-01-24 15:04_

i did run generate or something

---

_@zanieb reviewed on 2025-01-24 15:07_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:168 on 2025-01-24 15:07_

So... from what I can see we use Python to

- Transform the README (just needs to match the runner)
- Install the wheel (which we skip for aarch64)

In brief, you can run x86 Python on the x86-64 machine but you can't run the ARM64 Python. It'd just crash?

---

_@zanieb reviewed on 2025-01-24 15:23_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:168 on 2025-01-24 15:23_

Sneakily.. further down..

```
      - name: "Test wheel"
        if: ${{ !startsWith(matrix.platform.target, 'aarch64') }}
        shell: bash
```

---

_Merged by @zanieb on 2025-01-24 15:24_

---

_Closed by @zanieb on 2025-01-24 15:24_

---

_Branch deleted on 2025-01-24 15:24_

---

_@charliermarsh reviewed on 2025-01-24 15:31_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:168 on 2025-01-24 15:31_

Ah ok. So it just needs to match the runner.

---
