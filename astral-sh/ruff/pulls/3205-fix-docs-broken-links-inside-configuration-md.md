```yaml
number: 3205
title: "fix(docs): broken links inside Configuration.md"
type: pull_request
state: merged
author: carlosmiei
labels: []
assignees: []
merged: true
base: main
head: fix-docs
created_at: 2023-02-24T11:48:46Z
updated_at: 2023-02-24T22:10:45Z
url: https://github.com/astral-sh/ruff/pull/3205
synced_at: 2026-01-12T15:55:12Z
```

# fix(docs): broken links inside Configuration.md

---

_@carlosmiei_

- Found some relative paths pointing to unexistent files, so decided to redirect them to the docs page. 
- Not sure this is the right fix but it's better than nothing I guess :) 




---

_@charliermarsh reviewed on 2023-02-24 15:38_

---

_Review comment by @charliermarsh on `docs/configuration.md`:116 on 2023-02-24 15:38_

Ah yeah this is wrong.

---

_Review comment by @charliermarsh on `docs/configuration.md`:6 on 2023-02-24 15:38_

I think these are intentional because they're relative links once the docs are _live_.

---

_@charliermarsh reviewed on 2023-02-24 15:38_

---

_@charliermarsh reviewed on 2023-02-24 15:39_

---

_Review comment by @charliermarsh on `docs/configuration.md`:6 on 2023-02-24 15:39_

E.g., if you open. https://beta.ruff.rs/docs/configuration/, and click Settings, it takes you to the right place.

---

_@andersk reviewed on 2023-02-24 17:25_

---

_Review comment by @andersk on `docs/configuration.md`:6 on 2023-02-24 17:25_

Most Markdown documentation systems including MkDocs ([documentation](https://www.mkdocs.org/user-guide/writing-your-docs/#linking-to-pages)) expect you to write relative links to Markdown files: `[_Settings_](settings.md)`, `[_FAQ_](faq.md#ruff-tried-to-fix-something-but-it-broke-my-code)`. These are transformed at build time to point to the right HTML page.

---

_@charliermarsh reviewed on 2023-02-24 17:58_

---

_Review comment by @charliermarsh on `docs/configuration.md`:6 on 2023-02-24 17:58_

I'll fix this.

---

_Review comment by @charliermarsh on `scripts/generate_mkdocs.py`:43 on 2023-02-24 18:19_

I will probably change this before merging.

---

_@charliermarsh reviewed on 2023-02-24 18:19_

---

_Merged by @charliermarsh on 2023-02-24 18:55_

---

_Closed by @charliermarsh on 2023-02-24 18:55_

---

_@carlosmiei reviewed on 2023-02-24 21:56_

---

_Review comment by @carlosmiei on `docs/configuration.md`:6 on 2023-02-24 21:56_

@charliermarsh don't know if you're aware, but this is still broken, open https://github.com/charliermarsh/ruff/blob/main/docs/configuration.md and try to click `see Settings` because it redirects to the relative `Settings.md` which does not exist

---

_Review comment by @charliermarsh on `docs/configuration.md`:6 on 2023-02-24 22:05_

I think this is expected though. These files are meant to be accessed via the docs (https://beta.ruff.rs/), rather than through GitHub.

---

_@charliermarsh reviewed on 2023-02-24 22:05_

---

_Review comment by @carlosmiei on `docs/configuration.md`:6 on 2023-02-24 22:10_

@charliermarsh oh ok my bad then, I was assuming it should work both sides but I was wrong 😅.

---

_@carlosmiei reviewed on 2023-02-24 22:10_

---
