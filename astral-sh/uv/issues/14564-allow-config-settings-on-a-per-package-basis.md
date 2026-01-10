---
number: 14564
title: "Allow `config-settings` on a per-package basis"
type: issue
state: closed
author: MrNaif2018
labels:
  - enhancement
assignees: []
created_at: 2025-07-11T12:08:24Z
updated_at: 2025-07-18T01:27:55Z
url: https://github.com/astral-sh/uv/issues/14564
synced_at: 2026-01-10T01:25:46Z
---

# Allow `config-settings` on a per-package basis

---

_Issue opened by @MrNaif2018 on 2025-07-11 12:08_

### Summary

Hi! Thanks so much for uv!
So the issue is that, I have one package which does some compilation during the wheel build stage, and I want to disable it. It supports disabling it via env vars, but passing them seems to be not possible to the build stage of only this package via `pyproject.toml`?
The closest I've found is to add to this package:
```
if "electrum_ecc.dont_compile" in sys.argv:
    sys.argv.remove("electrum_ecc.dont_compile")
    ELECTRUM_ECC_DONT_COMPILE = "1"
else:
    ELECTRUM_ECC_DONT_COMPILE = os.getenv("ELECTRUM_ECC_DONT_COMPILE") or ""
```

And then in my `pyproject.toml` in uv:

```
[tool.uv]
config-settings = { "--build-option" = "electrum_ecc.dont_compile" }
```

But the problem is, it does work, but this option is passed to all package builds
So when a non-related package is being built, it errors out because it doesn't know such thing:
`invalid command name 'electrum_ecc.dont_compile'`

If I could provide it only during the wheel build of this specific package, it would help a lot!

### Example

_No response_

---

_Label `enhancement` added by @MrNaif2018 on 2025-07-11 12:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-11 16:16_

---

_Referenced in [astral-sh/uv#14573](../../astral-sh/uv/pulls/14573.md) on 2025-07-11 23:51_

---

_Closed by @charliermarsh on 2025-07-18 01:27_

---
