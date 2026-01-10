```yaml
number: 9875
title: Panicking when directories do not exist
type: issue
state: closed
author: bepri
labels:
  - bug
assignees: []
created_at: 2024-12-13T19:10:36Z
updated_at: 2024-12-13T21:39:47Z
url: https://github.com/astral-sh/uv/issues/9875
synced_at: 2026-01-10T04:36:21Z
```

# Panicking when directories do not exist

---

_Issue opened by @bepri on 2024-12-13 19:10_

uv can panic from any command if your current directory no longer exists.

To reproduce:
```sh
mkdir ./tmp && cd ./tmp
# open a second shell and run...
rm -r ./tmp
# back to the first shell
uv version # panic!
```

---

_Label `bug` added by @charliermarsh on 2024-12-13 20:15_

---

_Closed by @charliermarsh on 2024-12-13 21:39_

---

_Closed by @charliermarsh on 2024-12-13 21:39_

---
