---
number: 2257
title: "Changes to files outside the project root aren't picked up by the server"
type: issue
state: open
author: rivershah
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-27T16:04:37Z
updated_at: 2025-12-31T16:52:47Z
url: https://github.com/astral-sh/ty/issues/2257
synced_at: 2026-01-10T01:51:14Z
---

# Changes to files outside the project root aren't picked up by the server

---

_Issue opened by @rivershah on 2025-12-27 16:04_

I am inside a devcontainer. venvs are not being used.
packages are being installed like this:
`uv pip install --no-progress --compile-bytecode --system -r pyproject.toml`

`ty` configuration in `devcontainer.json` is like this:

```
....
        "ty.configuration": {
          "environment": {
            "python": "/usr/local/bin/python"
          }
        },
....
```

Code navigation does not work till I force server restart:

```
2025-12-27 15:50:21.106301373  INFO Version: 0.0.6
2025-12-27 15:50:21.299023590  INFO Defaulting to python-platform `linux`
2025-12-27 15:50:21.299860040  INFO Python version: Python 3.12, platform: linux
2025-12-27 15:50:22.552719729  INFO Indexed 69 file(s) in 0.006s
2025-12-27 15:50:24.277865454  INFO Initializing the default project
2025-12-27 15:50:24.280221553  INFO Defaulting to python-platform `linux`
2025-12-27 15:50:24.280910303  INFO Python version: Python 3.14, platform: linux
2025-12-27 15:51:04.646402464  INFO Defaulting to python-platform `linux`
2025-12-27 15:51:04.646872401  INFO Python version: Python 3.12, platform: linux
2025-12-27 15:53:32.730038234  INFO Indexed 69 file(s) in 0.007s
2025-12-27 15:53:38.647198755  INFO Indexed 69 file(s) in 0.006s
###### Code navigation is not working at this point
2025-12-27 15:53:45.505586220  INFO Server shut down
[Error - 3:53:45 PM] Server process exited with code 0.
2025-12-27 15:53:45.574405173  INFO Version: 0.0.6
2025-12-27 15:53:45.591334552  INFO Defaulting to python-platform `linux`
2025-12-27 15:53:45.592182967  INFO Python version: Python 3.12, platform: linux
2025-12-27 15:53:46.525817129  INFO Indexed 69 file(s) in 0.006s
2025-12-27 15:54:20.960931926  INFO Initializing the default project
2025-12-27 15:54:20.962718375  INFO Defaulting to python-platform `linux`
2025-12-27 15:54:20.963330738  INFO Python version: Python 3.14, platform: linux
2025-12-27 15:54:21.616495762  INFO Indexed 69 file(s) in 0.006s
2025-12-27 15:55:18.696206614  INFO Defaulting to python-platform `linux`
2025-12-27 15:55:18.696813328  INFO Python version: Python 3.12, platform: linux
2025-12-27 15:56:00.597192961  INFO Indexed 69 file(s) in 0.007s
###### Code navigation is working at this point
```

Can we please look into this for devcontainers.

 

---

_Comment by @MichaReiser on 2025-12-27 16:08_

I think the issue here is that we don't fully support system Python installations

Related to https://github.com/astral-sh/ty/issues/2068

---

_Comment by @rivershah on 2025-12-28 07:20_

Thanks for pointing me to that issue. I played around with this some more. Indexing seems in general flaky, sometimes not discovering third party system wide installed packages at all. Will await https://github.com/astral-sh/ty/issues/2068 resolution

---

_Comment by @MichaReiser on 2025-12-29 12:52_

I see, so this is about changes not being detected in your system Python installation. I'll move the issue to the ty repository.

Is the issue only that changes in your system python environment aren't picked up or do you see the same when changing project files? Could you try setting the `logLevel` to `DEBUG`, ty should then log when it detects file changes, see https://github.com/astral-sh/ty-vscode/blob/main/TROUBLESHOOTING.md

When writing this, I realized that the server currently doesn't subscribe to changes outside the project folder. We should change that.



---

_Label `bug` added by @MichaReiser on 2025-12-29 12:55_

---

_Label `server` added by @MichaReiser on 2025-12-29 12:55_

---

_Renamed from "Server restart needed devcontainers system wide installed packages" to "Changes to files outside the project root aren't picked up by the server" by @MichaReiser on 2025-12-29 12:56_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:46_

---

_Removed from milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:52_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 16:52_

---
