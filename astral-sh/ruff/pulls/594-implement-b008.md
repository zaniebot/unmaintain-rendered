```yaml
number: 594
title: Implement B008
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: B008
created_at: 2022-11-05T05:02:48Z
updated_at: 2022-11-05T18:05:04Z
url: https://github.com/astral-sh/ruff/pull/594
synced_at: 2026-01-12T05:48:45Z
```

# Implement B008

---

_Pull request opened by @harupy on 2022-11-05 05:02_

#389

---

_@harupy reviewed on 2022-11-05 05:09_

---

_Review comment by @harupy on `src/snapshots/ruff__linter__tests__B008_B006_B008.py.snap`:1 on 2022-11-05 05:09_

Comparison:

```
> cargo run -- --no-cache --select B008 resources/test/fixtures/B006_B008.py
resources/test/fixtures/B006_B008.py:85:61: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:89:64: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:93:60: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:109:39: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:113:12: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:113:32: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:117:30: B008 Do not perform function calls in argument defaults.
👇 flake8-bugbear doesn't have this
resources/test/fixtures/B006_B008.py:155:34: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:160:30: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:164:45: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:170:21: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:170:31: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:176:22: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:181:19: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:181:37: B008 Do not perform function calls in argument defaults.
```

```
> flake8 --select B008 resources/test/fixtures/B006_B008.py
resources/test/fixtures/B006_B008.py:85:61: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:89:64: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:93:60: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:109:39: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:113:12: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:113:32: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:117:30: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:160:30: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:164:45: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:170:21: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:170:31: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:176:22: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:181:19: B008 Do not perform function calls in argument defaults.
resources/test/fixtures/B006_B008.py:181:37: B008 Do not perform function calls in argument defaults.
```

---

_Review comment by @harupy on `src/snapshots/ruff__linter__tests__B008_B006_B008.py.snap`:1 on 2022-11-05 11:48_

`resources/test/fixtures/B006_B008.py:155` is:

https://github.com/charliermarsh/ruff/blob/fbdc075e5ce76037c80358f842478f865447c3ae/resources/test/fixtures/B006_B008.py#L155

---

_@harupy reviewed on 2022-11-05 11:48_

---

_Merged by @charliermarsh on 2022-11-05 18:05_

---

_Closed by @charliermarsh on 2022-11-05 18:05_

---
