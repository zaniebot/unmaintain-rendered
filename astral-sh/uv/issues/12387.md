```yaml
number: 12387
title: "Support `uv sync [--dry-run] --format=json`"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - enhancement
assignees: []
created_at: 2025-03-22T04:21:45Z
updated_at: 2025-07-24T09:13:26Z
url: https://github.com/astral-sh/uv/issues/12387
synced_at: 2026-01-10T03:32:45Z
```

# Support `uv sync [--dry-run] --format=json`

---

_Issue opened by @InSyncWithFoo on 2025-03-22 04:21_

(Opened as per the suggestion [here](https://github.com/astral-sh/uv/pull/11891#issuecomment-2742034566). Part of #411 and #3199.)

Currently, `uv sync --dry-run --script foo.py` would output something like this:

```text
Would create script environment at: [...]/uv/cache/environments-v2/foo-[...]
```

I'm interested in the path itself. Extracting it from the plain text output is rather inconvenient, however. A `--format=json` option could help with this.

An example output might look like:

```json
{
    "environment": "[...]/uv/cache/environments-v2/foo-[...]",
    "executable": "[...]/uv/cache/environments-v2/foo-[...]/bin/python",
    "exist": false
}
```

This format, obviously, needs some design.

---

_Label `enhancement` added by @InSyncWithFoo on 2025-03-22 04:21_

---

_Comment by @x0rw on 2025-03-23 03:51_

The issue is that there is this 'logger', i don't know if i should disable it while formatting or format it's output as well, the later is quite hard to do

---

_Renamed from "Support `uv sync --dry-run --format=json`" to "Support `uv sync [--dry-run] --format=json`" by @InSyncWithFoo on 2025-03-23 04:09_

---

_Comment by @x0rw on 2025-03-23 05:46_

@zanieb assign?

---

_Comment by @zanieb on 2025-03-23 21:29_

I wouldn't worry about formatting the logger. That'll require a larger restructuring to use jsonlines output. Is the logger writing to stdout or stderr? If the latter, nothing needs to change there.

---

_Comment by @iwinux on 2025-07-24 09:09_

IMO everything that might download things from the Internet might need a `--dry-run` flag. Sometimes I type a wrong package name to `uv add` and several hundreds MBs of unknown packages get downloaded and difficult to clean up.

---

_Comment by @InSyncWithFoo on 2025-07-24 09:13_

This seems to have been resolved by #13689.

---

_Closed by @InSyncWithFoo on 2025-07-24 09:13_

---
