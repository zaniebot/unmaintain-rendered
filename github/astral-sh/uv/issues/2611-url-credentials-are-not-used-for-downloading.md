---
number: 2611
title: URL credentials are not used for downloading packages
type: issue
state: closed
author: NiklasRosenstein
labels: []
assignees: []
created_at: 2024-03-22T11:58:39Z
updated_at: 2024-03-22T14:24:53Z
url: https://github.com/astral-sh/uv/issues/2611
synced_at: 2026-01-07T13:12:17-06:00
---

# URL credentials are not used for downloading packages

---

_Issue opened by @NiklasRosenstein on 2024-03-22 11:58_

* __Platform__: amd64-darwin
* __UV version__: 0.1.23

I'm using UV with an `--index-url` that contains basic auth credentials (e.g. for the form `https://username@password:hostname/simple`. It appears that UV is using the URL correctly to read the index, but on accessing individual packages it does not.


```
$ uv pip install --index-url https://[REDACTED]:[REDACTED]@[REDACTED]/artifactory/api/pypi/python-all/simple requests
error: Failed to download: requests==2.31.0
  Caused by: HTTP status client error (403 Forbidden) for url (https://[REDACTED]/artifactory/api/pypi/python-all/packages/packages/70/8e/0e2d847013cb52cd35b38c009bb167a1a26b2ce6cd6965bf26b47bc0bf44/requests-2.31.0-py3-none-any.whl#sha256=58cd2187c01e70e6e26505bca751777aa9f2ee0b7f4300988b709f44e013003f)
```

A similar request works without issues using Pip:

```
$ pip install --index-url https://[REDACTED]:[REDACTED]@[REDACTED]/artifactory/api/pypi/python-all/simple flask
Defaulting to user installation because normal site-packages is not writeable
Looking in indexes: https://[REDACTED]:****@[REDACTED]/artifactory/api/pypi/python-all/simple
Collecting flask
...
```


---

_Comment by @zanieb on 2024-03-22 14:11_

Thanks for the report! Can you reproduce this on `v0.1.22` as well? We just changed some things here.

Could you also share the `-v` verbose logs with your password omitted?

Thanks!
 


---

_Assigned to @zanieb by @zanieb on 2024-03-22 14:11_

---

_Comment by @zanieb on 2024-03-22 14:24_

Ah sorry this is actually fixed by https://github.com/astral-sh/uv/pull/2592 already it's just not released in 0.1.23 like I thought.

We'll have this released soon. You can see the pull request for details. Let me know if you have a chance to build from source to test it!

---

_Closed by @zanieb on 2024-03-22 14:24_

---
