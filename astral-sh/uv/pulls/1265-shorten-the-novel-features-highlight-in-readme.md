```yaml
number: 1265
title: Shorten the novel features highlight in README
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/novel-features
created_at: 2024-02-08T18:22:10Z
updated_at: 2024-02-15T21:56:06Z
url: https://github.com/astral-sh/uv/pull/1265
synced_at: 2026-01-10T15:33:24Z
```

# Shorten the novel features highlight in README

---

_Pull request opened by @zanieb on 2024-02-08 18:22_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-02-08 18:22_

---

_Review comment by @konstin on `README.md`:28 on 2024-02-08 19:29_

```suggestion
- 🧰 [dependency version overrides](#dependency-overrides] and [alternative resolution strategies](#resolution-strategy).
```

I'd replace the word "novel" with something less formal

---

_@konstin approved on 2024-02-08 19:29_

---

_@charliermarsh reviewed on 2024-02-08 21:10_

---

_Review comment by @charliermarsh on `README.md`:28 on 2024-02-08 21:10_

Could just be "Support for"? Don't feel strongly, defer to Zanie on that one.

---

_@charliermarsh approved on 2024-02-08 21:10_

---

_@zanieb reviewed on 2024-02-08 21:13_

---

_Review comment by @zanieb on `README.md`:28 on 2024-02-08 21:13_

I like that we point out that it's innovative. Not sure how to portray that. "Novel" sounds cool but is definitely formal. @BurntSushi master marketeer.

---

_@konstin reviewed on 2024-02-08 21:40_

---

_Review comment by @konstin on `README.md`:28 on 2024-02-08 21:40_

Does anyone else support them? If not "Exclusive features" would work, otherwise just "New features"

---

_@zanieb reviewed on 2024-02-08 23:04_

---

_Review comment by @zanieb on `README.md`:28 on 2024-02-08 23:04_

Nobody else supports these in Python

---

_@zanieb reviewed on 2024-02-08 23:11_

---

_Review comment by @zanieb on `README.md`:28 on 2024-02-08 23:11_

What about "Original features"? I think "Novel" is also fine tbh, unless there's a major objection?

---

_@konstin reviewed on 2024-02-09 02:16_

---

_Review comment by @konstin on `README.md`:28 on 2024-02-09 02:16_

Perfect!

---

_Review comment by @BurntSushi on `README.md`:28 on 2024-02-10 01:13_

I personally don't think "novel" is too formal of a word. With that said, if we are claiming novelty or original features, we should be really sure we are correct and that our claim is properly scoped.

---

_@BurntSushi reviewed on 2024-02-10 01:13_

---

_@zanieb reviewed on 2024-02-12 20:00_

---

_Review comment by @zanieb on `README.md`:28 on 2024-02-12 20:00_

I'm going to merge this and address this change separately — it's not a part of the intent of this pull request and there needs to be more discussion

---

_Merged by @zanieb on 2024-02-12 20:04_

---

_Closed by @zanieb on 2024-02-12 20:04_

---

_Branch deleted on 2024-02-12 20:04_

---

_@ofek reviewed on 2024-02-15 21:15_

---

_Review comment by @ofek on `README.md`:28 on 2024-02-15 21:15_

PDM supports dependency overrides https://pdm-project.org/latest/usage/config/#override-the-resolved-package-versions

Out of curiosity, what projects did you investigate in Python?

---

_@zanieb reviewed on 2024-02-15 21:56_

---

_Review comment by @zanieb on `README.md`:28 on 2024-02-15 21:56_

Hey! Thanks for pointing that out — we've looked at PDM but I missed that.

As noted here, my goal in this pull request wasn't to discuss the use of the word "novel". Our goal is to highlight new features beyond `pip` and `pip-tools` / things that aren't commonly available in the Python community. I'm happy to change the wording.

---
