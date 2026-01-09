---
number: 7453
title: Change Ruff Rules url to include the rule-id
type: issue
state: closed
author: Crissal1995
labels:
  - question
assignees: []
created_at: 2023-09-17T10:48:14Z
updated_at: 2023-09-18T17:27:07Z
url: https://github.com/astral-sh/ruff/issues/7453
synced_at: 2026-01-07T13:12:15-06:00
---

# Change Ruff Rules url to include the rule-id

---

_Issue opened by @Crissal1995 on 2023-09-17 10:48_

Hi,

I created a tool that uses Ruff under the hood to check my users' codebase, and warn them in case at least one Ruff violation is found.
In particular, I added (Github) annotations to their files in the PR page, adding a inline comment with the violated rule(s).

The problem, however, is that I would like to include an url to the violated rule, so that users can just click on the url in the annotation and land on the rule page, e.g., https://docs.astral.sh/ruff/rules/unused-import/

But the rule path is the rule name written in kebab-case, and not the rule id.

Since I parse the output returned from `ruff check`, this would be the easiest change; in the example, the url should become https://docs.astral.sh/ruff/rules/F401

Is it possible/feasible to change the urls? Or maybe setup a redirect, keeping only one page?


---

_Comment by @dhruvmanila on 2023-09-18 04:56_

Hey, thanks for opening this issue.

> In particular, I added (Github) annotations to their files in the PR page, adding a inline comment with the violated rule(s).

I am curious to know what the tool does as we do provide GitHub annotations via the `--format github` which then is picked up by GitHub to annotate the code in the pull request diff.

> The problem, however, is that I would like to include an url to the violated rule, so that users can just click on the url in the annotation and land on the rule page, e.g., [docs.astral.sh/ruff/rules/unused-import](https://docs.astral.sh/ruff/rules/unused-import/)

We provide the URL in the JSON output (`ruff check --format json`):
```json
[
  {
    "code": "F821",
    "end_location": {
      "column": 6,
      "row": 3
    },
    "filename": "/Users/dhruv/playground/ruff/fstring.py",
    "fix": null,
    "location": {
      "column": 5,
      "row": 3
    },
    "message": "Undefined name `x`",
    "noqa_row": 6,
    "url": "https://docs.astral.sh/ruff/rules/undefined-name"
  },
  {
    "code": "F821",
    "end_location": {
      "column": 14,
      "row": 5
    },
    "filename": "/Users/dhruv/playground/ruff/fstring.py",
    "fix": null,
    "location": {
      "column": 13,
      "row": 5
    },
    "message": "Undefined name `y`",
    "noqa_row": 6,
    "url": "https://docs.astral.sh/ruff/rules/undefined-name"
  }
]
```

I hope this helps for your use-case. Feel free to ask any other questions if not.

---

_Closed by @dhruvmanila on 2023-09-18 04:56_

---

_Label `question` added by @dhruvmanila on 2023-09-18 04:56_

---

_Comment by @Crissal1995 on 2023-09-18 17:00_

Hey, thanks for the detailes reply!

For the first bit, actually I annotate the code in a separate step (not during `ruff check`), but I will give it a try.

I was instead oblivious of the output format, that looks really nice and complete - and easier to parse than stdout logs 🫠.

---

_Comment by @dhruvmanila on 2023-09-18 17:27_

Sorry there was a typo. It's the `--format` flag and not the `--output` flag 😅 

```
      --format <FORMAT>
          Output serialization format for violations [env: RUFF_FORMAT=] [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure]
```

---
