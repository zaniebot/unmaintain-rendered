```yaml
number: 176
title: ruff uncondtionally ignores all files in folders starting with a dot
type: issue
state: closed
author: Jackenmen
labels: []
assignees: []
created_at: 2022-09-13T02:02:59Z
updated_at: 2022-09-15T02:21:18Z
url: https://github.com/astral-sh/ruff/issues/176
synced_at: 2026-01-12T15:54:40Z
```

# ruff uncondtionally ignores all files in folders starting with a dot

---

_@Jackenmen_

Noticed it while looking through some of the ruff's code and I realized that this means that when I put scripts in a folder such as `.github/workflows/scripts`, ruff won't lint it unless I explicitly pass the `.github` folder to it (I verified this by checking the output of ruff with `-v` flag on a project where I have Python files in `.github` folder).

I'm unsure what's a good solution here though, clearly there can be hidden folders which a user would want linter to include (as the mentioned `.github` folder) *but* there are many hidden folders (and some not hidden) that you would want to be excluded by default.
flake8 keeps a small default exclusion list but personally I think it's way too small as it doesn't even include such a common exclusion as `.venv` folder (and what's worse, flake8 also doesn't allow users to add user-specific exclusions anymore either).

#174 might make it less painful if the default exclusion of all hidden folders were removed but I'm not sure it solves the problem entirely as I imagine you could want to test non-Git projects with ruff and still get reasonable results. I think that adding some default exclusion list could be beneficial here but I don't think it should really be as small as flake8's - I think Black's default exclusion list is a lot better in that regard.

---

_Comment by @charliermarsh on 2022-09-13 14:05_

Yeah this makes sense. I'll just mimic whatever Black does.

---

_Label `enhancement` added by @charliermarsh on 2022-09-13 14:51_

---

_Closed by @charliermarsh on 2022-09-15 02:21_

---
