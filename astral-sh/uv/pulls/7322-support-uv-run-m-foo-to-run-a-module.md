```yaml
number: 7322
title: "Support `uv run -m foo` to run a module"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
draft: true
base: main
head: run-module
created_at: 2024-09-12T04:09:27Z
updated_at: 2024-09-25T06:53:33Z
url: https://github.com/astral-sh/uv/pull/7322
synced_at: 2026-01-10T12:53:45Z
```

# Support `uv run -m foo` to run a module

---

_Pull request opened by @j178 on 2024-09-12 04:09_

## Summary

Resolves #6638



---

_Converted to draft by @j178 on 2024-09-12 05:14_

---

_Comment by @j178 on 2024-09-12 14:21_

I'm having trouble with the command `uv run -m venv -h`; the `-h` option is being interpreted by the global arguments of `uv` instead of being passed down to `venv`.

---

_Comment by @zanieb on 2024-09-12 15:18_

Ah that's tough. I'm not sure if we can support passing arguments to modules in that way?

---

_Comment by @zanieb on 2024-09-12 15:18_

It seems like an argument against this interface since it's a bit ambiguous. I wonder if Clap even supports parsing the rest as a command?

---

_Comment by @eth3lbert on 2024-09-12 16:21_

The simplest solution I could think of would be to make `-m` an `Option<bool>` (or bool with default_value?) flag with num_args=0 and an action that sets it to true. Then, we could simply leverage the command as external subcommand (args in this case) as before. However, I'm unsure how to guarantee that the command is followed by the `-m` flag exactly.

---

_Closed by @j178 on 2024-09-25 06:53_

---
