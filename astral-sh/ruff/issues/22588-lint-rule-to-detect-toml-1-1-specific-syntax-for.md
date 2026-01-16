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
updated_at: 2026-01-16T15:37:38Z
url: https://github.com/astral-sh/ruff/issues/22588
synced_at: 2026-01-16T15:58:10Z
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

_Comment by @amyreese on 2026-01-15 20:44_

Yeah, the changes in TOML 1.1 allow multi-line table syntax that will fail to parse in TOML 1.0, eg:

```toml
[table]
value = {
  key = "value",
  key2 = "value",
}
```

which in TOML 1.0 would have to be written as:

```toml
[table]
value = {key = "value", key2 = "value"}

# OR

[table.value]
key = "value"
key2 = "value"
```

The problem is that if users start using the new syntax before all of their tools (including any that rely on tomllib from stdlib) support TOML 1.1 syntax, then that will break any incompatible tools. Since cpython is highly unlikely to backport TOML 1.1 support to existing releases, then use of 1.1 syntax would most likely need to wait until Python 3.14 is EOL.

---

_Comment by @dylwil3 on 2026-01-16 15:22_

I personally don't think this makes sense as a Ruff rule - it seems like we should let the error come from whichever tools do not support the new TOML syntax, rather than pre-empting them. That way users know which tools don't yet support the new syntax and can respond accordingly. Maybe they switch to a tool that does, or they adjust their syntax and file/upvote an issue asking for the tool to update, etc. 

---

_Comment by @MichaReiser on 2026-01-16 15:24_

> I personally don't think this makes sense as a Ruff rule - it seems like we should let the error come from whichever tools do not support the new TOML syntax, rather than pre-empting them. 

I agree with this sentiment if it comes to dev tools. I do think there's value in warning about TOML 1.1 syntax for packages published to pypi but only if there are cases where the installation would fail because of the TOML 1.1 syntax

On the other hand. It could be a useful opt-in rule for projects that know that they still are on 1.0 tools and can't upgrade.

---

_Comment by @notatallshaw on 2026-01-16 15:31_

> I personally don't think this makes sense as a Ruff rule - it seems like we should let the error come from whichever tools do not support the new TOML syntax, rather than pre-empting them.

The problem is it may work for the author of a package as they are using newer tools, and error for users of the package who are using other tools. E.g. author tests on Python 3.15+ only, and user tries to install on Python 3.14 and it throws an error. Or the author tests it using only uv but the user is installing with pip.

> That way users know which tools don't yet support the new syntax and can respond accordingly. Maybe they switch to a tool that does

Such users may not be able to update their tools due to their own support policies, 

>  they adjust their syntax and file/upvote an issue asking for the tool to update, etc.

In which case it would be helpful for the authors of the package to have a lint rule to prevent usage from happening in the first place...

> On the other hand. It could be a useful opt-in rule for projects that know that they still are on 1.0 tools and can't upgrade.

I am all for this rule being opt-in, I should have been clear in my original post, I am not suggesting this be made a default rule.

---
