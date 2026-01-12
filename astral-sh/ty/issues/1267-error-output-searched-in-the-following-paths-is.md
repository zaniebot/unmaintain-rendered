```yaml
number: 1267
title: "Error output \"Searched in the following paths...\" is very long when using bazel"
type: issue
state: closed
author: jasonprado
labels:
  - diagnostics
assignees: []
created_at: 2025-09-27T01:31:00Z
updated_at: 2025-10-16T11:18:10Z
url: https://github.com/astral-sh/ty/issues/1267
synced_at: 2026-01-12T15:54:24Z
```

# Error output "Searched in the following paths..." is very long when using bazel

---

_@jasonprado_

### Summary

My bazel monorepo has hundreds of python dependencies and when a missing module import is detected the error output grows very, very long. Example:

```
error[unresolved-import]: Cannot resolve imported module `deploy.cdk_stack`
  --> services/some_app/some_file.py:10:6
   |
10 | from does.not.exist import SomeClass
   |      ^^^^^^^^^^^^^^^^
   |
info: Searched in the following paths during module resolution:
info:   1. /my-repo (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. /my-reppo/.venv/lib/python3.12/site-packages (site-packages)
info:   4. /home/my-user/.cache/bazel/_bazel_my-user/0b37e5958ef8909453aa6cc50b6ee420/execroot/_main/bazel-out/aarch64-fastbuild/bin/build_tools/python_tools/venv.runfiles (editable install)
...
[800 more bazel "editable install" locations]
```

This is a bit burdensome but it sure is explicit. I'm not sure the behavior I want; maybe a more verbose or less verbose mode? I think outputting all 800 paths is probably not valuable. Collapsing similar ones then giving me an invocation I could run to get the full list would be nice. And ty is so fast I wouldn't mind running it again immediately.

### Version

ty 0.0.1-alpha.21

---

_Comment by @carljm on 2025-09-27 01:50_

Yeah, I don't think we were considering such environments when we added that diagnostic info 😆 Thanks for the report!

We should probably only output the full list in normal-verbosity if it's below a certain length; otherwise we should elide the list and suggest increasing the verbosity if you want the full list?

---

_Label `diagnostics` added by @carljm on 2025-09-27 01:51_

---

_Comment by @MichaReiser on 2025-09-27 07:45_

Uff yeah, this doesn't sound great. Truncating the list and showing more context when `-v` seems reasonable. 

---

_Added to milestone `GA` by @MichaReiser on 2025-09-27 07:45_

---

_Closed by @MichaReiser on 2025-10-16 11:18_

---
