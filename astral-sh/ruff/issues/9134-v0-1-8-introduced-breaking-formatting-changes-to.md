```yaml
number: 9134
title: "v0.1.8 introduced breaking formatting changes to my `# noqa` pragma comment"
type: issue
state: closed
author: tylerlaprade
labels: []
assignees: []
created_at: 2023-12-14T18:06:03Z
updated_at: 2024-01-05T04:39:45Z
url: https://github.com/astral-sh/ruff/issues/9134
synced_at: 2026-01-12T15:54:48Z
```

# v0.1.8 introduced breaking formatting changes to my `# noqa` pragma comment

---

_@tylerlaprade_

This formatting was stable in v0.1.7:
```
def update_or_create_patients_and_sites_from_edc(instance: 'EdcPatientRecordObjectMixin'):  # noqa: C901,PLR0912 -- # ! Disabled for emergency bugfix.
```

Under v.0.1.8, it gets changed to:
```
def update_or_create_patients_and_sites_from_edc(
    instance: 'EdcPatientRecordObjectMixin',
):  # noqa: C901,PLR0912 -- # ! Disabled for emergency bugfix.
```

Instead, it should be:
```
def update_or_create_patients_and_sites_from_edc(  # noqa: C901,PLR0912 -- # ! Disabled for emergency bugfix.
    instance: 'EdcPatientRecordObjectMixin',
):
```

which is also stable for formatting, but keeps the `# noqa` on the correct line.
We used to have this problem all the time with Black, but so far Ruff has been much better in how it handles pragmas.

---

_Comment by @charliermarsh on 2023-12-14 18:09_

What is your configured line length?

---

_Comment by @tylerlaprade on 2023-12-14 18:13_

I haven't set one. Here are the possibly relevant parts of my config:
```
[format]
skip-magic-trailing-comma = true
quote-style = "single"
docstring-code-format = true

[pycodestyle]
ignore-overlong-task-comments = true
max-doc-length = 150
```

---

_Comment by @charliermarsh on 2023-12-14 18:14_

At the default, it's correct to wrap the lines there, so I'm surprised that it wasn't wrapping at all in v0.1.7. Am I correct that you're saying the first snippet was unchanged in v0.1.7?

(I know the issue is about the comment.)


---

_Comment by @tylerlaprade on 2023-12-14 18:17_

Yes, it was not reformatting in v0.1.7

---

_Comment by @zanieb on 2023-12-14 18:18_

This reformats for me on v0.1.7

```
❯ ruff version
ruff 0.1.7 (8d9912a83 2023-12-04)
❯ ruff format --diff example.py
--- example.py
+++ example.py
@@ -1,2 +1,4 @@
-def update_or_create_patients_and_sites_from_edc(instance: 'EdcPatientRecordObjectMixin'):  # noqa: C901,PLR0912 -- # ! Disabled for emergency bugfix.
+def update_or_create_patients_and_sites_from_edc(
+    instance: "EdcPatientRecordObjectMixin",
+):  # noqa: C901,PLR0912 -- # ! Disabled for emergency bugfix.
     pass

1 file would be reformatted
```

---

_Comment by @tylerlaprade on 2023-12-14 18:21_

Ah, I see what happened here - v0.1.8 added the quotes to the type annotation, which must have pushed it over the default line length

---

_Comment by @charliermarsh on 2024-01-05 04:39_

I think this is not specific to a change in v0.1.8, and more a general issue with the pragma moving.

---

_Closed by @charliermarsh on 2024-01-05 04:39_

---
