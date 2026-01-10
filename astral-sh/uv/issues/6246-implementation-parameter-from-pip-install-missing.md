```yaml
number: 6246
title: "`implementation` parameter from `pip install` missing in `uv pip install / compile`"
type: issue
state: open
author: schlamar
labels:
  - compatibility
  - configuration
assignees: []
created_at: 2024-08-20T06:58:10Z
updated_at: 2024-08-24T07:49:50Z
url: https://github.com/astral-sh/uv/issues/6246
synced_at: 2026-01-10T04:45:09Z
```

# `implementation` parameter from `pip install` missing in `uv pip install / compile`

---

_Issue opened by @schlamar on 2024-08-20 06:58_

Follow up of #6198

With `pip` you can use `--implementation py` to only select packages which don't have platform dependent binary content. I couldn't find a way to reproduce this behavior with `uv`. See https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/ 

Would be great if this could be supported in `uv` for the relevant operations as well. I guess that would be `uv pip install` and maybe `uv pip compile`.

---

_Label `compatibility` added by @zanieb on 2024-08-20 12:31_

---

_Label `configuration` added by @zanieb on 2024-08-20 12:31_

---

_Comment by @schlamar on 2024-08-24 07:49_

I did a little a bit of experimentation (see #6553)

For my actual use case, to select only pure Python wheels (implementation, platform and abi agnostic), I just learned that you have to pass `--implementation py --abi none --platform any` to pip to get the expected behavior. Apperently it is perfectly fine to provide a wheel with a `py310-none-win_amd64` tag - for example if it is a pure Python library with embedded .exe or .dll file.

Maybe uv could provide a shortcut flag for getting this kind of behavior, `--pure-wheels` or something like that. I think I would be able to provide a PR for this part.

Additionally, there is the general use case to define implementation, abi and platform to get specific wheels. In pip you can define multiple abis **and** multiple platforms. I do not really see a real world use case for the latter part, but specifying multiple abis could make sense. For example if you want to install a Linux compatible wheel you could allow different manylinux abis.

I guess for the second part it needs more time and input from other to define a precise specification. It could require a lot of internal refactoring to select multiple abis from a command line argument.



---
