```yaml
number: 2167
title: "Document `configuration` and `configurationFile` editor settings"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/configuration-and-configuration-file
created_at: 2025-12-22T16:42:08Z
updated_at: 2025-12-23T09:10:42Z
url: https://github.com/astral-sh/ty/pull/2167
synced_at: 2026-01-10T02:34:11Z
```

# Document `configuration` and `configurationFile` editor settings

---

_Pull request opened by @MichaReiser on 2025-12-22 16:42_

<img width="1625" height="1561" alt="Screenshot 2025-12-23 at 10 06 58" src="https://github.com/user-attachments/assets/522c9279-dfc0-476f-a8a5-0504150cd71c" />


---

_Label `documentation` added by @MichaReiser on 2025-12-22 16:42_

---

_@MichaReiser reviewed on 2025-12-22 16:42_

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:41 on 2025-12-22 16:42_

I hope I go the syntax correct

---

_@dhruvmanila reviewed on 2025-12-23 06:02_

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:41 on 2025-12-23 06:02_

Yes, this is correct

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:9 on 2025-12-23 06:04_

We should expand this to make it clear that the "configuration files" also include the ones provided in `configurationFile`

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:17 on 2025-12-23 06:07_

It would be cool to include these example usages in https://docs.astral.sh/ty/reference/configuration as well to answer "how to specify these settings directly in the editor?". We can create a follow-up issue for this

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:83 on 2025-12-23 06:09_

I believe this needs to be indented?


```suggestion
!!! info

    While ty configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
```

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:1 on 2025-12-23 06:12_

It might be useful to provide a "Resolution order" between `configuration`, `configurationFile` and a local configuration file similar to https://docs.astral.sh/ruff/editors/settings/#configuration_resolution_order. And, I think in the future it would also include settings that can be directly specified.

---

_@dhruvmanila approved on 2025-12-23 06:12_

---

_@MichaReiser reviewed on 2025-12-23 07:46_

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:1 on 2025-12-23 07:46_

Agree, I intentionally skipped this for now to have something working sooner and get some user feedback too

---

_@MichaReiser reviewed on 2025-12-23 09:05_

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:17 on 2025-12-23 09:05_

Not sure, it feels a bit annoying if we have to mention this for every setting.

---

_@MichaReiser reviewed on 2025-12-23 09:06_

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:83 on 2025-12-23 09:06_

Hmm yes. Pre-commit reverts a single tab indent, but using two seems to work fine

---

_Merged by @MichaReiser on 2025-12-23 09:10_

---

_Closed by @MichaReiser on 2025-12-23 09:10_

---

_Branch deleted on 2025-12-23 09:10_

---
