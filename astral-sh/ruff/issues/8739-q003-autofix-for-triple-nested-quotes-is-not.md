```yaml
number: 8739
title: Q003 autofix for triple nested quotes is not valid python code
type: issue
state: closed
author: dragon-dxw
labels:
  - bug
assignees: []
created_at: 2023-11-17T11:51:37Z
updated_at: 2023-11-24T15:32:48Z
url: https://github.com/astral-sh/ruff/issues/8739
synced_at: 2026-01-12T15:54:48Z
```

# Q003 autofix for triple nested quotes is not valid python code

---

_@dragon-dxw_

Version: 0.1.5
input:
`msg = f"Document \"{context['document'].name}\" does not have a PDF."`
error [no args, invoked by precommit]:
`judgments/views/document_full_text.py:38:19: Q003 [*] Change outer quotes to avoid escaping inner quotes`
autofix via `--fix` [via precommit]
`msg = f'Document "{context['document'].name}" does not have a PDF.'`
which is not valid python

ruff config: https://github.com/nationalarchives/ds-caselaw-editor-ui/blob/chore/test-factory-improvements/pyproject.toml

---

_Renamed from "Q003 autofix not valid python" to "Q003 autofix is not valid python code" by @dragon-dxw on 2023-11-17 11:51_

---

_Renamed from "Q003 autofix is not valid python code" to "Q003 autofix for triple nested quotes is not valid python code" by @dragon-dxw on 2023-11-17 12:01_

---

_Label `bug` added by @charliermarsh on 2023-11-17 16:39_

---

_Comment by @sciyoshi on 2023-11-23 14:06_

@dragon-dxw the fixed version is in fact [valid syntax in Python 3.12](https://docs.python.org/3.12/whatsnew/3.12.html#pep-701-syntactic-formalization-of-f-strings). If you set the target Python version to 3.11 or lower in your configuration using `target-version`, this should no longer be marked as an error. See https://github.com/astral-sh/ruff/pull/7588

---

_Comment by @charliermarsh on 2023-11-24 15:32_

Yeah, in my testing this seems to be correctly detected based on the Python version. Do you mind testing if your Python version is set to Python 3.12? Happy to revisit if we're able to reproduce with a lower target-version.

---

_Closed by @charliermarsh on 2023-11-24 15:32_

---
