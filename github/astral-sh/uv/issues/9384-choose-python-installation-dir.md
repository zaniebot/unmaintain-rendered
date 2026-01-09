---
number: 9384
title: choose python installation dir
type: issue
state: closed
author: davidszotten
labels:
  - question
assignees: []
created_at: 2024-11-23T15:14:51Z
updated_at: 2024-11-23T18:58:22Z
url: https://github.com/astral-sh/uv/issues/9384
synced_at: 2026-01-07T13:12:18-06:00
---

# choose python installation dir

---

_Issue opened by @davidszotten on 2024-11-23 15:14_

Hi. Apologies if this is documented somewhere and i just failed to find it:

is it possible to choose the directory into which pythons are installed? (currently for me on os x: `~/Library/Application Support/uv/python/`

the issue is that some tools (today mason-nvim) choke on the space in the path (with very cryptic errors). i know that's a bug elsewhere but maybe an easy workaround for me would be to pick a different installation dir

---

_Comment by @charliermarsh on 2024-11-23 15:23_

Yeah: https://docs.astral.sh/uv/configuration/environment/#uv_python_install_dir

---

_Comment by @charliermarsh on 2024-11-23 15:25_

Also, if you remove that directory entirely (like `rm ~/Library/Application Support/uv`), we should stop using it. It's only used for backwards compatibility.

---

_Label `question` added by @charliermarsh on 2024-11-23 15:26_

---

_Comment by @davidszotten on 2024-11-23 18:58_

Thanks!

---

_Closed by @davidszotten on 2024-11-23 18:58_

---
