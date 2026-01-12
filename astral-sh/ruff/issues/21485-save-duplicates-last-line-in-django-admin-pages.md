```yaml
number: 21485
title: Save duplicates last line in django admin pages
type: issue
state: closed
author: swoopa-app
labels:
  - needs-info
assignees: []
created_at: 2025-11-17T00:48:16Z
updated_at: 2025-12-10T17:27:09Z
url: https://github.com/astral-sh/ruff/issues/21485
synced_at: 2026-01-12T15:54:57Z
```

# Save duplicates last line in django admin pages

---

_@swoopa-app_

### Summary


Save duplicates last line in django admin pages which if not noticed causes a build error:
admin.site.register(SearchListingProxy, SearchListingProxyAdmin)

admin.site.register(SearchListingProxy, SearchListingProxyAdmin)


### Version

its on the latest version

---

_Comment by @MichaReiser on 2025-11-17 07:19_

Hi

We'll need a few more information to help you:


* Do you run ruff on the CLI? Can you share the exact command?
* Do you run ruff in your editor? Do you use format on save? What other extensions do you use? Any chance you have the isort extension installed alongside Ruff? We had multiple reports that this causes issues and using both the isort and ruff extension isn't necessary
* If you have, do you have a minimal example that we can use to reproduce the issue


---

_Label `needs-info` added by @MichaReiser on 2025-11-17 07:19_

---

_Closed by @MichaReiser on 2025-12-10 17:27_

---
