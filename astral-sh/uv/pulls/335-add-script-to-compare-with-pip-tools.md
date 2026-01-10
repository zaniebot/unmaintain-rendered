```yaml
number: 335
title: Add script to compare with pip(-tools)
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: compare-pydantic
created_at: 2023-11-06T14:59:54Z
updated_at: 2023-11-07T15:32:13Z
url: https://github.com/astral-sh/uv/pull/335
synced_at: 2026-01-10T15:50:28Z
```

# Add script to compare with pip(-tools)

---

_Pull request opened by @konstin on 2023-11-06 14:59_

Add a script to compare with pip-tools and pydantic input we can compare with it. Below is the output for `pydantic.in`, created from pydantic's pyproject.toml, which i added for that purpose:

```console
$ scripts/compare_with_pip.sh scripts/benchmarks/requirements/pydantic.in
      Finished dev [unoptimized + debuginfo] target(s) in 0.08s
       Running `target/debug/puffin pip-compile scripts/benchmarks/requirements/pydantic.in`
  Resolved 85 packages in 1.61s

  real    0m1,733s
  user    0m1,714s
  sys     0m0,048s

  real    0m10,843s
  user    0m4,811s
  sys     0m0,399s
  --- /tmp/tmp.Y3FzvQ2xxo/pip-compile.txt 2023-11-06 15:47:29.221834123 +0100
  +++ /tmp/tmp.Y3FzvQ2xxo/puffin.txt      2023-11-06 15:47:18.377408860 +0100
  @@ -31,7 +31,7 @@
   mdurl==0.1.2
   memray==1.10.0
   mergedeep==1.3.4
  -mike @ git+https://github.com/jimporter/mike.git
  +mike @ git+https://github.com/jimporter/mike.git@076a4af3270a448f6aeb880c9c6c2fc0d80f603f
   mkdocs==1.5.3
   mkdocs-autorefs==0.5.0
   mkdocs-embed-external-markdown==3.0.1
  @@ -52,7 +52,7 @@
   py-cpuinfo==9.0.0
   pydantic==2.4.2
   pydantic-core==2.10.1
  -pydantic-extra-types @ git+https://github.com/pydantic/pydantic-extra-types.git@main
  +pydantic-extra-types @ git+https://github.com/pydantic/pydantic-extra-types.git@a973b7942112df731e2618336e55e3343a2e1c32
   pydantic-settings==2.0.3
   pyflakes==3.1.0
   pygments==2.16.1
  @@ -61,7 +61,7 @@
   pytest==7.4.3
   pytest-benchmark==4.0.0
   pytest-examples==0.0.10
  -pytest-memray==1.5.0 ; platform_system != "Windows"
  +pytest-memray==1.5.0
   pytest-mock==3.12.0
   pytest-pretty==1.2.0
   python-dateutil==2.8.2
```

---

_@charliermarsh reviewed on 2023-11-07 03:57_

---

_Review comment by @charliermarsh on `scripts/compare_with_pip.sh`:1 on 2023-11-07 03:57_

Can you please add example usage to the top?

---

_@charliermarsh reviewed on 2023-11-07 03:59_

---

_Review comment by @charliermarsh on `scripts/compare_with_pip.sh`:2 on 2023-11-07 03:59_

I typically add `set -euxo pipefail`, I'd suggest least `set -euo pipefail`.

---

_@charliermarsh approved on 2023-11-07 15:20_

---

_Merged by @konstin on 2023-11-07 15:32_

---

_Closed by @konstin on 2023-11-07 15:32_

---

_Branch deleted on 2023-11-07 15:32_

---
