---
number: 12854
title: show cache usage
type: issue
state: open
author: dotysan
labels:
  - enhancement
assignees: []
created_at: 2025-04-14T00:00:32Z
updated_at: 2025-06-13T10:33:11Z
url: https://github.com/astral-sh/uv/issues/12854
synced_at: 2026-01-10T01:25:26Z
---

# show cache usage

---

_Issue opened by @dotysan on 2025-04-14 00:00_

I'm new here. So apologies if this is a dupe...

But it would be nice if we could query the cache size/utilization. Maybe something akin to `docker system df`.

```sh
$ uv cache df
error: unrecognized subcommand 'df'

Usage: uv cache [OPTIONS] <COMMAND>

For more information, try '--help'.
```

And yes, I know. We can simply do `uv cache dir |xargs du -sh` or more detailed `find $(uv cache dir) -mindepth 1 -maxdepth 1 -type d |xargs du -sh`.


---

_Label `enhancement` added by @dotysan on 2025-04-14 00:00_

---

_Comment by @charliermarsh on 2025-04-14 12:11_

That's reasonable!

---

_Comment by @kalvdans on 2025-06-13 10:33_

The `du` doesn't really help since it is only some opaque objects inside `archive-v0` folder for me. I would expect a future `uv cache df` to order the disk usage by package name.

---
