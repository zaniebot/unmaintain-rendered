```yaml
number: 11598
title: " Add document for specifying Python interpreter in tool installation and upgrade commands."
type: pull_request
state: merged
author: FishAlchemist
labels:
  - documentation
assignees: []
merged: true
base: main
head: tool_specifying_python
created_at: 2025-02-18T13:50:02Z
updated_at: 2025-02-18T17:47:03Z
url: https://github.com/astral-sh/uv/pull/11598
synced_at: 2026-01-10T11:10:38Z
```

#  Add document for specifying Python interpreter in tool installation and upgrade commands.

---

_Pull request opened by @FishAlchemist on 2025-02-18 13:50_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Just to add the section for installing and upgrading uv tool, specifying the Python version, in the document.
Originally, it was planned to add a markdown block (header) for representation, but it was felt to be a bit redundant, so it ended up being like this.

close https://github.com/astral-sh/uv/issues/11536

## Test Plan
Run doc server with strict mode in local. (``mkdocs serve -f mkdocs.public.yml --strict``)
![image](https://github.com/user-attachments/assets/9da66a8b-5423-4937-bc66-ea696ad1ab88)



<!-- How was it tested? -->


---

_@zanieb reviewed on 2025-02-18 15:08_

---

_Review comment by @zanieb on `docs/guides/tools.md`:173 on 2025-02-18 15:08_

Could we move these into a dedicated section instead? We're explaining what tool installations are here — it seems distracting to jump to this.

In the guide, I'd put a `## Requesting Python versions` section after installing or upgrading tools? 

I think there should also be a `## Python versions` section in the tool concept page?

---

_@FishAlchemist reviewed on 2025-02-18 15:42_

---

_Review comment by @FishAlchemist on `docs/guides/tools.md`:173 on 2025-02-18 15:42_

> Could we move these into a dedicated section instead? We're explaining what tool installations are here — it seems distracting to jump to this.
In the guide, I'd put a ## Requesting Python versions section after installing or upgrading tools?

Yes, but should I put two commands (**install** and **upgrade**), or just put the **install** command?

> I think there should also be a ## Python versions section in the tool concept page?

Yes, it can be added, but perhaps it's only necessary to mention the minor differences and then point to https://docs.astral.sh/uv/concepts/python-versions/ .

---

_@zanieb reviewed on 2025-02-18 15:59_

---

_Review comment by @zanieb on `docs/guides/tools.md`:173 on 2025-02-18 15:59_

I think it'd be reasonable to be like..

To request a specific Python version for the tool installation:

...

To change the Python version of an existing tool during upgrade:

....


---

_Review comment by @zanieb on `docs/guides/tools.md`:173 on 2025-02-18 15:59_

(and yeah linking to the Python versions concept page seems correct)

---

_@zanieb reviewed on 2025-02-18 15:59_

---

_@FishAlchemist reviewed on 2025-02-18 16:18_

---

_Review comment by @FishAlchemist on `docs/guides/tools.md`:173 on 2025-02-18 16:18_


![image](https://github.com/user-attachments/assets/35395088-5b60-4b3d-bb37-3af37da5a79e)
Regarding the concept, to avoid misunderstanding it, I will refrain from modifying it for now. However, I have linked the guides to it.


---

_@zanieb approved on 2025-02-18 17:45_

Thank you! I made some small changes.

---

_Merged by @zanieb on 2025-02-18 17:45_

---

_Closed by @zanieb on 2025-02-18 17:45_

---

_Label `documentation` added by @zanieb on 2025-02-18 17:46_

---

_Branch deleted on 2025-02-18 17:47_

---
