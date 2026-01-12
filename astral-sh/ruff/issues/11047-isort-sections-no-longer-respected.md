```yaml
number: 11047
title: Isort sections no longer respected?
type: issue
state: closed
author: Badg
labels:
  - question
  - isort
assignees: []
created_at: 2024-04-19T20:23:27Z
updated_at: 2024-04-19T22:26:56Z
url: https://github.com/astral-sh/ruff/issues/11047
synced_at: 2026-01-12T15:54:50Z
```

# Isort sections no longer respected?

---

_@Badg_

I've noticed what I think has to be a bug/regression in ruff's implementation of isort grouping. Imports that were previously reported correct by ruff are now being reported as unsorted, in a way that conflicts with pyproject.toml. This is specific to import sections.

Some search keywords:
+ I001
+ import sorting
+ import groups
+ isort sections

Current ruff version is the package control 2024.04.19 release of LSP-ruff in sublime, which is, AFAIK, using the newest version of ruff.

In pyproject.toml, I have this configured (omitting the non-relevant stuff):

```toml
[tool.ruff.lint.isort]
force-single-line = true

[tool.ruff.lint.isort.sections]
testdeps = [
    "tt_website_testutils",
    "tt_website_tevecs",
    "tevec",
    "taev_ams_tevecs",]
```

However, the LSP linter incorrectly reports these as not sorted:

```python
import tempfile
from pathlib import Path
from unittest.mock import Mock

import pytest

from contextual_singleton import Singleton
from contextual_singleton import apply_context
from taev_ams.collections.gitqlite import GitqliteAssetCollection
from taev_ams.collections.gitqlite import GitqliteSqlite
from taev_ams.collections.gitqlite import add_revision
from taev_ams.registry.registry_ import AssetRegistry
from taev_ams.tasks.collection_mgmt.create_collection import create_database

from taev_ams_tevecs.jinja_assets import LocalizedJinjaAsset
from taev_ams_tevecs.sass_assets import SassAsset
from taev_ams_tevecs.static_assets import FakeFontResource
from taev_ams_tevecs.static_assets import LocalizedStaticAsset
from taev_ams_tevecs.static_assets import SimpleStaticAsset
from taev_ams_tevecs.static_assets import SimpleStaticAssetV2
```

When I ask it to organize imports (via code actions), it incorrectly merges ``taev_ams_tevecs`` with the third-party dependencies:

```python
import tempfile
from pathlib import Path
from unittest.mock import Mock

import pytest
from taev_ams_tevecs.jinja_assets import LocalizedJinjaAsset
from taev_ams_tevecs.sass_assets import SassAsset
from taev_ams_tevecs.static_assets import FakeFontResource
from taev_ams_tevecs.static_assets import LocalizedStaticAsset
from taev_ams_tevecs.static_assets import SimpleStaticAsset
from taev_ams_tevecs.static_assets import SimpleStaticAssetV2

from contextual_singleton import Singleton
from contextual_singleton import apply_context
from taev_ams.collections.gitqlite import GitqliteAssetCollection
from taev_ams.collections.gitqlite import GitqliteSqlite
from taev_ams.collections.gitqlite import add_revision
from taev_ams.registry.registry_ import AssetRegistry
from taev_ams.tasks.collection_mgmt.create_collection import create_database
```

I'm... not going crazy here, am I? I'm 99% sure I had ruff sorting these correctly, but I first noticed them being "wrong" ... yesterday? the day before yesterday? I'm not sure. Suspiciously close to the latest release, but memory is a fickle thing.

---

_Comment by @charliermarsh on 2024-04-19 20:31_

I think in https://github.com/astral-sh/ruff/pull/10149, the behavior was changed such that sections that aren't present in the `sections` setting are mapped to the default section, which is `third-party`? So now the imports in `testdeps` are being grouped with `third-party`.

You might want to add something like:

```toml
[tool.ruff.lint.isort.sections]
section-order = [
    "future",
    "third-party",
    "first-party",
    "local-folder",
    "testdeps",
]
```


---

_Label `question` added by @charliermarsh on 2024-04-19 20:31_

---

_Label `isort` added by @charliermarsh on 2024-04-19 20:31_

---

_Comment by @Badg on 2024-04-19 21:05_

Thanks for the lightning-fast response! I updated to this, which fixed the issue: 🎉 

```toml
[tool.ruff.lint.isort]
force-single-line = true
default-section = "third-party"
section-order = [
    "future",
    "standard-library",
    "third-party",
    "first-party",
    "local-folder",
    "testdeps",
]

[tool.ruff.lint.isort.sections]
testdeps = [
    "tt_website_testutils",
    "tt_website_tevecs",
    "tevec",
    "taev_ams_tevecs",]
```

**I think I would still consider this a documentation issue, though maybe just with the release notes.** I know there's explicitly no guarantee of stability right now, but especially when it comes to plugins like LSP packages for IDEs (where I don't realistically have the ability to pin a ruff version), it blindsided me. I'd say part of the difficulty is definitely the slightly-different spelling from isort's docs; when I'm unsure of something, I'm frequently bouncing back and forth between them, and it's just... hard to keep track of stuff, I guess.

Iunno. It's 23:00 on a Friday night here, and I've had a few beers, so maybe I'm just tired haha

---

_Comment by @Badg on 2024-04-19 21:06_

PS feel free to close as resolved -- I'll leave it up to the team if y'all consider this to really be something that needs better documentation or not! :)

---

_Comment by @augustelalande on 2024-04-19 21:19_

FYI LSP-ruff is using ruff-lsp 0.0.52 (latest is 0.0.53), and ruff-lsp 0.0.53 is only on ruff 0.2.2

---

_Comment by @charliermarsh on 2024-04-19 21:59_

I'll add a note on this in the documentation. I _think_ this change was intentionally bringing us more in-line with isort's behavior, and I _thought_ we were warning before when custom sections were omitted from `section-order`, but I might be totally wrong! I'm sorry for the trouble but glad we could resolve quickly.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-19 21:59_

---

_Closed by @charliermarsh on 2024-04-19 22:26_

---
