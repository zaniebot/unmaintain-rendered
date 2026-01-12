```yaml
number: 2078
title: "feat: update scripts to new rules structure"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: scripts-update
created_at: 2023-01-22T00:03:58Z
updated_at: 2023-01-22T00:20:15Z
url: https://github.com/astral-sh/ruff/pull/2078
synced_at: 2026-01-12T15:55:07Z
```

# feat: update scripts to new rules structure

---

_@sbrugman_

- optional `prefix` argument for `add_plugin.py`
- rules directory instead of `rules.rs`
- pathlib syntax
- fix test case where code was added instead of name

Example:
```
python scripts/add_plugin.py --url https://pypi.org/project/example/1.0.0/ example --prefix EXA
python scripts/add_rule.py --name SecondRule --code EXA002 --linter example
python scripts/add_rule.py --name FirstRule --code EXA001 --linter example
python scripts/add_rule.py --name ThirdRule --code EXA003 --linter example
 ```

Note that it breaks compatibility with 'old style' plugins (generation works fine, but namespaces need to be changed):
```
python scripts/add_rule.py --name DoTheThing --code PLC999 --linter pylint
```

---

_Converted to draft by @sbrugman on 2023-01-22 00:13_

---

_Marked ready for review by @sbrugman on 2023-01-22 00:15_

---

_Merged by @charliermarsh on 2023-01-22 00:19_

---

_Closed by @charliermarsh on 2023-01-22 00:19_

---

_Branch deleted on 2023-01-22 00:20_

---
