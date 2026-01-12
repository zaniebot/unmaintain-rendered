```yaml
number: 1503
title: Plans to support pip.conf file?
type: issue
state: closed
author: kvelicka
labels:
  - duplicate
  - configuration
assignees: []
created_at: 2024-02-16T15:41:15Z
updated_at: 2024-02-16T15:57:48Z
url: https://github.com/astral-sh/uv/issues/1503
synced_at: 2026-01-12T15:58:29Z
```

# Plans to support pip.conf file?

---

_@kvelicka_

It appears (juding by `strace` output) that `uv` doesn't currently support reading pip's config files (e.g. `/etc/xdg/pip/pip.conf`). The feature I'm after specifically is already described in #1474 (support for the `cert` entry in the config file/via a cmdline flag) but I'm curious about the general stance the `uv` takes with reading pip's config files, and other potential users might be interested too.

Thanks a lot in advance!

---

_Comment by @charliermarsh on 2024-02-16 15:43_

I'd prefer _not_ to read configuration files designed for other tools. Like, we should support a configuration file and configuration format, but (similar to in Ruff), I'd prefer if it's our own format, etc. Otherwise, we have to figure out how to support reading those files across multiple versions of the target tool, and support any changes they make to the format in the future.


---

_Comment by @kvelicka on 2024-02-16 15:47_

Makes a lot of sense, and thank you for the _rapid_ response! Looking forward to trying `uv`!

Happy for this issue to be closed as far as I'm concerned, though it might be worth mentioning this stance in the readme/documentation. PRs welcome?

---

_Label `duplicate` added by @zanieb on 2024-02-16 15:48_

---

_Comment by @zanieb on 2024-02-16 15:52_

Duplicate of #1404 

I'm not sure if it makes sense to document this yet as we're still establishing our formal stance around configuration files. I appreciate the offer though!

---

_Closed by @zanieb on 2024-02-16 15:52_

---

_Label `configuration` added by @zanieb on 2024-02-16 15:57_

---
