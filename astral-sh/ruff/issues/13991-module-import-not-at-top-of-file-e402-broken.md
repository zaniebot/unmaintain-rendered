---
number: 13991
title: "module-import-not-at-top-of-file (E402): Broken because of exceptional handling"
type: issue
state: closed
author: buhtz
labels:
  - question
assignees: []
created_at: 2024-10-30T11:04:18Z
updated_at: 2024-10-30T12:48:47Z
url: https://github.com/astral-sh/ruff/issues/13991
synced_at: 2026-01-10T01:22:54Z
---

# module-import-not-at-top-of-file (E402): Broken because of exceptional handling

---

_Issue opened by @buhtz on 2024-10-30 11:04_

The rule "[module-import-not-at-top-of-file (E402)](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/#module-import-not-at-top-of-file-e402)" was derived from `pycodestyle`. But compared to the original pycodestyle-e402, ruff's behavior has an exception:

> This rule makes an exception for both sys.path modifications (allowing for sys.path.insert, sys.path.append, etc.) and os.environ modifications between imports.

This causing problems in projects using several linters in a chain. In my example I do use `flake8` and `ruff`.

    import sys
    from pathlib import Path
    sys.path.append(os.path.join(os.path.dirname(__file__), '..'))
    import mymodule  # <-- E402 module level import not at top of file

Ruff is fine with that code because of its exceptional behavior described in the docs. But flake8 does warn about it.
I do ignore this line for flake8 with `# noqa: E402`.
But in this case Ruff warns with `RUF100 [*] Unused `noqa` directive (unused: `E402`)`.

My current workaround is this

        import mymodule  # noqa: E402, RUF100

Don't get me wrong I love Ruff and also think this is a valuable project in the FOSS community.
To my understand this example illustrates a general problem in the project. Ruff does break common standards. There are other differences compared to other linters, e.g. how Ruff count branches.

There is not problem in doing it but a problem in how it is done. I would suggest to implement E402 the same way pycodestyle does it. Additionally add a switch or something like that to modify its behavior. Maybe make the Ruff-like-E402 the default behavior but mention that clearly in the docs and state how to revert that back to the original pycodestyle-behavior, e.g. with `--e402-org` or something like this.

---

_Comment by @AlexWaygood on 2024-10-30 12:35_

Thanks for the report, and I'm sorry for the frustrating situation for you here.

In general, we do try to maintain some degree of compatibility with other tools that implement similar rules. For example, we'll often change our error codes to match another linter if our rule is a reimplementation of a pre-existing rule provided by the other linter, and the other linter changes the error code associated with the rule.

However, I don't think 100% compatibility with other tools is or should be a goal of the project, unfortunately. Here I would argue that we have implemented an extra feature -- some special handling of `sys.path` modifications -- that pycodestyle has not implemented. All else being equal, I think our behaviour is preferable here, because it reduces the possibility of false positives and incorporates a more sophisticated understanding of Python semantics into the rule. This doesn't mean that the pycodestyle behaviour is incorrect or bad, or that they should change the way their rule operates; I can well imagine that trying to incorporate the same logic into pycodestyle would be an unreasonable increase in complexity for that project. But with regards to your suggestion that we also implement a "pycodestyle-compatible mode", I'm not sure we should increase the complexity of our implementation and configuration purely for compatibility here.

Ultimately there isn't really a "standard" when it comes to Python linting. For example, there are many different ways of measuring complexity of a Python function, and there are many different tools that will attempt to lint your code using one or more of these methods. The different tools are often likely to come to different conclusions; which (if any) you find valuable is a subjective decision.

We try to have roughly similar semantics to pre-existing tools to ease adoption and to reduce fragmentation in the ecosystem, but 100% parity such that you can run Ruff at the same time as other linters is a non-goal. Ultimately I would say that you unfortunately have to choose between Ruff and pycodestyle here, and switch off either the Ruff version of this rule or the pycodestyle version of this rule.

---

_Comment by @MichaReiser on 2024-10-30 12:48_

I haven't tested it but you could consider adding `E402` to [`lint.external`](https://docs.astral.sh/ruff/settings/#lint_external) to forbid ruff from removing any `E402` noqa comments. It does have the downside that Ruff no longer flags any unused `E402` but maybe you're fine with that in case flake8 warns about unused noqa directives.

---

_Label `question` added by @MichaReiser on 2024-10-30 12:48_

---

_Comment by @buhtz on 2024-10-30 12:48_

Thank you for your reply. I do get your point.

> argue that we have implemented an extra feature -- some special handling of `sys.path` modifications -- that pycodestyle has not implemented. All else being equal, I think our behaviour is preferable here, because it reduces the possibility of false positives 

In the specific case of sys.path I would disagree. Modifying sys.path in context of import is, with modern Python, always hacking. It is bad habit and an indicator of problems with packaging and project configuration. Despite that it is a common and often used way to workaround existing problems. Forcing newcomers not to modify sys.path would force them to learn about the better way. Allowing this "bad habit" while linting by default indicates to them it is a good pythonic way, which it isn't.

---

_Closed by @buhtz on 2024-10-30 12:48_

---
