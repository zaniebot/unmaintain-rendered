```yaml
number: 8842
title: "✨[Feature Request]: Filter uv install by Python Package Authority's Security Advisory Database.✨"
type: issue
state: open
author: galenseilis
labels: []
assignees: []
created_at: 2024-11-05T21:31:24Z
updated_at: 2024-11-05T21:39:13Z
url: https://github.com/astral-sh/uv/issues/8842
synced_at: 2026-01-10T04:36:20Z
```

# ✨[Feature Request]: Filter uv install by Python Package Authority's Security Advisory Database.✨

---

_Issue opened by @galenseilis on 2024-11-05 21:31_

# What's the problem this feature will solve?

It would make a higher-level of package security a default.

# Description

I would like uv to only download packages that do not have entries in the Python security advisory database by default.

When given a package name without a version I would like uv to filter out packages with security advisories as it searches for viable package versions. If none can be found, then stderr should indicate that.

If a package version is given, then the package should be installed if there is no security advisory, else give a stderr message.

An optional flag to override this behaviour would be appropriate for those that need to work with packages with security advisories, and for cybersecurity specialists they may need to be able to attempt downloading and installing such a package.

# Alternative Solutions

Tools like PDM, uv, Hatch, and Poetry all support plugins. I expect a plugin which does the security check first before attempting to add a package to a project would also work with these various tools (including uv, obviously).

Another partial solution is to use the pip-audit pre-commit hook or github action. These automation tools will not always catch that there is an advisory on a package before it is installed, however.

Manually checking pip-audit is "ok", but the manual nature of it makes it less reliable.

# Additional context

Security as a default is important for most organizations (e.g. healthcare), and I expect that most users won't mind it either.

I have made a [nearly-identical request](https://github.com/pypa/pip/issues/13066) for PIP to do the same thing. I tend to use pip a lot at work, and uv on personal projects. Security is important in my work (healthcare data science), so whichever supports it if the other doesn't is going to take priority in the future.

# End

Happy to discuss if the above needs clarification or revision. I'm also not highly familiar with the internal implementation details, so if this behaviour is already supported I would be grateful for direction to that feature. 

Thanks for considering my request! 🙏

---

_Comment by @galenseilis on 2024-11-05 21:39_

Looks like [PDM supports uv](https://pdm-project.org/en/latest/usage/uv/) experimentally so, this type of change could become an integral part of PDM if that support continues and matures.

---
