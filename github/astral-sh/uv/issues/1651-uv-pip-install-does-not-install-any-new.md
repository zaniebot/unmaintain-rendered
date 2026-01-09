---
number: 1651
title: "`uv pip install` does not install any new dependencies"
type: issue
state: closed
author: skshetry
labels:
  - bug
assignees: []
created_at: 2024-02-18T15:00:25Z
updated_at: 2024-03-04T19:40:53Z
url: https://github.com/astral-sh/uv/issues/1651
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv pip install` does not install any new dependencies

---

_Issue opened by @skshetry on 2024-02-18 15:00_

Let's say I have a project with the following dependencies, and installed the package using:

```console
cat pyproject.toml
uv pip install -e "."
```

```toml
# pyproject.toml

[project]
name = "test-project"
version = "0.0.1"
dependencies = ["fsspec"]
```

And, later if I add any new dependencies, doing `uv pip install -e "."` does not pick new dependencies and/or install them, unless I pass `--upgrade`.
```toml
[project]
name = "test-project"
version = "0.0.1"
dependencies = ["fsspec", "s3fs"]
```

```console
$ uv pip install -e "."
Audited 1 package in 3ms
```

```console
$ uv pip install -e "." --upgrade
   Built file:///private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.tP1hZ9RVUH                                                Built 1 editable in 1.00s
Resolved 18 packages in 685ms
Downloaded 5 packages in 142ms
Installed 17 packages in 79ms
 + aiobotocore==2.11.2
 + aiohttp==3.9.3
 + aioitertools==0.11.0
 + aiosignal==1.3.1
 + attrs==23.2.0
 + botocore==1.34.34
 + frozenlist==1.4.1
 + idna==3.6
 + jmespath==1.0.1
 + multidict==6.0.5
 + python-dateutil==2.8.2
 + s3fs==2024.2.0
 + six==1.16.0
 - test-project==0.0.1 (from file:///private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.tP1hZ9RVUH)
 + test-project==0.0.1 (from file:///private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.tP1hZ9RVUH)
 + urllib3==2.0.7
 + wrapt==1.16.0
 + yarl==1.9.4
```


This differs from `pip install` behaviour.


### `uv version`
```console
uv --version
uv 0.1.4
```

---

_Referenced in [wntrblm/nox#762](../../wntrblm/nox/pulls/762.md) on 2024-02-18 15:00_

---

_Label `bug` added by @charliermarsh on 2024-02-18 15:06_

---

_Comment by @charliermarsh on 2024-02-18 15:07_

Agree that this should behave as you describe. I can't remember off-hand if this is due to a caching thing or something else.

---

_Comment by @agriyakhetarpal on 2024-02-18 18:36_

I am currently facing the same issue, but in other ways. It seems as if `uv` is auditing the installation of a package based on the metadata stored in the `<mypackage>.dist-info` directory – which is why if I delete the `<mypackage>` directory from a virtual environment, it successfully audits <mypackage> based on the metadata directory's contents and believes that `<mypackage>` is still installed. Of course, this isn't recommended behaviour (deleting a package's installation from the virtual environment, that is) and this problem is in-line with what `pip` itself does (I should probably open an issue there as well).

A potential solution to mitigate this problem could be to store the checksums of the `pyproject.toml`/`setup.py`/`setup.cfg` files in the `uv` cache and validate them at the time of the audit.

---

_Referenced in [astral-sh/rye#723](../../astral-sh/rye/issues/723.md) on 2024-02-21 07:29_

---

_Referenced in [astral-sh/uv#1948](../../astral-sh/uv/issues/1948.md) on 2024-02-24 14:36_

---

_Comment by @danielhollas on 2024-02-26 16:00_

I think this is fixed in version 0.1.11

---

_Comment by @skshetry on 2024-02-26 16:05_

I'll check when it gets released. Thanks. 🙂 

---

_Comment by @skshetry on 2024-02-26 16:19_

The editable issue seems to be fixed, but it does not work for non-editable installs yet. :(

---

_Comment by @charliermarsh on 2024-02-26 16:30_

@skshetry - Can you provide the exact repro for non-editable installs, just so I can track what isn't working?

---

_Comment by @skshetry on 2024-02-26 16:35_

```console
cat pyproject.toml
uv pip install "test-project @ ."
```

```toml
[project]
name = "test-project"
version = "0.0.1"
dependencies = ["fsspec"]
```

After I add a dependency, say, `s3fs`, and run the above script again, it does not install new dependencies unless I add `--upgrade`.
```
cat pyproject.toml
uv pip install "test-project @ ."
```

```toml
[project]
name = "test-project"
version = "0.0.1"
dependencies = ["fsspec", "s3fs"]
```

```console
$ uv pip install "test-project @ ."
Audited 1 package in 3ms

$ uv pip install "test-project @ ." --upgrade                                                                                    
Resolved 18 packages in 1.80s
   Built test-project @ file:///private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.MLCVX8rwMP                                 Downloaded 1 package in 1.15s
Installed 17 packages in 66ms
 + aiobotocore==2.11.2
 + aiohttp==3.9.3
 + aioitertools==0.11.0
 + aiosignal==1.3.1
 + attrs==23.2.0
 + botocore==1.34.34
 + frozenlist==1.4.1
 + idna==3.6
 + jmespath==1.0.1
 + multidict==6.0.5
 + python-dateutil==2.8.2
 + s3fs==2024.2.0
 + six==1.16.0
 - test-project==0.0.1 (from file:///private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.MLCVX8rwMP)
 + test-project==0.0.1 (from file:///private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.MLCVX8rwMP)
 + urllib3==2.0.7
 + wrapt==1.16.0
 + yarl==1.9.4
```


---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-04 01:57_

---

_Referenced in [astral-sh/uv#2169](../../astral-sh/uv/pulls/2169.md) on 2024-03-04 19:11_

---

_Closed by @charliermarsh on 2024-03-04 19:40_

---
