```yaml
number: 13389
title: "Support [lint.isort] group_by_package config"
type: issue
state: open
author: an0o0nym
labels:
  - isort
  - wish
  - needs-decision
assignees: []
created_at: 2024-09-18T07:37:19Z
updated_at: 2025-09-24T17:32:31Z
url: https://github.com/astral-sh/ruff/issues/13389
synced_at: 2026-01-10T11:09:55Z
```

# Support [lint.isort] group_by_package config

---

_Issue opened by @an0o0nym on 2024-09-18 07:37_

Hi,
isort linting works very nice, but I am missing a single option to finish my ruff setup - which is 'group_by_package'.

1. Would it be easy to implement?
2. What is my workaround for temporary solution until its implemented?


---

_Label `isort` added by @MichaReiser on 2024-09-18 15:21_

---

_Label `wish` added by @MichaReiser on 2024-09-18 15:21_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-18 15:21_

---

_Comment by @MichaReiser on 2024-09-18 15:22_

Hi @an0o0nym 

Thanks for the feature request. We've been hesitant to add support for more isort options. It's a non goal for us to support all isort options. But I'll keep this open to track the feature request.

---

_Comment by @an0o0nym on 2024-09-19 20:14_

@MichaReiser  thanks for quick reply. Is there any way I can use isort alongside ruff until its implemented [or not].?

---

_Comment by @charliermarsh on 2024-09-20 02:24_

You can always use isort alongside Ruff -- just be sure not to enable Ruff's `I` rules.

What does this setting do exactly? It's not obvious from the docs.

---

_Comment by @an0o0nym on 2024-09-20 08:42_

OK, thanks.


Ideally it should do something like :


```python
from django.http import Http404

from rest_framework.generics import CreateAPIView
from rest_framework.generics import ListAPIView
```

so grouping the top-level packages into its own sections.

---

_Comment by @dhruvmanila on 2024-10-04 05:09_

If I'm understanding it correctly, I think this could be achieved by [`sections`](https://docs.astral.sh/ruff/settings/#lint_isort_sections) config option to introduce the new sections and then updating the [`section-order`](https://docs.astral.sh/ruff/settings/#lint_isort_section-order) although it does require additional configuration for each group of imports. For example:

```toml
["tool.ruff"]
select = [ "I" ]

["tool.ruff.isort"]
section-order = [ "future", "standard-library", "third-party", "first-party", "local-folder", "django", "rest_framework" ]
force-single-line = true

["tool.ruff.isort".sections]
django = [ "django" ]
rest_framework = [ "rest_framework" ]
```

The [`force-single-line`](https://docs.astral.sh/ruff/settings/#lint_isort_force-single-line) can be used to keep the imports separated on their own line similar to your example.

Playground: https://play.ruff.rs/2eb35ec5-05b4-4432-8ac9-999b8d79882a



---

_Comment by @an0o0nym on 2024-10-22 08:44_

@dhruvmanila sure this would work, but as you mentioned it requires naming each package manually in the config. 

---

_Comment by @tonycoco on 2025-09-24 17:32_

Ruff's isort config should probably get parity so folks can use the isort profiles like Google's styleguide:

```
[tool.ruff.lint.isort]
force-single-line = true
force-sort-within-sections = true
lexicographical = true
line-length = 1000
single-line-exclusions = [
    "collections.abc",
    "six.moves",
    "typing",
    "typing-extensions",
]
order-by-type = false
group-by-package = true
```

This impedes a lot of team's ability to share a single config across projects and for us to adhere to Google's styleguide.

---
