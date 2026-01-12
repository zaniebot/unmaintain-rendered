```yaml
number: 15135
title: GitHub New Issue template appears empty / current template is deprecated
type: issue
state: closed
author: Avasam
labels:
  - help wanted
assignees: []
created_at: 2024-12-24T19:01:52Z
updated_at: 2025-01-15T10:59:23Z
url: https://github.com/astral-sh/ruff/issues/15135
synced_at: 2026-01-12T15:54:54Z
```

# GitHub New Issue template appears empty / current template is deprecated

---

_@Avasam_

With the new GitHub issue view,
![Image](https://github.com/user-attachments/assets/cedf2512-0bda-413e-9e2f-c9b14890f5ec)
this is what I get when creating a new issue:
![Image](https://github.com/user-attachments/assets/256e92f8-b63c-419a-85b4-e4ccea134703)

Likely related to the fact that the current template is deprecated
![Image](https://github.com/user-attachments/assets/88685455-4d9e-4394-8ad6-edb73b816622)

(I made sure to disable Refined GitHub extension in case that was messing with it)

---

_Renamed from "GitHub New Issue template appears empty" to "GitHub New Issue template appears empty / current template is deprecated" by @Avasam on 2024-12-24 19:49_

---

_Comment by @MichaReiser on 2024-12-26 09:25_

Thanks. I didn't know we had an issue template 😆 



---

_Label `help wanted` added by @MichaReiser on 2024-12-26 09:26_

---

_Comment by @InSyncWithFoo on 2024-12-27 17:13_

The new YAML-based format has one minor problem:

* Each issue template must have a body of "form items". These items define how the issue is rendered.
* Each form item must be of one of the following types: `checkboxes`, `dropdown`, `input`, `markdown`, `textarea`. All of these, except for `markdown`, require that a `label` attribute be defined. The value of this attribute is then used as a header.
* The body must contain at least one non-`markdown` block. This means, even for the simplest of issues, an extraneous header will be added.

On the positive side, the form looks quite nice:

![](https://github.com/user-attachments/assets/8dffe1b6-515e-4817-ae07-f0df569fe20c)

By default (no user modifications), the new issue will be created with the following Markdown content:

```md
### This is an input

_No response_

### These are a couple of checkboxes

- [ ] Option 1
- [ ] Option 2

### This is a dropdown menu

None

### This is a multiline textarea

Default value
```

---

_Comment by @MichaReiser on 2024-12-31 15:20_

I posted in the [feedback discussion](https://github.com/orgs/community/discussions/139935#discussioncomment-11706271) to understand if removing support for the legacy issue templates is intentional or not. 

---

_Comment by @MichaReiser on 2025-01-14 15:49_

The old issue templates are being phased out. We either have to migrate or remove it. https://github.com/orgs/community/discussions/148713#discussioncomment-11831926

---

_Comment by @MichaReiser on 2025-01-15 07:18_

We discussed this internally and decided to remove the template for now. The reported issues haven't decreased in quality since the template is gone. We can consider re-introducing it when there's a need. Sorry, @InSyncWithFoo, for the changing instructions. Are you interested in updating your PR accordingly?

---

_Closed by @MichaReiser on 2025-01-15 10:59_

---
