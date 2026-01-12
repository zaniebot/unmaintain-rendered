```yaml
number: 4710
title: "docs: feature-request: add aliases for short codes and add new format mode"
type: issue
state: closed
author: saippuakauppias
labels:
  - documentation
assignees: []
created_at: 2023-05-29T20:35:18Z
updated_at: 2023-11-12T22:47:12Z
url: https://github.com/astral-sh/ruff/issues/4710
synced_at: 2026-01-12T15:54:44Z
```

# docs: feature-request: add aliases for short codes and add new format mode

---

_@saippuakauppias_

It would be great if for every rule that has a page in the documentation you could go to it not only by its full name (`unused-import`: https://beta.ruff.rs/docs/rules/unused-import/ ) but also by its short code (`F401`: https://beta.ruff.rs/docs/rules/F401/). 
This would make it easier to find best practice examples for this bug.

Also, I suggest adding a separate formatting mode based on `text` (e.g., call it `text_links`). In which a link to such pages would be given next to the short error code. For example, it could look like this:
```
web/currency/models.py:1:1: I001 (https://beta.ruff.rs/docs/rules/I001/) [*] Import block is un-sorted or un-formatted
web/currency/models.py:10:7: N818 (https://beta.ruff.rs/docs/rules/N818/) Exception name `CurrencyNotFound` should be named with an Error suffix
web/currency/models.py:20:121: E501 (https://beta.ruff.rs/docs/rules/E501/) Line too long (152 > 120 characters)
```

Or, for example, add a separate setting to enable link display mode, which will add links for formatting modes: `text`, `grouped`

This would make it a lot easier to find "how to do it better" examples and links to the original rules from the linters (which are on the rules pages) and would speed up the process of correcting mistakes.

PS: In option `format` there is no `groupped` in the default values.

---

_Comment by @charliermarsh on 2023-05-29 20:58_

You actually _can_ link to these on the main page via anchors, like https://beta.ruff.rs/docs/rules#F401, but it's not ideal. However, we do want to move towards using the human-readable names everywhere eventually, so I'm not likely to prioritize this _personally_.

---

_Label `documentation` added by @charliermarsh on 2023-05-29 20:58_

---

_Comment by @saippuakauppias on 2023-05-29 21:37_

Long names are great too, but they will take up a lot of space in the links (if implemented) within the error.

I saw this idea in the linter [ShellCheck for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=timonwong.shellcheck), when hovering over the error in the IDE - a link is displayed (in the example it is SC2086 on the right) and when you click on it we get to the page: https://www.shellcheck.net/wiki/SC2086
<img width="659" alt="Снимок экрана 2023-05-30 в 00 32 56" src="https://github.com/charliermarsh/ruff/assets/945306/a7207a82-c3c4-4cd5-a261-186da7caf591">


This is very handy: if you don't know what is the right fix, go to the documentation for the rule and see examples.

PS: or make a subdomain with short links that will redirect to long links. For example: `https://r.ruff.rs/F401` will redirect to `https://beta.ruff.rs/docs/rules/unused-import/`

---

_Comment by @charliermarsh on 2023-06-07 04:13_

It would be nice to include those links in the LSP.

---

_Comment by @charliermarsh on 2023-06-19 01:54_

One thing we could do: add a Cloudflare Worker to redirect `/rules/F401` to `/rules/unused-import`.

---

_Comment by @saippuakauppias on 2023-06-19 16:36_

That would be wonderful!

After that, someone can embed links in the error log to go directly from the console to the rule description

---

_Closed by @charliermarsh on 2023-11-12 22:47_

---
