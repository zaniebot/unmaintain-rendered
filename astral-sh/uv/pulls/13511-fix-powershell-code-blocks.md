```yaml
number: 13511
title: Fix PowerShell code blocks
type: pull_request
state: merged
author: h4iku
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-codeblocks
created_at: 2025-05-17T21:56:42Z
updated_at: 2025-05-18T10:09:19Z
url: https://github.com/astral-sh/uv/pull/13511
synced_at: 2026-01-10T11:10:41Z
```

# Fix PowerShell code blocks

---

_Pull request opened by @h4iku on 2025-05-17 21:56_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The PowerShell prompt is not `$`, so it is not detected as a `Generic.Prompt` token by Pygments lexers. Therefore, the JavaScript code does not strip the prompt when copying from PowerShell code blocks, such as [here](https://docs.astral.sh/uv/getting-started/installation/#__tabbed_5_2).

Other places in the docs have removed the prompt completely to address this issue:
* https://docs.astral.sh/uv/guides/projects/#__tabbed_1_2
* https://docs.astral.sh/uv/guides/integration/jupyter/#__tabbed_1_2

This PR updates the PowerShell prompt to `PS>` and changes the code fence language to `pwsh-session` to match the lexer name from [Pygments](https://pygments.org/docs/lexers/#pygments.lexers.shell.PowerShellSessionLexer). This allows the prompt to be correctly detected as a `Generic.Prompt` token and is stripped during copy.

Related: https://github.com/astral-sh/uv/pull/12520



---

_Comment by @zanieb on 2025-05-18 02:05_

Oh, cool! Thanks for the explanation.

---

_Label `documentation` added by @zanieb on 2025-05-18 02:06_

---

_Merged by @zanieb on 2025-05-18 02:06_

---

_Closed by @zanieb on 2025-05-18 02:06_

---

_Branch deleted on 2025-05-18 10:09_

---
