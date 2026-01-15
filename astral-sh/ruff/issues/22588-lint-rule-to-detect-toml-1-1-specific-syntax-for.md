```yaml
number: 22588
title: "Lint rule to detect TOML 1.1 specific syntax for `pyproject.toml`, `pylock.toml`, or any other Python packaging TOML file"
type: issue
state: open
author: notatallshaw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2026-01-14T23:51:12Z
updated_at: 2026-01-15T13:21:39Z
url: https://github.com/astral-sh/ruff/issues/22588
synced_at: 2026-01-15T13:51:54Z
```

# Lint rule to detect TOML 1.1 specific syntax for `pyproject.toml`, `pylock.toml`, or any other Python packaging TOML file

---

_@notatallshaw_

### Summary

There is an extensive discussion on DPO packaging about tools adopting TOML 1.1: https://discuss.python.org/t/adopting-toml-1-1/105624

A concern by some is that as some tools become permissive about TOML 1.1 specific syntax that users will write TOML 1.1 specific syntax intentionally, or otherwise, and older tools or tools that choose not to be permissive about TOML 1.1 specific syntax will throw an error.

If enacted I also propose this rule would essentially be deprecated once Python 3.15 has reached end of life (around October 2031).

---

_Label `rule` added by @amyreese on 2026-01-15 00:54_

---

_Label `needs-decision` added by @amyreese on 2026-01-15 00:54_

---

_Comment by @amyreese on 2026-01-15 00:55_

I like the idea, and this could be a good complement to RUF200.  I think the main concern is the list of files to check, because I think ruff only looks at `pyproject.toml` right now, and whether there's a good set of libraries and or mechanisms for detecting new toml features like this that won't require owning a toml implementation in ruff.

---

_Comment by @MichaReiser on 2026-01-15 07:59_

Is the concernthat the pyproject.toml isn't backwards compatible when publishing to pypi or that local tools might start failing or is it something else?

If it's the latter, this won't be a rule we can enable by default, because it's overly pedantic if you don't use any such tool (and then it is unnecessarily restrictive). 

---

_Comment by @notatallshaw on 2026-01-15 13:19_

Reading a `pyproject.toml` is needed for installing sdists (e.g. from pypi), archived source files, local building, local running of tools that store their configuration there, and by remote static analysis tools such as GitHub's dependency services (e.g. dependabot and security alerts).

So the answer is for a  `pyproject.toml` that uses TOML 1.1 specific syntax affect all 3 situations, PyPI installation, local building, and other situations. 

That said, I didn't imagine the rule would be on by default unless there was real world reports of users having issues with TOML 1.1 specific syntax. I assumed it would be advocated by proponents as good practice, and users would opt in. 

---
