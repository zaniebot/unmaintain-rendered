```yaml
number: 2805
title: "[`flake8-pyi`]: add rules for unrecognized platform check (PYI007, PYI008)"
type: pull_request
state: merged
author: SigureMo
labels:
  - rule
assignees: []
merged: true
base: main
head: pyi007-008
created_at: 2023-02-12T10:18:51Z
updated_at: 2023-02-12T18:02:48Z
url: https://github.com/astral-sh/ruff/pull/2805
synced_at: 2026-01-12T04:52:01Z
```

# [`flake8-pyi`]: add rules for unrecognized platform check (PYI007, PYI008)

---

_Pull request opened by @SigureMo on 2023-02-12 10:18_

Add two [flake8-pyi](https://github.com/PyCQA/flake8-pyi) rules (Y007, Y008). ref: #848

The specifications are described in [PEP 484 - Version and platform checking](https://peps.python.org/pep-0484/#version-and-platform-checking)

The original implementation in flake8-pyi is shown below.

- Implemention: https://github.com/PyCQA/flake8-pyi/blob/66f28a4407af2156f95d370941fae4dd98280740/pyi.py#L1429-L1443
- Tests: https://github.com/PyCQA/flake8-pyi/blob/66f28a4407af2156f95d370941fae4dd98280740/tests/sysplatform.pyi


---

_Comment by @charliermarsh on 2023-02-12 16:56_

This looks like really good and thorough work. Before merging: is there any overlap between these rules and those we enforce via `flake8-2020` (e.g., in `crates/ruff/resources/test/fixtures/flake8_2020`)?

---

_Comment by @SigureMo on 2023-02-12 17:32_

I don't think there's any overlap between them. They have different motivations.

The `flake8-2020` is mainly used to ensure that no errors occur when comparing with `sys.version_info` at runtime.

The `flake8-pyi`(`Y002`-`Y008`) is mainly used to ensure that the comparison statements with `sys.version_info`/`sys.platform` is simple enough, and thus ensure that the type checker can correctly distinguish version/platform specific behaviour.

---

_Label `rule` added by @charliermarsh on 2023-02-12 17:57_

---

_Merged by @charliermarsh on 2023-02-12 18:02_

---

_Closed by @charliermarsh on 2023-02-12 18:02_

---

_Branch deleted on 2023-02-12 18:02_

---
