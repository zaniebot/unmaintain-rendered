```yaml
number: 15918
title: "Weird behaviour when running concurrent `uv run --script` with different python versions"
type: issue
state: open
author: mohammedgqudah
labels:
  - bug
assignees: []
created_at: 2025-09-17T19:37:38Z
updated_at: 2025-09-17T21:16:12Z
url: https://github.com/astral-sh/uv/issues/15918
synced_at: 2026-01-12T16:02:19Z
```

# Weird behaviour when running concurrent `uv run --script` with different python versions

---

_@mohammedgqudah_

### Summary

When I run two `uv run --python <VERSION> --script file.py` commands at the same time with different python versions, one of the runs fails to install the required packages.

## Steps to reproduce
1. Use the example in https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies
2. Run two commands at the same time
```
uv run --python 3.13 --script test.py & uv run --python 3.12 --script test.py &
``` 

the behaviour is inconsistent, but many of the times i get this output

<img width="1428" height="1147" alt="Image" src="https://github.com/user-attachments/assets/417e2a4f-03e1-4055-ac34-b93ffdab7058" />

### Platform

Linux 6.16.7-arch1-1 x86_64 GNU/Linux

### Version

uv 0.8.17

### Python version

Python 3.12.11

---

_Label `bug` added by @mohammedgqudah on 2025-09-17 19:37_

---

_Comment by @zanieb on 2025-09-17 19:47_

Can you share verbose logs please?

I'd expect us to perform some locking here per https://github.com/astral-sh/uv/pull/14153 but it's still plausible that these race since we release the lock once we're done updating the environment.

---

_Comment by @mohammedgqudah on 2025-09-17 20:05_

> Can you share verbose logs please?

sure
```
λ uv run --python 3.13 --script test.py --verbose & uv run --verbose --python 3.12 --script test.py &
[1] 249650
[2] 249651
hyper@archbox ~/Desktop/projects/pyexpl
λ DEBUG uv 0.8.17
DEBUG Reading inline script metadata from `test.py`
DEBUG Acquired lock for `/home/hyper/Desktop/projects/pyexpl/test.py`
DEBUG Using Python request `3.12` from explicit request
DEBUG Using Python request Python 3.12 from explicit request
DEBUG Checking for Python environment at: `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07`
DEBUG The script environment's Python version satisfies the request: `Python 3.12`
DEBUG Released lock at `/tmp/uv-601831d429944c07.lock`
DEBUG Acquired lock for `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07`
DEBUG All requirements satisfied: certifi>=2017.4.17 | charset-normalizer>=2, <4 | idna>=2.5, <4 | markdown-it-py>=2.2.0 | mdurl~=0.1 | pygments>=2.13.0, <3.0.0 | requests<3 | rich | urllib3>=1.21.1, <3
DEBUG Released lock at `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07/.lock`
DEBUG Using Python 3.12.11 interpreter at: /home/hyper/.cache/uv/environments-v2/test-601831d429944c07/bin/python3
DEBUG Running `python test.py`
DEBUG Spawned child 249658 in process group 249651
Traceback (most recent call last):
  File "/home/hyper/Desktop/projects/pyexpl/test.py", line 8, in <module>
    import requests
ModuleNotFoundError: No module named 'requests'
DEBUG Command exited with code: 1

[2]  + 249651 exit 1     uv run --verbose --python 3.12 --script test.py
hyper@archbox ~/Desktop/projects/pyexpl
λ ⠋ Resolving dependencies...
Installed 9 packages in 9ms
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

[1]  + 249650 done       uv run --python 3.13 --script test.py --verbose
hyper@archbox ~/Desktop/projects/pyexpl
λ

```

