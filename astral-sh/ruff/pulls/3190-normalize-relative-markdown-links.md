```yaml
number: 3190
title: Normalize relative markdown links
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: normalize-docs-links
created_at: 2023-02-23T19:47:47Z
updated_at: 2023-02-23T22:23:42Z
url: https://github.com/astral-sh/ruff/pull/3190
synced_at: 2026-01-12T15:55:12Z
```

# Normalize relative markdown links

---

_@JonathanPlasse_

Use `/docs/` instead of `https://beta.ruff.rs/docs/`
Fix `hierarchical and cascading configuration` links
Remove outdated `DOCUMENTATION_LINK` removal which is not in `Overview` anymore.

---

_Review comment by @charliermarsh on `scripts/generate_mkdocs.py`:49 on 2023-02-23 19:57_

This _is_ in the docs, is it just that those portions of the README are no longer included in the docs? (I believe so... just confirming.)

---

_@charliermarsh reviewed on 2023-02-23 19:57_

---

_@JonathanPlasse reviewed on 2023-02-23 20:34_

---

_Review comment by @JonathanPlasse on `scripts/generate_mkdocs.py`:49 on 2023-02-23 20:34_

I checked the diff with and without removing these lines.
Neither are included in a section like `Overview` or `Rules`, the only remaining sections in the `README.md`.
```markdown
<!-- End section: Overview -->

## Table of Contents

For more, see the [documentation](https://beta.ruff.rs/docs/).

1. [Getting Started](#getting-started)
2. [Configuration](#configuration)
3. [Rules](#rules)
4. [Contributing](#contributing)
5. [Support](#support)
6. [Acknowledgements](#acknowledgements)
7. [Who's Using Ruff?](#whos-using-ruff)
8. [License](#license)

## Getting Started

For more, see the [documentation](https://beta.ruff.rs/docs/).
```

---

_Review comment by @JonathanPlasse on `scripts/generate_mkdocs.py`:49 on 2023-02-23 20:36_

Yes, it is in the GitHub docs, but no longer in the `beta.ruff.rs/docs` docs.

---

_@JonathanPlasse reviewed on 2023-02-23 20:36_

---

_Merged by @charliermarsh on 2023-02-23 21:24_

---

_Closed by @charliermarsh on 2023-02-23 21:24_

---

_Branch deleted on 2023-02-23 22:23_

---
