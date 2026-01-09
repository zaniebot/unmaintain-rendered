---
number: 1421
title: "[Question] Is there autofix for E501 and how do I turn it on?"
type: issue
state: closed
author: simkimsia
labels: []
assignees: []
created_at: 2022-12-28T07:23:20Z
updated_at: 2022-12-28T12:15:54Z
url: https://github.com/astral-sh/ruff/issues/1421
synced_at: 2026-01-07T13:12:14-06:00
---

# [Question] Is there autofix for E501 and how do I turn it on?

---

_Issue opened by @simkimsia on 2022-12-28 07:23_

i'm one of your latest stars after you hit 4k.

So I'm a newbie.

I have searched in the issue before posting this.

I know you labelled E501 as not autofix in the readme file. 

I was wondering if this means there is autofix but it's not turned on, OR there isn't autofix at all?

If there isn't what's your recommended for the autofix for format on save? i'm using vscode.

Thank you

---

_Comment by @notypecheck on 2022-12-28 07:40_

If it's not labeled as auto-fixable in readme then it's not (at least currently), I personally run black, isort and then ruff

---

_Comment by @simkimsia on 2022-12-28 08:04_

@ThirVondukr why still need isort when it's already autofix? https://github.com/charliermarsh/ruff#isort-i

---

_Comment by @notypecheck on 2022-12-28 09:03_

@simkimsia Isort and ruff didn't/don't produce identical results

---

_Comment by @charliermarsh on 2022-12-28 12:15_

Welcome!

E501 isn't autofixable right now. It will be in the future, in some cases, but that requires implementing a full autoformatter.

If you want to use Ruff with Black (such that Ruff handles lint errors, and Black handles formatting), you could try this configuration:

```json
{
    "[python]": {
        "editor.formatOnSave": true,
        "editor.codeActionsOnSave": {
            "source.fixAll": true
        }
    },
    "python.formatting.provider": "black"
}
```


---

_Closed by @charliermarsh on 2022-12-28 12:15_

---
