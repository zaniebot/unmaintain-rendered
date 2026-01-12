```yaml
number: 8318
title: Let User Know Self Update is Disabled
type: issue
state: closed
author: mahyarmirrashed
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-10-18T04:40:29Z
updated_at: 2024-10-19T11:11:48Z
url: https://github.com/astral-sh/uv/issues/8318
synced_at: 2026-01-12T15:59:23Z
```

# Let User Know Self Update is Disabled

---

_@mahyarmirrashed_

I had installed `uv` from `brew` and was trying to run `uv self update`. However, that won't work (and it shouldn't, so kudos)!
The only thing was that the error message could be improved a bit.

```
mahyar@lioness vivaldi % uv self update
error: unrecognized subcommand 'self'

Usage: uv [OPTIONS] <COMMAND>

For more information, try '--help'.
mahyar@lioness vivaldi % uv --version
uv 0.4.18 (Homebrew 2024-10-01)
```

I would like to see something along the lines of "Hey, it looks like you're trying to self-update. However, `uv` was installed from <source> and cannot be updated in this way. Please use your package manager to update." or something along those lines.

Thanks for a great and fast tool.

---

_Label `cli` added by @zanieb on 2024-10-18 04:50_

---

_Label `help wanted` added by @zanieb on 2024-10-18 04:50_

---

_Closed by @konstin on 2024-10-18 17:38_

---

_Comment by @mahyarmirrashed on 2024-10-19 11:11_

Thanks for the quick fix!

---
