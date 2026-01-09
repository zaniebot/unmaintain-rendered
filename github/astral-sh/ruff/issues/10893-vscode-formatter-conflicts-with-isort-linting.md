---
number: 10893
title: "Vscode formatter conflicts with isort linting quick fix on I001, can't save file and auto-format without breaking linting in some files"
type: issue
state: closed
author: ryanovas
labels: []
assignees: []
created_at: 2024-04-12T01:37:15Z
updated_at: 2024-04-12T01:45:03Z
url: https://github.com/astral-sh/ruff/issues/10893
synced_at: 2026-01-07T13:12:15-06:00
---

# Vscode formatter conflicts with isort linting quick fix on I001, can't save file and auto-format without breaking linting in some files

---

_Issue opened by @ryanovas on 2024-04-12 01:37_

Hi, new user of ruff here.

Having a strange issue where the linter quick fix wants to solve I001 one way, and the formatter wants to format in another way. I have format & quick fix on save both enabled, and they both try to apply and it always flips between the 2, settling on the formatter and the linting error not going away.

I don't want to turn off the isort linting, I would prefer for them not to conflict in the first place. Here are my files:

pyproject.toml:

```
[tool.ruff]
exclude = ["*/migrations/"]
line-length = 100

[tool.ruff.format]
quote-style = "single"
indent-style = "tab"
docstring-code-format = true

[tool.ruff.lint]
select = [
  # pycodestyle
  "E",
  # Pyflakes
  "F",
  # pyupgrade
  "UP",
  # flake8-bugbear
  "B",
  # flake8-simplify
  "SIM",
  # isort
  "I",
]
# https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules
ignore = ["W191", "E111", "E114", "E117", "D206", "D300", "Q000", "Q001", "Q002", "Q003", "COM812", "COM819", "ISC001", "ISC002", "E501"]
```

my file when the quick fix from the linter is applied:

```
from rest_framework.generics import ListAPIView, RetrieveAPIView

from blog.models import BlogPost, BlogPostTag
from blog.serializers import BlogPostSerializer, BlogPostTagSerializer
```

my file when the formatter runs:

```
from blog.models import BlogPost, BlogPostTag
from blog.serializers import BlogPostSerializer, BlogPostTagSerializer
from rest_framework.generics import ListAPIView, RetrieveAPIView
```


It also does this in another file:

with linter:

```
from core.base_tests import LiveApiTestCase

from blog.models import BlogPost
```

with formatter:

```
from blog.models import BlogPost
from core.base_tests import LiveApiTestCase
```

Interestingly, it only seems to do it with the vscode extention. If I run format manually it doesn't change the file from the linting version. It also doesn't do this in every file. Here are some examples that don't do this:

```
from pprint import pformat

import requests
from django.test import LiveServerTestCase, TestCase
```

```
from autoslug import AutoSlugField
from core.base_models import BaseModel
from core.utils.cms import cms_image_preview
from django.conf import settings
from django.db import models
```

```
import uuid

from django.db import models
```

My versions are:
ruff = "^0.3.6"
extension version: v2024.16.0

---

_Comment by @charliermarsh on 2024-04-12 01:39_

Is it possible that you have the isort extension installed?

---

_Comment by @charliermarsh on 2024-04-12 01:40_

Separately, what does your project structure look like? Do you use a `src` directory?

---

_Comment by @ryanovas on 2024-04-12 01:42_

Looks like I did have the isort extension, disabling it fixed the problem. Good call, and such a fast response!

Thank you!

---

_Closed by @ryanovas on 2024-04-12 01:42_

---

_Comment by @charliermarsh on 2024-04-12 01:43_

Oh nice! Okay, glad it was an easy fix. Happy to help.

---

_Comment by @ryanovas on 2024-04-12 01:45_

easy fixes are the best kind of fixes

---
