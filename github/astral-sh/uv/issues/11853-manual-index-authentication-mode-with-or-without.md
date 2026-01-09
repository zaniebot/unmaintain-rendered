---
number: 11853
title: Manual index authentication mode with (or without) keyring
type: issue
state: open
author: marcellszekrenyes
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-02-28T13:13:10Z
updated_at: 2025-02-28T14:15:33Z
url: https://github.com/astral-sh/uv/issues/11853
synced_at: 2026-01-07T13:12:18-06:00
---

# Manual index authentication mode with (or without) keyring

---

_Issue opened by @marcellszekrenyes on 2025-02-28 13:13_

### Summary
At the moment I see a two ways of HTTPS authentication for private package registries. Either by storing the username/password/token somewhere, or by directly including it in the URL. My idea/request is to have a mode where the user gets prompted for username/password instead. I have looked around a bit, I can see a few possible options to achieve this and I would be willing to work on such a feature if there is no specific reason not to have such a functionality.

Some possible options that seem to be feasible:
1. Use the --keyring-provider subprocess flag and have a special value for username or username:password combination, which leads to prompts for the credentials instead of using the keyring. (This seems to be the easiest way, but might not be the cleanest option)
2. Create a --keyring-provider manual-input option that then achieves the same.
3. Extending this issue: https://github.com/astral-sh/uv/issues/11600 with a manual-input mode which then leads to the user being prompted for credentials.

### Example

_No response_

---

_Label `enhancement` added by @marcellszekrenyes on 2025-02-28 13:13_

---

_Label `needs-decision` added by @konstin on 2025-02-28 13:22_

---
