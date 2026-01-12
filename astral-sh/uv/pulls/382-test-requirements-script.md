```yaml
number: 382
title: Test requirements script
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: test-requirements-script
created_at: 2023-11-09T16:46:37Z
updated_at: 2023-11-19T08:50:51Z
url: https://github.com/astral-sh/uv/pull/382
synced_at: 2026-01-12T16:03:55Z
```

# Test requirements script

---

_@konstin_

This script can compare different requirements between pip(-compile) and puffin across python versions, with debug and release builds.

Examples:
```shell
scripts/compare_with_pip/compare_with_pip.py
scripts/compare_with_pip/compare_with_pip.py -p 3.10
scripts/compare_with_pip/compare_with_pip.py --release -p 3.9 --target 'transformers[deepspeed-testing,dev-tensorflow]'
```

It found a bunch of fixed bugs, e.g. the lack of yanked package handling and source dist handling, as well as #423, which is currently most of the output.

Example output: https://gist.github.com/konstin/9ccf8dc7c2dcca737bf705429ced4892

#443 should be merged first


---

_Converted to draft by @konstin on 2023-11-09 16:46_

---

_Comment by @konstin on 2023-11-13 13:18_

It's already finding deviations: https://gist.github.com/konstin/a90bf01eabcd24d958d50019d224b118

---

_Marked ready for review by @konstin on 2023-11-17 14:10_

---

_Review comment by @BurntSushi on `scripts/compare_with_pip/compare_with_pip.py`:222 on 2023-11-17 16:39_

What about `range(8, sys.version_info.minor)` instead? Might avoid needing to remember to update this when new versions of Python come out.

---

_@BurntSushi approved on 2023-11-17 16:40_

I like it!

---

_@konstin reviewed on 2023-11-17 18:22_

---

_Review comment by @konstin on `scripts/compare_with_pip/compare_with_pip.py`:222 on 2023-11-17 18:22_

I wouldn't expect the dev venv to always be the latest python minor (i switch venvs a lot), and there's also sometimes delay with compatibility for new python versions

---

_@zanieb reviewed on 2023-11-17 18:25_

---

_Review comment by @zanieb on `scripts/compare_with_pip/compare_with_pip.py`:31 on 2023-11-17 18:25_

Naughty to run this on import :p

---

_@zanieb reviewed on 2023-11-17 18:26_

---

_Review comment by @zanieb on `scripts/compare_with_pip/compare_with_pip.py`:55 on 2023-11-17 18:26_

Nit: I'd write a comprehension here 

---

_Merged by @konstin on 2023-11-17 18:26_

---

_Closed by @konstin on 2023-11-17 18:26_

---

_Branch deleted on 2023-11-17 18:26_

---

_@konstin reviewed on 2023-11-19 08:50_

---

_Review comment by @konstin on `scripts/compare_with_pip/compare_with_pip.py`:31 on 2023-11-19 08:50_

I just copy this one between all my helper scripts

---
