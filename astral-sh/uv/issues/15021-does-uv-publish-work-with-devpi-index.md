```yaml
number: 15021
title: "Does `uv publish` work with devpi index?"
type: issue
state: closed
author: yitoli-c
labels:
  - error messages
  - external
assignees: []
created_at: 2025-08-01T22:01:24Z
updated_at: 2025-08-27T13:59:53Z
url: https://github.com/astral-sh/uv/issues/15021
synced_at: 2026-01-10T03:23:54Z
```

# Does `uv publish` work with devpi index?

---

_Issue opened by @yitoli-c on 2025-08-01 22:01_

### Question

Tried to set up a private python package index, and couldn't figure out how to use `uv publish` to do the package upload to the index;

This is the command I tried
```shell
uv publish --publish-url http://localhost:3141/user/index/+simple/
warning: `uv publish` is experimental and may change without warning
Publishing 2 files http://localhost:3141/user/index/+simple/
Enter username ('__token__' if using a token):
Enter password:
Uploading <whl file>
Uploading <tar.gz>
```
but the files do not show up on the index.
I could use `devpi` client to upload the packages, but it will be awesome if I could use `uv` for it!

Wondering if `uv publish` currently works with devpi, or perhaps any other private index solutions?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @yitoli-c on 2025-08-01 22:01_

---

_Comment by @zanieb on 2025-08-01 22:19_

We support the standard Upload API for Python package registries, so yeah we support other private index solutions and we should support devpi — though I don't know the particulars of their implementation.

---

_Comment by @konstin on 2025-08-02 18:44_

`uv publish` is being used with devpi (https://github.com/search?q=%22uv+publish%22+devpi&type=code). How are you checking whether the package has been uploaded?

---

_Comment by @yitoli-c on 2025-08-05 14:41_

I just open up my local devpi webpage and check if the new package is listed over there. Seems like my issue was because of `/+simple/` in the publish url; should have been `uv publish --publish-url http://localhost:3141/user/index/`.

Perhaps nitpicking, if we get a success/failure message from `uv` after the publish, it would be helpful.

`uv` is absolutely awesome! Thanks for all the effort!

---

_Comment by @konstin on 2025-08-05 15:39_

I've filed an issue with devpi to improve the situation when mistaking index and publish URL: https://github.com/devpi/devpi/issues/1097

---

_Label `question` removed by @konstin on 2025-08-05 15:39_

---

_Label `error messages` added by @konstin on 2025-08-05 15:39_

---

_Label `external` added by @konstin on 2025-08-05 15:39_

---

_Comment by @konstin on 2025-08-27 13:59_

This has been fixed upstream.

---

_Closed by @konstin on 2025-08-27 13:59_

---
