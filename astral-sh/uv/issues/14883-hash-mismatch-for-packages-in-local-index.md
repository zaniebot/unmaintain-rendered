```yaml
number: 14883
title: Hash mismatch for packages in local index
type: issue
state: closed
author: iwinux
labels:
  - bug
assignees: []
created_at: 2025-07-25T04:27:15Z
updated_at: 2025-07-31T14:20:50Z
url: https://github.com/astral-sh/uv/issues/14883
synced_at: 2026-01-12T16:01:58Z
```

# Hash mismatch for packages in local index

---

_@iwinux_

## Problem

Installing a package (`foo`) that is hosted in a local index (PEP-503 compliant) fails with error:

```bash
  × Failed to download `foo==0.1.0`
  ╰─▶ Hash mismatch for `foo==0.1.0`

      Expected:
        sha256:e6a87f2bc51bda97f0fd26303adb3053b114daa92620d924263f08c3587eb005

      Computed:
        sha256:b2a09ff57ec883aaffd9a9ec985648724b52fef68d995cdc0e96dbedfa426d1b
  help: `foo` (v0.1.0) was included because `bar` (v0.1.0) depends on `foo`
```

But `sha256sum` shows that the listed hashes are indeed correct:

```bash
$ sha256sum /tmp/pypi/foo/foo-*
b2a09ff57ec883aaffd9a9ec985648724b52fef68d995cdc0e96dbedfa426d1b  /tmp/pypi/foo/foo-0.1.0-py3-none-any.whl
e6a87f2bc51bda97f0fd26303adb3053b114daa92620d924263f08c3587eb005  /tmp/pypi/foo/foo-0.1.0.tar.gz
```

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Links for foo</title>
  </head>
  <body>
    <h1>Links for foo</h1>
    <a href="/tmp/pypi/foo/foo-0.1.0-py3-none-any.whl#sha256=b2a09ff57ec883aaffd9a9ec985648724b52fef68d995cdc0e96dbedfa426d1b">foo-0.1.0-py3-none-any.whl</a><br>
    <a href="/tmp/pypi/foo/foo-0.1.0.tar.gz#sha256=e6a87f2bc51bda97f0fd26303adb3053b114daa92620d924263f08c3587eb005">foo-0.1.0.tar.gz</a><br>
  </body>
</html>
```

## Reproduce

1. Clone repo: https://github.com/iwinux/uv-issue-reproduce-14883
2. Run `make foo-publish`: "publish" the package to local index `/tmp/pypi`.
3. Run `make bar-sync`: install `foo` from `/tmp/pypi` - will fail with hash mismatch error

However, the following steps somehow fix it:

1. Revert changes to `bar/uv.lock`: `git checkout bar/uv.lock`
2. Clean cache: `uv cache clean foo`
3. Remove `*.tar.gz` from `index.html`: `sed -i '/tar/d' /tmp/pypi/foo/index.html`
4. Run `make bar-sync`: no error this time

## Env

### Platform

Linux 6.6.72 x86_64 GNU/Linux

### Version

uv 0.8.3

### Python version

Python 3.12.11

---

_Label `bug` added by @iwinux on 2025-07-25 04:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-25 14:21_

---

_Comment by @charliermarsh on 2025-07-25 14:39_

Thanks, I'll take a look.

---

_Comment by @charliermarsh on 2025-07-31 14:09_

Fixed in the next release, thanks for the clear reproduction, it made it significantly easier to fix.

---

_Closed by @charliermarsh on 2025-07-31 14:20_

---
