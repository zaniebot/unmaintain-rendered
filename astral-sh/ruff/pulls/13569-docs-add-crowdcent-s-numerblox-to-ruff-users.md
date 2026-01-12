```yaml
number: 13569
title: "[DOCS] Add CrowdCent's `numerblox` to Ruff users."
type: pull_request
state: merged
author: CarloLepelaars
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-09-30T13:49:17Z
updated_at: 2024-11-01T13:31:51Z
url: https://github.com/astral-sh/ruff/pull/13569
synced_at: 2026-01-12T15:55:44Z
```

# [DOCS] Add CrowdCent's `numerblox` to Ruff users.

---

_@CarloLepelaars_

Hi, our open source project [NumerBlox](https://github.com/crowdcent/numerblox) migrated to `uv` and `ruff`. Would appreciate the project being included in the list of Ruff users.

## Summary

Add [NumerBlox](https://github.com/crowdcent/numerblox) to Ruff users in README.md. 

---

_Renamed from "[DOCS] Add CrowdCent `numerblox` to Ruff users." to "[DOCS] Add CrowdCent's `numerblox` to Ruff users." by @CarloLepelaars on 2024-09-30 13:49_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-10-07 12:52_

---

_@MichaReiser requested changes on 2024-10-07 12:52_

Could you take a look at the formatting (the pre-commit test is failing)

---

_Comment by @CarloLepelaars on 2024-10-07 13:02_

> Could you take a look at the formatting (the pre-commit test is failing)

Hey @MichaReiser, thank you for the reply! The pre-commit thinks `NumerBlox` is a typo, but it is not. 

I tried:
1. Adding `NumerBlox` to `_typos.toml`
2. Adding inline comment `<!-- typos: ignore -->` to the README line.

Though these don't work. Do you have an idea on how to make sure the pre-commit doesn't classify `NumerBlox` as a typo? This is the only obstacle.

---

_Comment by @MichaReiser on 2024-10-07 13:10_

Adding it to `types.toml should do the job

---

_Comment by @CarloLepelaars on 2024-10-07 13:28_

I've added `NumerBlox` to `_typos.toml`, but unfortunately the pre-commit still fails. Is there something I'm missing?

---

_Comment by @calumy on 2024-10-07 15:17_

> I've added `NumerBlox` to `_typos.toml`, but unfortunately the pre-commit still fails. Is there something I'm missing?

Could you try just `Numer` rather than `NumerBlox` as this is the bit that typos appears to be trying to correct.


---

_Comment by @CarloLepelaars on 2024-10-07 22:24_

> Could you try just `Numer` rather than `NumerBlox` as this is the bit that typos appears to be trying to correct.

Thank you for the suggestion @calumy! All CI is passing now!



---

_Review requested from @MichaReiser by @MichaReiser on 2024-10-08 06:31_

---

_Assigned to @charliermarsh by @AlexWaygood on 2024-10-21 13:43_

---

_Merged by @charliermarsh on 2024-10-28 14:53_

---

_Closed by @charliermarsh on 2024-10-28 14:53_

---

_Comment by @charliermarsh on 2024-10-28 14:54_

Thank you for the PR @CarloLepelaars. Just as a heads up, I may remove this in the future as I'd like to redo the list to focus it on "highly notable" projects (either significant open source usage or from well-known enterprises).


---

_Comment by @CarloLepelaars on 2024-10-28 15:27_

That's ok @charliermarsh. Thank you for including us in the list in the meantime!

---

_Branch deleted on 2024-10-28 15:27_

---

_Label `documentation` added by @dhruvmanila on 2024-11-01 13:31_

---
