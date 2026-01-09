---
number: 12154
title: "Proposal: detect exposed secrets"
type: issue
state: open
author: knyazer
labels:
  - rule
assignees: []
created_at: 2024-07-02T16:58:46Z
updated_at: 2024-07-03T06:32:34Z
url: https://github.com/astral-sh/ruff/issues/12154
synced_at: 2026-01-07T13:12:15-06:00
---

# Proposal: detect exposed secrets

---

_Issue opened by @knyazer on 2024-07-02 16:58_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  "secrets", "api keys", "exposed"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hey, guys,

It would be wonderful if ruff could detect exposed API keys. This is a pretty common security vulnerability. However, it is not included as part of flake8-bandit, probably because it would require quite a bit of effort to make sure that the api keys regexes are up to date. But, I still believe this is a pretty useful rule.

I can implement a rule like this, for example in a similar fashion to [this project](https://github.com/sdushantha/dora/blob/main/dora/db/data.json), with a neat json/yaml/toml file, that would include all the api key regex that we are detecting. But, while I can promise support for this over the next year or so, I am obviously going to stop supporting it at some point: new apis are appearing every second, and making changes for all of them for the rest of my life is not something I would want to do. 

I still believe in usability of such a rule, even if maintenance for it would be dropped. I would expect that half of the regexes would rot (become invalid) over the course of 5 years, which is quite quickly, but still, it is better than nothing.

So, if you believe this is a nice and useful rule, what would be the steps I shall take before making a PR into ruff? Currently, the most relevant source I found about it is this [issue](https://github.com/PyCQA/bandit/issues/443), which seems abandoned, so I might actually implement it PyCQA. But then the route to get it into ruff through bandit is pretty long: bandit -> flake8-bandit -> ruff, so I wonder if there is any fast-track way to do it.



---

_Label `rule` added by @MichaReiser on 2024-07-03 06:32_

---
