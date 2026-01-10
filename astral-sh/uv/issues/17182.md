```yaml
number: 17182
title: "Make usage of `--config-file`/`UV_CONFIG_FILE` visible in verbose output"
type: issue
state: open
author: cmarqu
labels:
  - enhancement
  - good first issue
  - tracing
assignees: []
created_at: 2025-12-18T20:40:13Z
updated_at: 2025-12-19T09:21:40Z
url: https://github.com/astral-sh/uv/issues/17182
synced_at: 2026-01-10T03:11:36Z
```

# Make usage of `--config-file`/`UV_CONFIG_FILE` visible in verbose output

---

_Issue opened by @cmarqu on 2025-12-18 20:40_

### Summary

The very end of the section https://docs.astral.sh/uv/concepts/configuration-files/#configuration-files says
> uv also accepts a `--config-file` command-line argument, which accepts a path to a `uv.toml` to use as the configuration file. When provided, this file will be used in place of *any* discovered configuration files (e.g., user-level configuration will be ignored).

The option can also come in (rather hidden) via `UV_CONFIG_FILE`, which was the case for me.

The effect of this option is very powerful - I would claim more powerful than what users intuitively expect.
Its use should thus be mentioned in verbose output.
Right now, it does not even show up in trace output.



### Example

Such a line could look like the following:
```
DEBUG --config-file/UV_CONFIG_FILE is set to /foo/bar/uv.toml, ignoring all user-level configuration
```


---

_Label `enhancement` added by @cmarqu on 2025-12-18 20:40_

---

_Label `good first issue` added by @zanieb on 2025-12-18 21:14_

---

_Label `tracing` added by @zanieb on 2025-12-18 21:14_

---

_Comment by @zanieb on 2025-12-18 21:15_

I presume it's not logged right now because we're reading the configuration before we've set up logging? I'm not sure off the top of my head though. It's fine to add a log. I'd probably just say

> DEBUG Using explicit configuration file at <path>

I wouldn't say `--config-file / UV_CONFIG_FILE` unless we start tracking them separately and can report the correct one.

---

_Comment by @valkrypton on 2025-12-19 09:21_

Is this available? I'd like to give this a try


---
