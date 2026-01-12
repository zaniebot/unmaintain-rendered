```yaml
number: 1986
title: Add issue template for pip works, uv fails cases
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/pip-issue-templae
created_at: 2024-02-26T14:20:57Z
updated_at: 2024-04-04T16:25:55Z
url: https://github.com/astral-sh/uv/pull/1986
synced_at: 2026-01-12T16:04:49Z
```

# Add issue template for pip works, uv fails cases

---

_@konstin_

In cases where uv doesn't work on something where pip succeeds, it often took us some back-and-forth to get the relevant information. I want to improve this with this issue template, especially asking early about their python setup. Feel free to add fields you find relevant.

---

_Converted to draft by @konstin on 2024-02-26 14:21_

---

_Label `internal` added by @konstin on 2024-02-26 14:22_

---

_Marked ready for review by @konstin on 2024-02-28 12:59_

---

_@MichaReiser approved on 2024-02-28 13:03_

---

_Review comment by @AlexWaygood on `.github/ISSUE_TEMPLATE/pip-works--uv-doesn-t.md`:11 on 2024-03-01 11:07_

I feel like people who know that their Python setup is non-standard will probably already put that information in an issue without this prompt, and people who don't know that their Python setup is non-standard probably won't understand what's meant by the term "non-standard" here... Maybe it's better to just ask some concrete questions here? Suggestions:
- How did you install Python? (python.org, pyenv, Windows store, conda, Linux distro, etc.)
- Please give the output of running `python -VV` on the command line

---

_Review comment by @AlexWaygood on `.github/ISSUE_TEMPLATE/pip-works--uv-doesn-t.md`:13 on 2024-03-01 11:07_

```suggestion
<!-- Please provide a list of the requirements you attempted to install with uv (requirements.in or requirements.txt), ideally a minimal set that fails. -->
```

---

_Review comment by @AlexWaygood on `.github/ISSUE_TEMPLATE/pip-works--uv-doesn-t.md`:21 on 2024-03-01 11:08_

```suggestion
<!-- The working pip command. Please make sure you are using `--no-cache-dir` to disable using cached built wheels. You can link long output as a [gist](https://gist.github.com/). -->
```

---

_Review comment by @AlexWaygood on `.github/ISSUE_TEMPLATE/pip-works--uv-doesn-t.md`:28 on 2024-03-01 11:08_

```suggestion
<!-- The uv command you tried. Please make sure you are using `--no-cache`. -->
```

---

_@AlexWaygood reviewed on 2024-03-01 11:11_

Nice. We could also consider using an issue form (e.g. see [our templates over at CPython](https://github.com/python/cpython/issues/new?assignees=&labels=type-bug&projects=&template=bug.yml)), but a template is simpler to start off with

---

_@konstin reviewed on 2024-03-01 11:24_

---

_Review comment by @konstin on `.github/ISSUE_TEMPLATE/pip-works--uv-doesn-t.md`:11 on 2024-03-01 11:24_

Those should be covered by the last two lines, i've removed this and left it as a blanket "description" header

---

_@AlexWaygood reviewed on 2024-03-01 11:25_

---

_Review comment by @AlexWaygood on `.github/ISSUE_TEMPLATE/pip-works--uv-doesn-t.md`:11 on 2024-03-01 11:25_

makese sense!

---

_@AlexWaygood approved on 2024-03-01 11:25_

---

_Closed by @konstin on 2024-04-04 16:25_

---
