```yaml
number: 5514
title: "Add support for isort's `--case-sensitive` sorting"
type: issue
state: closed
author: matejsp
labels:
  - isort
  - configuration
  - help wanted
assignees: []
created_at: 2023-07-04T20:27:15Z
updated_at: 2023-07-05T20:10:55Z
url: https://github.com/astral-sh/ruff/issues/5514
synced_at: 2026-01-10T11:09:48Z
```

# Add support for isort's `--case-sensitive` sorting

---

_Issue opened by @matejsp on 2023-07-04 20:27_

We currently use flake8 with import order that is set to pycharm. And would like to sort case sensitive.

https://pypi.org/project/flake8-import-order/

Currently there are: 
```
cryptography - see an [example](https://github.com/PyCQA/flake8-import-order/blob/master/tests/test_cases/complete_cryptography.py)

google - style described in [Google Style Guidelines](https://google.github.io/styleguide/pyguide.html?showone=Imports_formatting#Imports_formatting), see an [example](https://github.com/PyCQA/flake8-import-order/blob/master/tests/test_cases/complete_google.py)

smarkets - style as google only with import statements before from X import … statements, see an [example](https://github.com/PyCQA/flake8-import-order/blob/master/tests/test_cases/complete_smarkets.py)

appnexus - style as google only with import statements for packages local to your company or organisation coming after import statements for third-party packages, see an [example](https://github.com/PyCQA/flake8-import-order/blob/master/tests/test_cases/complete_appnexus.py)

edited - see an [example](https://github.com/PyCQA/flake8-import-order/blob/master/tests/test_cases/complete_edited.py)

pycharm - style as smarkets only with case sensitive sorting imported names

pep8 - style that only enforces groups without enforcing the order within the groups
```

In isort there is:
```
Case Sensitive
Tells isort to include casing when sorting module names

Type: Bool
Default: False
Config default: false
Python & Config File Name: case_sensitive
CLI Flags:

--case-sensitive
```

---

_Comment by @charliermarsh on 2023-07-04 22:01_

Is it possible to describe the settings you're looking for in terms of isort configuration? (Is it possible to achieve with isort, or is the ordering not supported by isort at all?)

---

_Comment by @charliermarsh on 2023-07-04 22:02_

(I see that you mentioned `--case-sensitive` below, but I'm guessing that's not sufficient to achieve what you're looking for.)

---

_Label `question` added by @charliermarsh on 2023-07-04 22:02_

---

_Label `isort` added by @charliermarsh on 2023-07-04 22:02_

---

_Comment by @matejsp on 2023-07-05 05:09_

Based on my testing isort feature case-sensitive is enough and it is mostly compatible.
Can you support this flag? And if we need more I will file another issue.

---

_Comment by @charliermarsh on 2023-07-05 14:09_

Yeah, don't mind supporting this flag.

---

_Renamed from "[Feature] Support for isort pycharm style -> case sensitive sort" to "Add support for isort's `--case-sensitive` sorting" by @charliermarsh on 2023-07-05 14:09_

---

_Label `question` removed by @charliermarsh on 2023-07-05 14:10_

---

_Label `configuration` added by @charliermarsh on 2023-07-05 14:10_

---

_Label `help wanted` added by @charliermarsh on 2023-07-05 14:10_

---

_Closed by @charliermarsh on 2023-07-05 20:10_

---
