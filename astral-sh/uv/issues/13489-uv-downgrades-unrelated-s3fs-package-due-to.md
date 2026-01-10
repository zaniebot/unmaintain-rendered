---
number: 13489
title: uv downgrades unrelated s3fs package due to aiobotocore and boto3 version incompatibility
type: issue
state: closed
author: cas--
labels:
  - question
assignees: []
created_at: 2025-05-16T12:40:10Z
updated_at: 2025-05-16T14:03:17Z
url: https://github.com/astral-sh/uv/issues/13489
synced_at: 2026-01-10T01:25:34Z
---

# uv downgrades unrelated s3fs package due to aiobotocore and boto3 version incompatibility

---

_Issue opened by @cas-- on 2025-05-16 12:40_

### Summary

Encountered strange bug where s3fs is downgraded to an ancient package due to conflict between aiobotocore and latest boto3

First add fsspec with s3 extra and can see s3fs latest version 2025.3.2 is installed:
```
❯ uv add fsspec[s3]
Resolved 20 packages in 11ms
Installed 19 packages in 250ms
 + aiobotocore==2.22.0
 + aiohappyeyeballs==2.6.1
 + aiohttp==3.11.18
 + aioitertools==0.12.0
 + aiosignal==1.3.2
 + attrs==25.3.0
 + botocore==1.37.3
 + frozenlist==1.6.0
 + fsspec==2025.3.2
 + idna==3.10
 + jmespath==1.0.1
 + multidict==6.4.3
 + propcache==0.3.1
 + python-dateutil==2.9.0.post0
 + s3fs==2025.3.2
 + six==1.17.0
 + urllib3==2.4.0
 + wrapt==1.17.2
 + yarl==1.20.0
```

Then add boto3 and it upgrades `botocore`, notably to a  version not compatible with `aiobotocore` which results in `s3fs` being downgraded to a really old version `0.4.2`:
```
❯ uv add boto3
Resolved 10 packages in 8.67s
Uninstalled 2 packages in 50ms
Installed 4 packages in 228ms
 + boto3==1.38.17
 - botocore==1.37.3
 + botocore==1.38.17
 - s3fs==2025.3.2
 + s3fs==0.4.2
 + s3transfer==0.12.0
```

For reference here are the install requirements (parsed by setup.py into `install_requires`) for s3fs:
- https://github.com/fsspec/s3fs/blob/main/requirements.txt
- https://github.com/fsspec/s3fs/blob/0.4.2/requirements.txt

With pip it generates an error but still installs latest boto3 and leaves s3fs
```
❯ pip install fsspec[s3]
Successfully installed aiobotocore-2.22.0 aiohappyeyeballs-2.6.1 aiohttp-3.11.18 aioitertools-0.12.0 aiosignal-1.3.2 attrs-25.3.0 botocore-1.37.3 frozenlist-1.6.0 fsspec-2025.3.2 idna-3.10 jmespath-1.0.1 multidict-6.4.3 propcache-0.3.1 python-dateutil-2.9.0.post0 s3fs-2025.3.2 six-1.17.0 urllib3-2.4.0 wrapt-1.17.2 yarl-1.20.0

❯ pip install boto3
...
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
aiobotocore 2.22.0 requires botocore<1.37.4,>=1.37.2, but you have botocore 1.38.17 which is incompatible.
Successfully installed boto3-1.38.17 botocore-1.38.17 s3transfer-0.12.0

❯ pip list
Package          Version
---------------- -----------
aiobotocore      2.22.0
aiohappyeyeballs 2.6.1
aiohttp          3.11.18
aioitertools     0.12.0
aiosignal        1.3.2
attrs            25.3.0
boto3            1.38.17
botocore         1.38.17
frozenlist       1.6.0
fsspec           2025.3.2
idna             3.10
jmespath         1.0.1
multidict        6.4.3
pip              24.2
propcache        0.3.1
python-dateutil  2.9.0.post0
s3fs             2025.3.2
s3transfer       0.12.0
six              1.17.0
urllib3          2.4.0
wrapt            1.17.2
yarl             1.20.0
```

The workaround for now is to specify boto3 1.37 but it was not obvious that this should be the solution:

```
❯ uv add boto3==1.37
Resolved 10 packages in 484ms
Prepared 3 packages in 1.80s
Uninstalled 3 packages in 143ms
Installed 3 packages in 105ms
 - boto3==1.38.17
 + boto3==1.37.0
 - botocore==1.38.17
 + botocore==1.37.38
 - s3transfer==0.12.0
 + s3transfer==0.11.5

❯ uv sync --upgrade
Resolved 22 packages in 575ms
Prepared 3 packages in 194ms
Uninstalled 3 packages in 40ms
Installed 3 packages in 238ms
 - botocore==1.37.38
 + botocore==1.37.3
 - s3fs==0.4.2
 + s3fs==2025.3.2
 - s3transfer==0.11.5
 + s3transfer==0.11.3
```



### Platform

Ubuntu 22.04

### Version

0.7.4

### Python version

Python 3.12.6

---

_Label `bug` added by @cas-- on 2025-05-16 12:40_

---

_Comment by @konstin on 2025-05-16 13:03_

Both are valid solution from the point of a resolver: uv can't tell which is the better resolution of the two if there is a choice of one of two packages to downgrade. If you require specific minimum version, either you or the project that includes the package originally needs to declare the minimum version. In your case boto3 is a direct dependency, so it is preferred for having a higher version over the transitive dependency s3fs.

---

_Label `bug` removed by @konstin on 2025-05-16 13:03_

---

_Label `question` added by @konstin on 2025-05-16 13:03_

---

_Comment by @cas-- on 2025-05-16 14:03_

Thanks for feedback, I think I can now see what's occurring, I missed that uv in the end removed aiobotocore and that's how it ended up with oldest compatible s3fs.

I was following [universal_pathlib](https://github.com/fsspec/universal_pathlib/) recommendation of using `fsspec[s3]` but seems in this instance specifying `s3fs` prevents this resolving to older version instead of pinning boto3:

```diff
 dependencies = [
+    "boto3>=1.37.3",
-    "fsspec[s3]>=2025.3.2",
+    "s3fs>=2025.3.2",
 ]
```

---

_Closed by @cas-- on 2025-05-16 14:03_

---
