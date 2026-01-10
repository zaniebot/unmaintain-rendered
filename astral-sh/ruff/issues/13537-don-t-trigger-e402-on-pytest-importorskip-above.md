```yaml
number: 13537
title: "Don't trigger E402 on `pytest.importorskip` above imports"
type: issue
state: closed
author: janosh
labels:
  - rule
assignees: []
created_at: 2024-09-27T13:51:11Z
updated_at: 2024-11-20T03:08:16Z
url: https://github.com/astral-sh/ruff/issues/13537
synced_at: 2026-01-10T11:09:55Z
```

# Don't trigger E402 on `pytest.importorskip` above imports

---

_Issue opened by @janosh on 2024-09-27 13:51_

Similar to ignoring `sys.path` (https://github.com/astral-sh/ruff/issues/10059) and `os.environ` manipulations above imports, I propose adding another exception for the widely used [`pytest.importorskip`](https://docs.pytest.org/en/stable/reference/reference.html#pytest-importorskip).


picking up on @ion-elgreco's https://github.com/astral-sh/ruff/issues/10059#issuecomment-2068900587, it may make sense to allow `warnings.filterwarnings` interspersed between imports as well.


example code snippet from https://github.com/materialsproject/atomate2/pull/993 where this came up repeatedly (would be nice to get rid off the many `# noqa: E402`):

```py
from atomate2.openff.utils import counts_from_box_size, merge_specs_by_name_and_smiles

pytest.importorskip("openff.toolkit")
import openff.toolkit as tk  # noqa: E402
from openff.interchange import Interchange  # noqa: E402
from openff.toolkit.topology import Topology  # noqa: E402
from openff.toolkit.topology.molecule import Molecule  # noqa: E402
```

---

_Label `rule` added by @charliermarsh on 2024-09-28 12:31_

---

_Comment by @dhruvmanila on 2024-10-03 15:21_

For anyone interested, this requires adding a new function in https://github.com/astral-sh/ruff/blob/fdd0a22c03ddecea7e8b870b6d7a007210ee3d27/crates/ruff_python_semantic/src/analyze/imports.rs similar to `is_os_environ_modification` or `is_sys_path_modification` and updating the condition at https://github.com/astral-sh/ruff/blob/fdd0a22c03ddecea7e8b870b6d7a007210ee3d27/crates/ruff_linter/src/checkers/ast/mod.rs#L496-L502

Once https://github.com/astral-sh/ruff/issues/8794 is implemented, maybe we could only check for this in test files.

This should be added under preview mode.

---

_Closed by @charliermarsh on 2024-11-20 03:08_

---

_Closed by @charliermarsh on 2024-11-20 03:08_

---
