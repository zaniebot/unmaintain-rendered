---
number: 13684
title: Direct link when searching for a short code
type: issue
state: closed
author: opk12
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2024-10-08T18:45:34Z
updated_at: 2024-11-05T05:57:01Z
url: https://github.com/astral-sh/ruff/issues/13684
synced_at: 2026-01-07T13:12:15-06:00
---

# Direct link when searching for a short code

---

_Issue opened by @opk12 on 2024-10-08 18:45_

When I type a short code in the search bar on the website (https://docs.astral.sh/ruff), for example `ruf005`, the search suggestion is a link to the list of warnings. When I type a long code, for example `collection-literal-concatenation`, the search suggestion is a direct link to the warning's article. Could the short code case be changed?

---

_Comment by @MichaReiser on 2024-10-09 06:27_

Hmm, that's unfortunate. We would have to look into our Algolia search configuration to see how it index pages and either change the Algolia configuration or the structure of the rule's page. I don't think I've access to Algolia (@charliermarsh )

---

_Label `documentation` added by @MichaReiser on 2024-10-09 06:27_

---

_Comment by @charliermarsh on 2024-10-09 08:40_

I think we'd need some sort of support in material-for-mkdocs, e.g., via https://squidfunk.github.io/mkdocs-material/plugins/search/.

---

_Comment by @squidfunk on 2024-10-10 08:35_

Author of Material for MkDocs here – results should drastically improve once we release [the new search](https://github.com/squidfunk/mkdocs-material/issues/6307), which is currently blocked by a bigger refactor we're working on in the background (@charliermarsh we talked about this recently), as mentioned in [our latest blog post](https://squidfunk.github.io/mkdocs-material/blog/2024/08/19/how-were-transforming-material-for-mkdocs/). Once we got a first version of that ready, we'll reach out to you for giving it a try ☺️ We estimate that this should be the case by the beginning of next year.

In the meantime, as a bandaid, you should be able to improve discovery by using tags, and tagging each rule with its modifier. Tags currently take precedence over everything else in search:

``` md
---
tags:
- ruf005
---

# Tag name
```

---

_Label `help wanted` added by @MichaReiser on 2024-10-10 14:02_

---

_Comment by @MichaReiser on 2024-10-10 14:02_

Wow thanks @squidfunk for the details! Adding the tags should be a very straightforward change and looking forward to the new search engine

---

_Referenced in [astral-sh/ruff#14040](../../astral-sh/ruff/pulls/14040.md) on 2024-11-01 14:09_

---

_Comment by @hreeder on 2024-11-01 14:10_

I've pushed a PR that adds these tags to the function used to generate the page metadata - I think this should be all that's required but absolutely open to input & feedback on how to do it.

---

_Comment by @dhruvmanila on 2024-11-04 03:50_

The PR implementing this (#14040) has been merged and has been deployed with the release of `0.7.2`.

<img width="1728" alt="Screenshot 2024-11-04 at 9 19 22 AM" src="https://github.com/user-attachments/assets/46f6ca4a-7274-4df8-b442-fa173da31794">


---

_Closed by @dhruvmanila on 2024-11-04 03:50_

---
