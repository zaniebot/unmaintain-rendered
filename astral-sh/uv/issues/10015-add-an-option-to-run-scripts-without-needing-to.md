```yaml
number: 10015
title: "Add an option to run scripts without needing to use 'uv run'"
type: issue
state: closed
author: sandenbergmelo
labels:
  - question
  - wontfix
assignees: []
created_at: 2024-12-19T00:31:01Z
updated_at: 2025-02-28T13:29:24Z
url: https://github.com/astral-sh/uv/issues/10015
synced_at: 2026-01-12T16:00:05Z
```

# Add an option to run scripts without needing to use 'uv run'

---

_@sandenbergmelo_

It would be more practical if uv had an option to run Python files without needing to use the `run` command explicitly.

Like this:
`uv example.py`
As an alternative to:
`uv run example.py`


---

_Comment by @zanieb on 2024-12-19 00:35_

Sorry, but I don't think we will be doing this. We might add a Python shim that let's you do `python example.py` (#6265) but `uv <file>` seems too ambiguous given the amount of other things uv can do.

---

_Label `question` added by @samypr100 on 2024-12-19 00:55_

---

_Label `wontfix` added by @samypr100 on 2024-12-19 00:55_

---

_Comment by @rgwood-dd on 2024-12-19 18:06_

Another option: if you're on a *nix system you can use a shebang.

Make this the first line of `example.py`:

```
#!/usr/bin/env -S uv run
```

Make sure `example.py` is executable:

```
chmod +x example.py
```

And then run it directly:

```
./example.py
```

---

_Closed by @zanieb on 2024-12-19 18:13_

---

_Comment by @EliteTK on 2025-02-28 13:29_

@rgwood-dd the `-S` option to env is an extension which is not available on all "*nix" systems.

I think that it would at least be helpful to provide a uv-run symlink to uv which would be equivalent to doing `uv run` so that shebangs can utilise `/usr/bin/env` (which is also not a standardised location for env, but at least it's more widely supported than `-S`) without needing to rely on `-S`.

---
