```yaml
number: 16610
title: "Ruff assuming python v 3.9 - syntax error on 'match'"
type: issue
state: closed
author: leonlonsdale
labels:
  - question
assignees: []
created_at: 2025-03-10T21:16:37Z
updated_at: 2025-03-11T09:59:27Z
url: https://github.com/astral-sh/ruff/issues/16610
synced_at: 2026-01-12T15:54:55Z
```

# Ruff assuming python v 3.9 - syntax error on 'match'

---

_@leonlonsdale_

### Summary

Using Ruff v 0.9.10 and Python 3.13.2 I am getting the following Syntax error message:

<img width="837" alt="Image" src="https://github.com/user-attachments/assets/9ff4c00c-2552-43aa-902b-fdaa6d80a8a1" />

Removing "--preview" from args appears to fix it. I note a previously closed issue suggests that this is fixed, so just wanted to highlight it still appears to exist. My bad if the fix was not included in the latest release 3 days ago.

<img width="415" alt="Image" src="https://github.com/user-attachments/assets/009a498c-b9c5-4ecd-8b24-661365caead4" />

### Version

0.9.10

---

_Closed by @AlexWaygood on 2025-03-10 21:56_

---

_Comment by @dhruvmanila on 2025-03-11 06:27_

If you don't mind, I'm curious to know why you're passing in the `--preview` flag to the `server` subcommand? If you're intending to enable preview mode for Ruff, you can provide it in the server options for the [linter](https://docs.astral.sh/ruff/editors/settings/#lint_preview) and the [formatter](https://docs.astral.sh/ruff/editors/settings/#format_preview). Related #16388

---

_Reopened by @MichaReiser on 2025-03-11 06:53_

---

_Comment by @dhruvmanila on 2025-03-11 07:17_

> Using Ruff v 0.9.10 and Python 3.13.2

Is the Python version configured in the Ruff config? Like, using either `requires-python` or [`target-version`](https://docs.astral.sh/ruff/settings/#target-version).

---

_Comment by @leonlonsdale on 2025-03-11 08:40_

> > Using Ruff v 0.9.10 and Python 3.13.2
> 
> Is the Python version configured in the Ruff config? Like, using either `requires-python` or [`target-version`](https://docs.astral.sh/ruff/settings/#target-version).

No this is not configured. I use a central config file in ~/.config/ruff. You can view it here: https://github.com/ionztorm/dotfiles/blob/main/ruff/ruff.toml

Note: This issue has only started since updating to Ruff 0.9.10.

> If you don't mind, I'm curious to know why you're passing in the `--preview` flag to the `server` subcommand? If you're intending to enable preview mode for Ruff, you can provide it in the server options for the [linter](https://docs.astral.sh/ruff/editors/settings/#lint_preview) and the [formatter](https://docs.astral.sh/ruff/editors/settings/#format_preview). Related [#16388](https://github.com/astral-sh/ruff/issues/16388)

The Helix Editor config is one I found on reddit, and appeared to be working so I made no changes.

---

_Comment by @MichaReiser on 2025-03-11 08:55_

Ruff defaults to Python 3.9 if you don't specify an explicit version in your configuration. Can you try setting `target-version = "py312"` in your configuration?

See https://github.com/astral-sh/ruff/issues/16417 for more details

---

_Comment by @leonlonsdale on 2025-03-11 08:59_

> Ruff defaults to Python 3.9 if you don't specify an explicit version in your configuration. Can you try setting `target-version = "py312"` in your configuration?
> 
> See [#16417](https://github.com/astral-sh/ruff/issues/16417) for more details

Sure, is there any particular area this needs to be put?

---

_Comment by @leonlonsdale on 2025-03-11 09:02_

This seems to have worked, thank you. I assume this should be set in a local config on a per-project basis for projects using different versions of python?

---

_Comment by @dhruvmanila on 2025-03-11 09:28_

> The Helix Editor config is one I found on reddit, and appeared to be working so I made no changes.

We have a setup guide for Helix: https://docs.astral.sh/ruff/editors/setup/#helix :)

---

_Label `question` added by @dhruvmanila on 2025-03-11 09:28_

---

_Comment by @MichaReiser on 2025-03-11 09:38_

> This seems to have worked, thank you. I assume this should be set in a local config on a per-project basis for projects using different versions of python?

Yes, it's best to set it at the project level if they use different python versions. 

---

_Comment by @leonlonsdale on 2025-03-11 09:57_

> > This seems to have worked, thank you. I assume this should be set in a local config on a per-project basis for projects using different versions of python?
> 
> Yes, it's best to set it at the project level if they use different python versions.

Awesome, thanks for the help

---

_Comment by @MichaReiser on 2025-03-11 09:59_

You're welcome. 

---

_Closed by @MichaReiser on 2025-03-11 09:59_

---
