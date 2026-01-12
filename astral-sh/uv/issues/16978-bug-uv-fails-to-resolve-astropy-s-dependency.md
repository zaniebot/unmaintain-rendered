```yaml
number: 16978
title: "BUG(?): uv fails to resolve astropy's dependency graph in CI (circular dependency), but succeeds locally."
type: issue
state: closed
author: neutrinoceros
labels:
  - bug
assignees: []
created_at: 2025-12-04T14:23:13Z
updated_at: 2025-12-04T20:24:11Z
url: https://github.com/astral-sh/uv/issues/16978
synced_at: 2026-01-12T16:02:41Z
```

# BUG(?): uv fails to resolve astropy's dependency graph in CI (circular dependency), but succeeds locally.

---

_@neutrinoceros_

### Summary

uv is failing to resolve astropy's dependency graph in CI (but succeeds when run locally). Specifically, the resolver is choking on a cycle between `astropy[all]` and `asdf-astropy` (which are mutually dependent)

command
```
uv tree --resolution=lowest --universal --all-groups
```

output
```
Using CPython 3.14.0 interpreter at: /opt/hostedtoolcache/Python/3.14.0/x64/bin/python3
  × No solution found when resolving dependencies for split (markers:
  │ python_full_version < '3.12'):
  ╰─▶ Because asdf-astropy==0.9.0 depends on your project and only the
      following versions of asdf-astropy are available:
          asdf-astropy<=0.7.0
          asdf-astropy==0.7.1
          asdf-astropy==0.8.0
          asdf-astropy==0.9.0
      we can conclude that asdf-astropy>0.8.0 depends on your project.
      And because asdf-astropy>=0.7.0,<=0.8.0 depends on your project, we can
      conclude that asdf-astropy>=0.7.0 depends on your project.
      And because astropy[all] depends on asdf-astropy>=0.7.0 and your project
      requires astropy[all], we can conclude that your project's requirements
      are unsatisfiable.

      hint: The package `asdf-astropy` depends on the package `astropy` but
      the name is shadowed by your project. Consider changing the name of
      the project.

      hint: While the active Python version is 3.14, the resolution failed for
      other Python versions supported by your project. Consider limiting your
      project's supported Python versions using `requires-python`.
```

The error message seems either wrong (or maybe just confusing ?). In any case I don't understand the justification for failing resolution; dependency cycles have never been disallowed, and the same commands complete successfully when run locally (even with `--no-cache`, which to my knowledge bridges the only meaningful difference between these two contextes ?). I assume I must be missing something, but maybe that's a genuine bug ?

xref: https://github.com/astropy/astropy/pull/19022


### Platform

Ubuntu 24.04

### Version

uv 0.9.15 (pinned in the python script being run)

### Python version

3.14.0

---

_Label `bug` added by @neutrinoceros on 2025-12-04 14:23_

---

_Comment by @neutrinoceros on 2025-12-04 17:36_

nevermind, I figured it out using `RUST_LOG=uv=debug`; the issue was that I only used a shallow clone in CI, but astropy's version is dynamically computed at build time from the most recent git tag, and defaults to a `0.0.0+...` scheme, which is really what broke the resolver. To some extent, I think the error message could be improved to hint at it, but I'm not even sure that's a reasonable expectation. Anyway, feel free to close this as my immediate problem was both fixed *and* never was a bug in uv in the first place.

---

_Comment by @zanieb on 2025-12-04 18:58_

Hm the error message is indeed horrible. If you can construct a MRE in a new issue I'd be happy to investigate improving it, it seems a little too complicated here to be actionable.

I'm glad you figured out the problem!

---

_Closed by @zanieb on 2025-12-04 18:58_

---

_Comment by @neutrinoceros on 2025-12-04 19:14_

I'll see if I can manage an MRE in the next couple days. Is it okay if I ping you on the issue when I got it ?

---

_Comment by @zanieb on 2025-12-04 20:24_

Yep!

---
