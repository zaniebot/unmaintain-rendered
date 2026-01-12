```yaml
number: 3515
title: uv cache clean fails after build failures
type: issue
state: closed
author: yeswalrus
labels:
  - bug
  - cache
assignees: []
created_at: 2024-05-10T18:30:04Z
updated_at: 2024-05-11T17:34:46Z
url: https://github.com/astral-sh/uv/issues/3515
synced_at: 2026-01-12T15:58:44Z
```

# uv cache clean fails after build failures

---

_@yeswalrus_

Related to https://github.com/astral-sh/uv/issues/3512

When builds fail, it appears that uv cache can get into a corrupted state where a particular directory exists, and is owned, but is missing +rx permissions and so cannot be deleted.

Repro steps
1) Run
```
#!/bin/bash
make_venv(){
    uv venv $1
    source $1/bin/activate
    uv pip install jaeger-client
}

for i in {1..8}
do
   make_venv $1/$i &
done
```
2) run `uv cache clean`


---

_Label `cache` added by @AlexWaygood on 2024-05-10 18:30_

---

_Comment by @charliermarsh on 2024-05-10 18:33_

Can you share the permission flags on the file?

---

_Comment by @charliermarsh on 2024-05-10 18:33_

We already have handling for read-only files (we try to delete; if it fails and the file is read-only, we change permissions and try again), so wondering if there's something else...

---

_Comment by @yeswalrus on 2024-05-10 22:54_

The file it fails on is actually a directory, so that may have been it?

---

_Comment by @yeswalrus on 2024-05-11 01:02_

```
Clearing cache at: ~/.cache/uv
error: Failed to clear cache at: ~.cache/uv
  Caused by: IO error for operation on ~/.cache/uv/built-wheels-v3/index/6024232a22aa81c8/jaeger-client/4.3.0/b1Sqzocp-Y-aYlYPEp_R4/jaeger-client-4.3.0.tar.gz/build/bdist.linux-x86_64/wheel/jaeger_client: Permission denied (os error 13)
  Caused by: Permission denied (os error 13)
  ```
  ls -la on that file shows:
  ```
  d-w------- 2 user user 4096 May 10 18:00 jaeger_client
  ```

---

_Comment by @charliermarsh on 2024-05-11 01:13_

Thank you!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-11 16:28_

---

_Label `bug` added by @charliermarsh on 2024-05-11 16:28_

---

_Comment by @charliermarsh on 2024-05-11 16:30_

Thanks, reproduced and think I have a fix.

---

_Closed by @charliermarsh on 2024-05-11 17:02_

---

_Comment by @yeswalrus on 2024-05-11 17:34_

Thanks for the prompt resolution!

---
