```yaml
number: 3980
title: "bug: `ERA001` flags non-code segments as commented-out code"
type: issue
state: closed
author: ksquarekumar
labels:
  - bug
assignees: []
created_at: 2023-04-15T20:22:08Z
updated_at: 2023-04-16T23:11:02Z
url: https://github.com/astral-sh/ruff/issues/3980
synced_at: 2026-01-10T11:09:46Z
```

# bug: `ERA001` flags non-code segments as commented-out code

---

_Issue opened by @ksquarekumar on 2023-04-15 20:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```python
#
# # Copyright © 2023 <ORG_NAME> or its affiliates. All Rights Reserved.
# #
# # Licensed under the Apache License, Version 2.0 (the "License"). You
# # may not use this file except in compliance with the License. A copy of
# # the License is located at:
# #
# # https://github.com/<ORG_NAME>/<PROJECT>/blob/main/LICENSE
# #
# # or in the "license" file accompanying this file. This file is
# # distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF
# # ANY KIND, either express or implied. See the License for the specific
# # language governing permissions and limitations under the License.
# #
# # This file is part of the <PROJECT>.
# # see (https://github.com/<ORG_NAME>/<PROJECT>)
# #
# # You should have received a copy of the APACHE LICENSE, VERSION 2.0
# # along with this program. If not, see <https://apache.org/licenses/LICENSE-2.0>
#
```

<img width="591" alt="image" src="https://user-images.githubusercontent.com/7306892/232251459-4ec8de8c-67ba-4136-b00d-74aab9a7f73e.png">

The current Ruff version
- using `ruff` version `0.0.261`

The current Ruff settings:
- using `select = [
    "ALL"
]` in `pyproject.toml`

The command you invoked:
- no command, but using `ruff` in `vs-code`,along with the configured rule in `.pre-commit-config.yaml` listed below.
```yaml
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    # Ruff version.
    rev: 'v0.0.261'
    hooks:
      - id: ruff
```

I could drop this rule, but then having this check is important as well, which is why am filing this bug.

---

_Renamed from "bug: `ERA001` flags non-code segments" to "bug: `ERA001` flags non-code segments as commented-out code" by @ksquarekumar on 2023-04-15 20:22_

---

_Label `bug` added by @charliermarsh on 2023-04-15 20:54_

---

_Closed by @charliermarsh on 2023-04-16 23:11_

---
