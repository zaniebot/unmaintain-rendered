```yaml
number: 10599
title: "`unconventional-import-alias` (ICN001) always flags imports without an `as` clause"
type: issue
state: closed
author: bardiharborow
labels:
  - bug
  - rule
assignees: []
created_at: 2024-03-26T06:57:12Z
updated_at: 2024-04-02T03:47:21Z
url: https://github.com/astral-sh/ruff/issues/10599
synced_at: 2026-01-12T15:54:50Z
```

# `unconventional-import-alias` (ICN001) always flags imports without an `as` clause

---

_@bardiharborow_

Currently `unconventional-import-alias` (ICN001) will always flag imports without an `as` clause, even if the conventional alias is the same as the module name:

```toml
[tool.ruff.lint.flake8-import-conventions.aliases]
"django.conf.settings" = "settings"
```

```py
from django.conf import settings  # ICN001 `django.conf.settings` should be imported as `settings`
from django.conf import settings as settings  # No errors
```

Instead it should detect that the module name matches the conventional alias:

```py
from django.conf import settings  # No errors
from django.conf import settings as settings  # No errors
```

(Though I realise that another rule may want to simply `from django.conf import settings as settings` to `from django.conf import settings`.)



---

_Label `rule` added by @AlexWaygood on 2024-03-26 07:08_

---

_Label `bug` added by @MichaReiser on 2024-03-26 07:35_

---

_Comment by @ngnpope on 2024-03-26 10:23_

This is an interesting case... [`ICN001`](https://docs.astral.sh/ruff/rules/unconventional-import-alias/) seems to be targeted at importing as a _different_ name, not enforcing that `import x.y.z` should be styled `from x.y import z`. This is certainly a bug - it shouldn't be flagged - but it feels as though the issue trying to be solved is what I have described at #10608 (and why I came here in the first-place).

Semi-related is that, with the above configuration, `import django.conf.settings` would imply changing to `import django.conf.settings as settings` which would be in violation of [`PLR0402`](https://docs.astral.sh/ruff/rules/manual-from-import/) and that is fixable to `from django.conf import settings` which raises the above issue.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-02 03:02_

---

_Closed by @charliermarsh on 2024-04-02 03:47_

---
