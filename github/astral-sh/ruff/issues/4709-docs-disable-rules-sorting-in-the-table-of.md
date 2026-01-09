---
number: 4709
title: "docs: Disable rules sorting in the table of contents"
type: issue
state: closed
author: saippuakauppias
labels:
  - documentation
  - question
assignees: []
created_at: 2023-05-29T20:24:41Z
updated_at: 2023-05-29T22:20:43Z
url: https://github.com/astral-sh/ruff/issues/4709
synced_at: 2026-01-07T13:12:14-06:00
---

# docs: Disable rules sorting in the table of contents

---

_Issue opened by @saippuakauppias on 2023-05-29 20:24_

On page https://beta.ruff.rs/docs/rules/ in the table of contents, the rules are sorted by name, and then they go on in the order in which they were added to the linter. 

For ease of use, it is best to disable the sorting of the table of contents.

---

_Comment by @charliermarsh on 2023-05-29 20:59_

Sorry, which list are you suggesting should be reordered, and based on what ordering? The list in the sidebar?

---

_Label `documentation` added by @charliermarsh on 2023-05-29 20:59_

---

_Label `question` added by @charliermarsh on 2023-05-29 20:59_

---

_Comment by @saippuakauppias on 2023-05-29 21:27_

Oh, excuse me. I meant the list here:
<img width="812" alt="image" src="https://github.com/charliermarsh/ruff/assets/945306/f2c183bc-2944-4d61-96cf-d9569237f8b8">


But it turns out that these are links to pypi packages, not links within the page. It's just that this huge list of packages that are supported is confusing, I thought it was the table of contents inside the page.

---

_Comment by @charliermarsh on 2023-05-29 21:53_

Oh, no problem. Yeah, I can see why that's confusing. It may not be correct to include those at all in this context, since the rules are enumerated on the left anyway.

---

_Referenced in [astral-sh/ruff#4715](../../astral-sh/ruff/pulls/4715.md) on 2023-05-29 22:11_

---

_Closed by @charliermarsh on 2023-05-29 22:20_

---
