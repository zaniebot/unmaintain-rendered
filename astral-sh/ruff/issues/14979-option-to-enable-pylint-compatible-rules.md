```yaml
number: 14979
title: Option to enable Pylint-compatible rules?
type: issue
state: closed
author: pwschaedler
labels:
  - question
assignees: []
created_at: 2024-12-15T02:42:22Z
updated_at: 2024-12-25T08:15:28Z
url: https://github.com/astral-sh/ruff/issues/14979
synced_at: 2026-01-10T11:09:56Z
```

# Option to enable Pylint-compatible rules?

---

_Issue opened by @pwschaedler on 2024-12-15 02:42_

I currently use Pylint for my projects and would love to switch to Ruff. I understand [not all Pylint rules are implemented yet](https://github.com/astral-sh/ruff/issues/970), but my question is if it's possible to select all rules that are already implemented? Running `ruff check --select PL` will select the rules that are specific to Pylint, but it doesn't include all the other rules that are already covered by another tool (e.g. [N805](https://docs.astral.sh/ruff/rules/invalid-first-argument-name-for-method/) which matches Pylint's [E0213](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/no-self-argument.html)).

Is there currently a way to select all of the rules that match with Pylint? Or would it be possible to implement a way to select these, in order to provide a smoother migration path for users of other tools?

---

_Label `question` added by @MichaReiser on 2024-12-15 15:33_

---

_Comment by @MichaReiser on 2024-12-15 15:36_

> Is there currently a way to select all of the rules that match with Pylint?

I'm sorry. There's currently no such way. We could consider adding aliases for these rules but I'd have to double check if aliases work when selecting a linter-group instead of a specific rule. 

Our long-term goal is to recategorize our rules and to make the upstream linter groups less prominent (or remove them entirely). Instead, we want to focus on giving users an easy way to opt in to the rules relevant for their project. See #1774 

---

_Closed by @pwschaedler on 2024-12-16 00:20_

---

_Comment by @pwschaedler on 2024-12-25 08:15_

If anyone comes across this and wants it, I wrote a script to get a list of the current Pylint compatible rules from issue #970  and optionally add it to a `pyproject.toml`. Until a better solution becomes available! https://gist.github.com/pwschaedler/9de151297bb82e4598867b1e4a58a6b5

---
