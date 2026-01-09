---
number: 11350
title: Improper cache behavior when installing in development mode
type: issue
state: closed
author: youkaichao
labels:
  - question
  - cache
assignees: []
created_at: 2025-02-09T09:41:00Z
updated_at: 2025-02-10T02:37:47Z
url: https://github.com/astral-sh/uv/issues/11350
synced_at: 2026-01-07T13:12:18-06:00
---

# Improper cache behavior when installing in development mode

---

_Issue opened by @youkaichao on 2025-02-09 09:41_

### Summary

I'm using uv via `uv pip install -vvv -e .` , when I change my code and install again, I find that `uv` directly caches the package and refuses to install again.

I can only get it work by `uv cache clean` , but that's too expensive. I don't want to clean all the packages downloaded by `uv` .

### Platform

Linux 5.15.0-1063-nvidia x86_64 GNU/Linux

### Version

uv 0.5.11

### Python version

3.10.15

---

_Label `bug` added by @youkaichao on 2025-02-09 09:41_

---

_Comment by @youkaichao on 2025-02-09 09:42_

More context:

I'm developing vllm. it can be reproduced by:

```bash
$ git clone https://github.com/vllm-project/vllm
$ cd vllm
$ uv pip install -vvv -e .
$ # change some c++ code
$ uv pip install -vvv -e . # it refuses to re-build
```

---

_Comment by @charliermarsh on 2025-02-09 18:06_

This is intentional behavior -- we don't re-build packages on arbitrary code changes. You can use `--reinstall-package vllm` if you want to force a re-build, or you can setup a [cache key](https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata) for the package.

---

_Label `bug` removed by @charliermarsh on 2025-02-09 18:06_

---

_Label `question` added by @charliermarsh on 2025-02-09 18:06_

---

_Comment by @charliermarsh on 2025-02-09 18:07_

(Separately, you can always do `uv cache clean vllm` to only clean a specific package.)

---

_Label `cache` added by @charliermarsh on 2025-02-09 18:07_

---

_Comment by @youkaichao on 2025-02-10 02:36_

thanks for the help, today I learned!

`uv cache clean vllm` and `--reinstall-package vllm` , both are very helpful!

---

_Closed by @youkaichao on 2025-02-10 02:36_

---

_Comment by @charliermarsh on 2025-02-10 02:37_

No worries! Sorry for any confusion.

---