and here is another run
```
λ uv run --python 3.13 --script test.py --verbose & uv run --verbose --python 3.12 --script test.py &
[1] 249833
[2] 249834
hyper@archbox ~/Desktop/projects/pyexpl
λ DEBUG uv 0.8.17
DEBUG Reading inline script metadata from `test.py`
DEBUG Acquired lock for `/home/hyper/Desktop/projects/pyexpl/test.py`
DEBUG Using Python request `3.12` from explicit request
DEBUG Using Python request Python 3.12 from explicit request
DEBUG Checking for Python environment at: `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07`
DEBUG The script environment's Python version does not satisfy the request: `Python 3.12`
DEBUG Searching for Python 3.12 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.11-linux-x86_64-gnu` at `/home/hyper/Desktop/projects/pyexpl/.venv/bin/python3` (virtual environment)
DEBUG Removed virtual environment at: \x1b[36m/home/hyper/.cache/uv/environments-v2/test-601831d429944c07\x1b[39m
DEBUG Creating script environment at: \x1b[36m/home/hyper/.cache/uv/environments-v2/test-601831d429944c07\x1b[39m
DEBUG Assessing Python executable as base candidate: /home/hyper/.local/share/uv/python/cpython-3.12.11-linux-x86_64-gnu/bin/python3.12
DEBUG Using base executable for virtual environment: /home/hyper/.local/share/uv/python/cpython-3.12.11-linux-x86_64-gnu/bin/python3.12
DEBUG Released lock at `/tmp/uv-601831d429944c07.lock`
DEBUG Acquired lock for `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07`
DEBUG At least one requirement is not satisfied: rich
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.11
DEBUG Solving with target Python version: >=3.12.11
DEBUG Adding direct dependency: requests<3
DEBUG Adding direct dependency: rich*
DEBUG Found fresh response for: https://pypi.org/simple/requests/
DEBUG Searching for a compatible version of requests (<3)
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/rich/
DEBUG Adding transitive dependency for requests==2.32.5: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.5: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.5: urllib3>=1.21.1, <3
DEBUG Adding transitive dependency for requests==2.32.5: certifi>=2017.4.17
DEBUG Searching for a compatible version of rich (*)
DEBUG Selecting: rich==14.1.0 [compatible] (rich-14.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e3/30/3c4d035596d3cf444529e0b2953ad0466f6049528a879d27534700580395/rich-14.1.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for rich==14.1.0: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==14.1.0: pygments>=2.13.0, <3.0.0
DEBUG Found fresh response for: https://pypi.org/simple/charset-normalizer/
DEBUG Found fresh response for: https://pypi.org/simple/urllib3/
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Found fresh response for: https://pypi.org/simple/pygments/
DEBUG Selecting: charset-normalizer==3.4.3 [compatible] (charset_normalizer-3.4.3-cp312-cp312-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/idna/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/16/ab/0233c3231af734f5dfcf0844aa9582d5a1466c985bbed6cedab85af9bfe3/charset_normalizer-3.4.3-cp312-cp312-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.10 [compatible] (idna-3.10-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.5.0 [compatible] (urllib3-2.5.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e5/48/1549795ba7742c948d2ad169c1c8cdbae65bc450d6cd753d124b17c8cd32/certifi-2025.8.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/markdown-it-py/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/94/54/e7d793b573f298e1c9013b8c4dade17d481164aa517d1d7148619c2cedbf/markdown_it_py-4.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a7/c2/fe1e52489ae3122415c51f387e221dd0773709bad6c6cdaa599e8a2c5185/urllib3-2.5.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2025.8.3 [compatible] (certifi-2025.8.3-py3-none-any.whl)
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==4.0.0 [compatible] (markdown_it_py-4.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for markdown-it-py==4.0.0: mdurl>=0.1, <1.dev0
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.19.2 [compatible] (pygments-2.19.2-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/mdurl/
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [compatible] (mdurl-0.1.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
DEBUG Tried 9 versions: certifi 1, charset-normalizer 1, idna 1, markdown-it-py 1, mdurl 1, pygments 1, requests 1, rich 1, urllib3 1
DEBUG marker environment resolution took 0.002s
Resolved 9 packages in 2ms
DEBUG Registry requirement already cached: urllib3==2.5.0
DEBUG Registry requirement already cached: charset-normalizer==3.4.3
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: mdurl==0.1.2
DEBUG Registry requirement already cached: markdown-it-py==4.0.0
DEBUG Registry requirement already cached: pygments==2.19.2
DEBUG Registry requirement already cached: rich==14.1.0
DEBUG Registry requirement already cached: requests==2.32.5
DEBUG Registry requirement already cached: certifi==2025.8.3
Traceback (most recent call last):
Installed 9 packages in 7ms
 + certifi==2025.8.3
 + charset-normalizer==3.4.3
 + idna==3.10
 + markdown-it-py==4.0.0
 + mdurl==0.1.2
 + pygments==2.19.2
 + requests==2.32.5
 + rich==14.1.0
 + urllib3==2.5.0
DEBUG Released lock at `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07/.lock`
DEBUG Using Python 3.12.11 interpreter at: /home/hyper/.cache/uv/environments-v2/test-601831d429944c07/bin/python
DEBUG Running `python test.py`
DEBUG Spawned child 249862 in process group 249834
  File "/home/hyper/Desktop/projects/pyexpl/test.py", line 8, in <module>
    import requests
ModuleNotFoundError: No module named 'requests'

[1]  - 249833 exit 1     uv run --python 3.13 --script test.py --verbose
hyper@archbox ~/Desktop/projects/pyexpl
λ [
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
DEBUG Command exited with code: 0

[2]  + 249834 done       uv run --verbose --python 3.12 --script test.py
```
btw, one of the runs printed this error but I wasn't capturing verbose output then
```
error: Failed to read metadata for: certifi==2025.8.3
  Caused by: failed to open file `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07/lib/python3.13/site-packages/certifi-2025.8.3.dist-info/METADATA`: No such file or directory (os error 2)
```

---

_Comment by @zanieb on 2025-09-17 20:13_

We take a lock while mutating the environment

```
DEBUG Acquired lock for `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07`
DEBUG All requirements satisfied: certifi>=2017.4.17 | charset-normalizer>=2, <4 | idna>=2.5, <4 | markdown-it-py>=2.2.0 | mdurl~=0.1 | pygments>=2.13.0, <3.0.0 | requests<3 | rich | urllib3>=1.21.1, <3
DEBUG Released lock at `/home/hyper/.cache/uv/environments-v2/test-601831d429944c07/.lock`
```

then release it before spawning the command. Otherwise, you wouldn't be able to use `uv run` concurrently at all. Unfortunately I'm not sure we can do better here. We could hold the lock for longer to reduce the window for a race, but it's still possible.

I think what you need is for us to key the environment by the Python version (which makes editor support worse) or for #15919 to work so you can opt-in to separate environments for these.

---

_Comment by @mohammedgqudah on 2025-09-17 20:28_

> I think what you need is for us to key the environment by the Python version (which makes editor support worse) or for [#15919](https://github.com/astral-sh/uv/issues/15919) to work so you can opt-in to separate environments for these.

I can work on https://github.com/astral-sh/uv/issues/15919

I haven't contributed to UV before so I might take some time to get familiar and set up the project.

---

_Comment by @zanieb on 2025-09-17 21:16_

Sweet, let me know if you need help.

---
