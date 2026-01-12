```yaml
number: 1044
title: "Support ``.ty.toml`` for configuration"
type: issue
state: open
author: AA-Turner
labels:
  - configuration
  - wish
assignees: []
created_at: 2025-08-18T20:09:09Z
updated_at: 2025-09-19T16:01:30Z
url: https://github.com/astral-sh/ty/issues/1044
synced_at: 2026-01-12T15:54:24Z
```

# Support ``.ty.toml`` for configuration

---

_@AA-Turner_

I initially created my configuration file as `.ty.toml`, to mirror `.ruff.toml` and group files next to each other in the directory. However, this [seems unsupported](https://docs.astral.sh/ty/configuration/).

I've searched for other issues (rather hard with `.ty.toml` as a search pattern!) but couldn't find any requesting support, so I humbly beg thus in this issue.

Thanks,
Adam

---

_Label `configuration` added by @AlexWaygood on 2025-08-18 20:10_

---

_Label `wish` added by @MichaReiser on 2025-08-19 06:15_

---

_Comment by @MichaReiser on 2025-08-19 06:16_

Yes, this was somewhat intentional to reduce the number of places ty has to look and to align with uv.

I'm open to adding support for this but I first want more evidence that this is something many users want.

---

_Comment by @dalcinl on 2025-09-19 16:01_

I'm about to use ty for regularly do type checking in my project. I definitely want this. I do not like overloading my `pyproject.toml` with tool configuration, thus I prefer separate configuration files. Most of my configuration file names start with a dot as per usual UNIX tradition to keep them "hidden" (there is one exception, tough: `tox.ini`) . It would be great if ty, like ruff, adheres to this commonly used UNIX convention.

---
