```yaml
number: 8118
title: "Render Markdown output from `ruff rule`"
type: pull_request
state: closed
author: cjolowicz
labels: []
assignees: []
base: main
head: pretty-rule
created_at: 2023-10-22T14:31:06Z
updated_at: 2024-03-13T06:35:15Z
url: https://github.com/astral-sh/ruff/pull/8118
synced_at: 2026-01-10T22:47:01Z
```

# Render Markdown output from `ruff rule`

---

_Pull request opened by @cjolowicz on 2023-10-22 14:31_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

See https://github.com/astral-sh/ruff/issues/2938#issuecomment-1509755352

This is a partial revert of #2959 with adaptations for the new API.

## Test Plan

<!-- How was it tested? -->

```console
$ ruff rule B006
$ ruff rule --all
```

<img width="1158" alt="Screenshot 2023-10-23 at 01 53 30" src="https://github.com/astral-sh/ruff/assets/653941/53d882a5-0b39-4539-b483-d8e31fff720d">


---

_Comment by @cjolowicz on 2023-10-22 17:51_

Needs rerun of snapshot in 80x25 terminal/no tty I think... 

update: fixed

---

_Comment by @github-actions[bot] on 2023-10-23 00:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Comment by @cjolowicz on 2023-11-22 07:11_

Just a thought: An alternative approach would be to allow users to configure an external Markdown viewer in _pyproject.toml_ or _ruff.toml_. Something like this:

```toml
[tool.ruff]
markdown-viewer = "mdcat"
#markdown-viewer = "mdcat --columns=72 --paginate"
#markdown-viewer = "rich --markdown --width=72 -"
#markdown-viewer = "bat --plain --language=md"
```

Pros:
- any viewer you like
- no dependencies on Ruff
- users can pass options

Cons:
- user has to install the tool
- user has to configure Ruff, default output would stay raw Markdown

---

_Comment by @MichaReiser on 2023-11-27 07:44_

What syntax does pulldown markdown support? I've run into issues where our website didn't support the markdown used. It would get more challenging if pulldown markdown adds further restrictions.

A less heavy approach would be to explicitly define which markdown syntax ruff supports and implement parsing and rendering for it manually. I believe we only use bold, code blocks, maybe italic, and links.

---

_Comment by @zanieb on 2024-03-12 18:32_

It looks like there hasn't been any activity on this in a while. It seems like we need to have more discussion about the design before we can accept a pull request. Thank you!

---

_Closed by @zanieb on 2024-03-12 18:32_

---

_Branch deleted on 2024-03-13 06:35_

---
