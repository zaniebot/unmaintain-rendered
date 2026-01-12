```yaml
number: 4092
title: Add note for install of specific versions of uv
type: pull_request
state: closed
author: zanieb
labels:
  - documentation
assignees: []
base: main
head: zb/readme-install
created_at: 2024-06-06T12:54:43Z
updated_at: 2024-06-06T17:58:08Z
url: https://github.com/astral-sh/uv/pull/4092
synced_at: 2026-01-12T16:06:02Z
```

# Add note for install of specific versions of uv

---

_@zanieb_

I opted not to have this in the getting started section to avoid complexity there. I figure this will move during docs restructuring soon.

---

_Label `documentation` added by @zanieb on 2024-06-06 12:54_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-06 12:58_

---

_@charliermarsh reviewed on 2024-06-06 13:05_

---

_Review comment by @charliermarsh on `README.md`:143 on 2024-06-06 13:05_

Can we move this lower down? It seems fringe compared to other topics here.

---

_@zanieb reviewed on 2024-06-06 13:15_

---

_Review comment by @zanieb on `README.md`:143 on 2024-06-06 13:15_

I don't think it's fringe, I feel like pinning the version is good practice and pretty normal?

---

_@zanieb reviewed on 2024-06-06 13:15_

---

_Review comment by @zanieb on `README.md`:143 on 2024-06-06 13:15_

Do you want it under the entire advanced usage section?

---

_@zanieb reviewed on 2024-06-06 13:36_

---

_Review comment by @zanieb on `README.md`:143 on 2024-06-06 13:36_

Moved down

---

_Review comment by @charliermarsh on `README.md`:65 on 2024-06-06 13:40_

Can we remove this? 

---

_Review comment by @charliermarsh on `README.md`:490 on 2024-06-06 13:41_

For consistency:

```
uv's standalone installer can also be pinned to a specific version. For example,
```

---

_Review comment by @charliermarsh on `README.md`:514 on 2024-06-06 13:41_

I would omit this.

---

_@charliermarsh approved on 2024-06-06 13:41_

---

_@zanieb reviewed on 2024-06-06 13:43_

---

_Review comment by @zanieb on `README.md`:65 on 2024-06-06 13:43_

I really do not want people to have to scroll to the bottom of the documentation to discover how to pin the version with the standalone installer. Can we compromise here? I'm happy to rephrase it.

---

_@charliermarsh reviewed on 2024-06-06 13:56_

---

_Review comment by @charliermarsh on `README.md`:65 on 2024-06-06 13:56_

But this doesn't actually tell people how to pin versions with the standalone installer. Is it more useful than having to Command + F for "install"? (If it's that important, should it be up here in the preamble?)


---

_Review comment by @charliermarsh on `README.md`:65 on 2024-06-06 13:57_

E.g., could the whole change just be a line like:

```
# Or a specific version.
curl -LsSf https://astral.sh/uv/0.2.6/install.sh | sh
```

In the above code block? Isn't that more economical than adding this to the real estate of the Getting Started section?

---

_@charliermarsh reviewed on 2024-06-06 13:57_

---

_@zanieb reviewed on 2024-06-06 14:04_

---

_Review comment by @zanieb on `README.md`:65 on 2024-06-06 14:04_

> But this doesn't actually tell people how to pin versions with the standalone installer.

That's what I was getting at when I said I'd rephrase.

>  Isn't that more economical than adding this to the real estate of the Getting Started section?

Yeah. So that was my first thought, but there are two small problems

1. I don't want the getting started to suggest installing an out of date version. We can resolve this by adding the README to rooster's target files though so it's always the latest in the example.
2. It's a little awkward given that we split between unix and windows, `# Or, a specific version on MacOS and Linux`?

I'd prefer this honestly just not sure how to phrase it since it doesn't quite fit into the style of the rest of the code block.

---

_@zanieb reviewed on 2024-06-06 16:33_

---

_Review comment by @zanieb on `README.md`:65 on 2024-06-06 16:33_

Okay: https://github.com/astral-sh/uv/pull/4105

---

_Comment by @zanieb on 2024-06-06 17:58_

Using #4105 instead

---

_Closed by @zanieb on 2024-06-06 17:58_

---
