```yaml
number: 9886
title: Confusing Python requirement statement in resolver error
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-12-13T21:11:16Z
updated_at: 2024-12-13T22:32:10Z
url: https://github.com/astral-sh/uv/issues/9886
synced_at: 2026-01-12T16:00:01Z
```

# Confusing Python requirement statement in resolver error

---

_@zanieb_

The message `Because the requested Python version (>=3.8.0) does not satisfy Python>=3.10 and the requested Python version (>=3.8.0) does not satisfy Python>=3.9,<3.10, we can conclude that Python>=3.9 is incompatible.` is basically incomprehensible.

```
❯ echo "numpy>2" | uv pip compile -p 3.8 -
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.8.0) does not satisfy Python>=3.10 and the requested Python version (>=3.8.0) does not
      satisfy Python>=3.9,<3.10, we can conclude that Python>=3.9 is incompatible.
      And because numpy>=2.0.1,<=2.0.2 depends on Python>=3.9 and only the following versions of numpy are available:
          numpy<=2.0.2
          numpy>=2.1.0,<=2.1.3
          numpy>=2.2.0
      we can conclude that all of:
          numpy>2,<2.1.0
          numpy>2.1.3,<2.2.0
       cannot be used. (1)

      Because the requested Python version (>=3.8.0) does not satisfy Python>=3.10 and all of:
          numpy>=2.1.0,<=2.1.3
          numpy>=2.2.0
      depend on Python>=3.10, we can conclude that all of:
          numpy>=2.1.0,<=2.1.3
          numpy>=2.2.0
       cannot be used.
      And because we know from (1) that all of:
          numpy>2,<2.1.0
          numpy>2.1.3,<2.2.0
       cannot be used, we can conclude that numpy>2 cannot be used.
      And because you require numpy>2, we can conclude that your requirements are unsatisfiable.

      hint: Pre-releases are available for `numpy` in the requested range (e.g., 2.2.0rc1), but pre-releases weren't enabled (try:
      `--prerelease=allow`)

      hint: The `--python-version` value (>=3.8.0) includes Python versions that are not supported by your dependencies (e.g.,
      numpy>=2.0.1,<=2.0.2 only supports >=3.9). Consider using a higher `--python-version` value.
```

---

_Label `error messages` added by @zanieb on 2024-12-13 21:11_

---

_Comment by @zanieb on 2024-12-13 22:32_

The formatting is implemented at https://github.com/astral-sh/uv/blob/b751648bfef7712a53c9dc3bfdaaff084c01e76d/crates/uv-resolver/src/pubgrub/report.rs#L51-L70

---
