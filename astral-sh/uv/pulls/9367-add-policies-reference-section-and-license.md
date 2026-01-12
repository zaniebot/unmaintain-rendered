```yaml
number: 9367
title: Add policies reference section and license document
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/reference-sections
created_at: 2024-11-22T20:03:58Z
updated_at: 2024-12-03T17:33:16Z
url: https://github.com/astral-sh/uv/pull/9367
synced_at: 2026-01-12T16:08:46Z
```

# Add policies reference section and license document

---

_@zanieb_

In preparation for adding more pages to the reference section

e.g., 
<img width="1427" alt="Screenshot 2024-11-27 at 1 19 10 PM" src="https://github.com/user-attachments/assets/10079959-ee01-4c3f-b38e-7c2a7022dd9d">


---

_Label `documentation` added by @zanieb on 2024-11-22 20:03_

---

_Comment by @FishAlchemist on 2024-11-23 07:14_

 If the pr is going to include the license in a document, what should I link to in the license section for other distribution methods like winget? Should I link to the document or to GitHub?
 
 To prevent link broken, I'm currently using a tag-specific GitHub link on winget-pkgs, for example, version 0.5.3 is linked this way.
 ```
 https://github.com/astral-sh/uv/blob/0.5.3/README.md#license
 ```

---

_Comment by @zanieb on 2024-11-25 21:16_

The documentation isn't versioned, so we don't need to use permalinks yet (though we may want to in the long term).

It sounds like linking to the specific version's license makes sense for downstream distribution.

---

_Marked ready for review by @zanieb on 2024-11-27 20:03_

---

_Merged by @zanieb on 2024-12-03 16:56_

---

_Closed by @zanieb on 2024-12-03 16:56_

---

_Branch deleted on 2024-12-03 16:56_

---

_Comment by @FishAlchemist on 2024-12-03 17:22_

@zanieb The document currently isn't versioned , and there are two license descriptions in the repository. If a downstream distribution requires a license link, which one should I choose?

(Documentation on github)
1. github/astral-sh/uv/blob/main/docs/reference/policies/license.md
2. github/astral-sh/uv/blob/``version tag``/docs/reference/policies/license.md

(README link)

3. github/astral-sh/uv/blob/main/README.md#license
4. github/astral-sh/uv/blob/``version tag``/README.md#license

(Documentation on astral-sh website)

5.  docs.astral.sh/uv/reference/policies/license

---

_Comment by @zanieb on 2024-12-03 17:33_

I don't think it matters honestly, but I'd use `github/astral-sh/uv/blob/version tag/README.md#license`

---
