```yaml
number: 4726
title: "uv run doesn't respect PEP 723"
type: issue
state: closed
author: mgaitan
labels: []
assignees: []
created_at: 2024-07-02T11:48:21Z
updated_at: 2024-07-02T12:54:15Z
url: https://github.com/astral-sh/uv/issues/4726
synced_at: 2026-01-12T15:58:51Z
```

# uv run doesn't respect PEP 723

---

_@mgaitan_

I know it is still experimental but I wanted to make use of the requirements statement via metadata for a script that was implemented in https://github.com/astral-sh/uv/pull/4656 but it doesn't work. 

`rich` is satisfied by the venv and I expect requests to be installed automatically. However, it works using `--with`

I used the same example given in the PR mentioned. 

```console
tin@morocha:~/lab/uv_tests$ cat example.py 
# /// script
# requires-python = ">=3.11"
# dependencies = [
#   "requests<3",
# 	"rich",
# ]
# ///

import requests
from rich.pretty import pprint

resp = requests.get("https://peps.python.org/api/peps.json")
data = resp.json()
pprint([(k, v["title"]) for k, v in data.items()][:10])

tin@morocha:~/lab/uv_tests$ uv pip tree
rich v13.7.1
├── markdown-it-py v3.0.0
│   └── mdurl v0.1.2
└── pygments v2.18.0

tin@morocha:~/lab/uv_tests$ uv run example.py 
warning: `uv run` is experimental and may change without warning.
Traceback (most recent call last):
  File "/home/tin/lab/uv_tests/example.py", line 9, in <module>
    import requests
ModuleNotFoundError: No module named 'requests'

tin@morocha:~/lab/uv_tests$ uv run --with "requests<3" example.py 
warning: `uv run` is experimental and may change without warning.
Resolved 5 packages in 224ms
Installed 5 packages in 4ms
 + certifi==2024.6.2
 + charset-normalizer==3.3.2
 + idna==3.7
 + requests==2.32.3
 + urllib3==2.2.2
[
│   ('1', 'PEP Purpose and Guidelines'),
│   ('2', 'Procedure for Adding New Modules'),
│   ('3', 'Guidelines for Handling Bug Reports'),
│   ('4', 'Deprecation of Standard Modules'),
│   ('5', 'Guidelines for Language Evolution'),
│   ('6', 'Bug Fix Releases'),
│   ('7', 'Style Guide for C Code'),
│   ('8', 'Style Guide for Python Code'),
│   ('9', 'Sample Plaintext PEP Template'),
│   ('10', 'Voting Guidelines')
]

tin@morocha:~/lab/uv_tests$ uv pip tree
rich v13.7.1
├── markdown-it-py v3.0.0
│   └── mdurl v0.1.2
└── pygments v2.18.0

tin@morocha:~/lab/uv_tests$ uv --version
uv 0.2.18

tin@morocha:~/lab/uv_tests$ python --version
Python 3.12.3
```

---

_Comment by @charliermarsh on 2024-07-02 11:50_

That changed merged yesterday but it hasn’t been released.

---

_Closed by @mgaitan on 2024-07-02 12:54_

---
