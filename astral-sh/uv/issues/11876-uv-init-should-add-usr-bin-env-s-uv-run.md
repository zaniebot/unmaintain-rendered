```yaml
number: 11876
title: "uv init should add #!/usr/bin/env -S uv run"
type: issue
state: open
author: hyperknot
labels:
  - enhancement
assignees: []
created_at: 2025-03-01T03:42:09Z
updated_at: 2025-10-11T20:49:26Z
url: https://github.com/astral-sh/uv/issues/11876
synced_at: 2026-01-12T16:00:49Z
```

# uv init should add #!/usr/bin/env -S uv run

---

_@hyperknot_

### Summary

`uv init` should add the following shebang line to the cli script

```
#!/usr/bin/env -S uv run
```

It should also make it executable.

I mean, just give an option for users to run `./cli.py` as well as `uv run cli.py`. 

Currently, it's _really_ difficult to find what is the correct shebang line to be used with `uv init` projects.


### Example

_No response_

---

_Label `enhancement` added by @hyperknot on 2025-03-01 03:42_

---

_Comment by @magistau on 2025-03-22 23:26_

It's hard to implement properly, because `-S` flag is not a part of [POSIX standard][env]. That is, different `env` implementations may treat it differently, potentially causing issues. IMO a better solution would be introducing a helper executable (say, `uvs`) that could be used instead of `uv run` when a shebang is needed.

[env]: https://pubs.opengroup.org/onlinepubs/9799919799/

---

_Comment by @treyhunner on 2025-10-11 20:49_

I love using scripts in uv but don't like adding the shebang line and setting the executable bit on all my scripts, so this issue is important for my use case.

I opened issue #16241 about adding a `uvs`-like command as @magistau suggested.

As an interim solution, [I made a tool to do this](https://github.com/treyhunner/uvrs).

---
