```yaml
number: 7486
title: "Documentation: add `--no-preview` to `--help`"
type: issue
state: closed
author: bersbersbers
labels:
  - documentation
assignees: []
created_at: 2023-09-18T09:24:57Z
updated_at: 2023-09-19T13:22:48Z
url: https://github.com/astral-sh/ruff/issues/7486
synced_at: 2026-01-12T15:54:47Z
```

# Documentation: add `--no-preview` to `--help`

---

_@bersbersbers_

I like using `ruff` with `preview` rules, locally - so I have added `preview = true` to my `pyproject.toml`, and it seems to be picked up by VS Code, `pre-commit`, and manual command-line invocations of `ruff`. However, I think I would prefer new preview rules to break CI. I could pin the `ruff` version, but would like to avoid that ideally. Could the `--preview` command-line flag be made switchable, e.g., to allow `ruff --preview=false` on CI?

---

_Comment by @dhruvmanila on 2023-09-18 12:11_

I believe you're looking for the `--no-preview` flag.

---

_Label `question` added by @dhruvmanila on 2023-09-18 12:12_

---

_Comment by @bersbersbers on 2023-09-18 12:20_

@dhruvmanila yes, thanks! It probably needs to be added to `--help` then:

```
>ruff --version                             
ruff 0.0.290

>ruff check --help | wsl grep preview       
      --preview
          Enable preview mode; checks will include unstable rules and fixes
```

---

_Renamed from "Feature request: be able to disable `preview`" to "Documentation: add `--no-preview` to `--help`" by @bersbersbers on 2023-09-18 12:29_

---

_Label `documentation` added by @MichaReiser on 2023-09-18 13:01_

---

_Closed by @charliermarsh on 2023-09-19 12:27_

---

_Label `question` removed by @dhruvmanila on 2023-09-19 13:22_

---
