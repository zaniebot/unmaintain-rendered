```yaml
number: 2220
title: "unknown options for workspace `file:///workspace` keeps popping up arbitrarily in VS Code"
type: issue
state: closed
author: epicwhale
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-25T13:20:39Z
updated_at: 2025-12-29T12:49:37Z
url: https://github.com/astral-sh/ty/issues/2220
synced_at: 2026-01-10T01:56:41Z
```

# unknown options for workspace `file:///workspace` keeps popping up arbitrarily in VS Code

---

_Issue opened by @epicwhale on 2025-12-25 13:20_

### Summary

VS Code + Remote SSH + Dev Containers setup arbitrarily pops the following error in a dialog.. logs captured below from Output tab of VSC

```
2025-12-25 13:15:32.556273167  INFO Version: 0.0.4
2025-12-25 13:15:32.651041218  WARN Received unknown options for workspace `file:///workspace`: {
  "configuration": null,
  "configurationFile": null
}
2025-12-25 13:15:32.652637271  INFO Defaulting to python-platform `linux`
2025-12-25 13:15:32.653326004  INFO Python version: Python 3.13, platform: linux
2025-12-25 13:15:32.683210158  INFO Indexed 44 file(s) in 0.007
```

### Version

ty 0.0.4

---

_Label `server` added by @AlexWaygood on 2025-12-25 13:22_

---

_Comment by @kkpattern on 2025-12-25 13:24_

I think the `configurationFile` option is added in `0.0.6`, you need to upgrade the ty version.

---

_Comment by @MichaReiser on 2025-12-25 14:53_

Thanks for reporting this. This is obviously very annoying. 

We added the new settings in the latest ty version but we obviously want to avoid showing this warning for older ty versions. We should do some client-side filtering to filter out settings that aren't compatible with the ty version (for as long as they have the default value).



---

_Label `bug` added by @MichaReiser on 2025-12-25 14:53_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-25 14:53_

---

_Closed by @MichaReiser on 2025-12-29 12:49_

---
