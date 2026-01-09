---
number: 4666
title: "Consider omitting `(*)` explanation from `uv pip tree`?"
type: issue
state: open
author: charliermarsh
labels:
  - needs-decision
  - cli
assignees: []
created_at: 2024-06-30T20:14:56Z
updated_at: 2024-07-04T15:53:06Z
url: https://github.com/astral-sh/uv/issues/4666
synced_at: 2026-01-07T13:12:17-06:00
---

# Consider omitting `(*)` explanation from `uv pip tree`?

---

_Issue opened by @charliermarsh on 2024-06-30 20:14_

Is this totally necessary? Seems like a lot to include it on every invocation (Cargo does not).

---

_Label `cli` added by @charliermarsh on 2024-06-30 20:15_

---

_Comment by @ibraheemdev on 2024-06-30 21:43_

I personally don't see the need to include it as long as we have easy-to-find documentation. cc @zanieb.

---

_Comment by @zanieb on 2024-06-30 21:59_

I had no clue what it meant — I was confused that all these packages were being marked. Idk.

---

_Comment by @potiuk on 2024-07-01 16:43_

I think it makes sense to add **some** indication in the tree output when there are some transitive dependencies in the package and we do not show them. Nut it makes no sense to do when there are no hidden transitive dependencies. 

If you do it this way (for example by adding `(*)` or `...` (which would by more pythonic IMHO), then suddenly there will be a lot less of those in the output - and only where it would actually indicate that the tree node is not complete. 

Example (from Airflow):

It makes sense that requests down there has some indication that it's not a complete set of dependencies - and that you should look it up above to find out what those transitive dependencies are.

```
├── requests v2.31.0
│   ├── charset-normalizer v3.3.2
│   ├── idna v3.7
│   ├── urllib3 v1.26.19
│   └── certifi v2024.6.2
├── pyjwt v2.8.0
├── typing-extensions v4.12.2
├── urllib3 v1.26.19 (*)
└── deprecated v1.2.14
    └── wrapt v1.16.0
pyhive v0.7.0
├── future v1.0.0
└── python-dateutil v2.9.0.post0
    └── six v1.16.0
adlfs v2024.4.1
├── azure-core v1.30.2
│   ├── requests v2.31.0 (*)
```

But here it makes completely no sense to add (*) for the second markupsafe occurence - because the tree is actually complete:

```
flask-bcrypt v1.0.1
├── flask v2.2.5
│   ├── werkzeug v2.2.3
│   │   └── markupsafe v2.1.5
│   ├── jinja2 v3.1.4
│   │   └── markupsafe v2.1.5 (*)
│   ├── itsdangerous v2.2.0
│   ├── click v8.1.7
│   └── importlib-metadata v6.11.0
│       └── zipp v3.19.2
```


---

_Comment by @charliermarsh on 2024-07-01 16:46_

@potiuk - I believe that's fixed on `main` (omitting `*` for packages with no dependencies).

---

_Comment by @zanieb on 2024-07-01 16:51_

Also note the commentary here is about the footnote explaining of the _meaning_ of `*` not including it at all.

---

_Comment by @potiuk on 2024-07-01 20:08_

> @potiuk - I believe that's fixed on main (omitting * for packages with no dependencies).

Ups. you are moving too fast ... I only saw that while catchng up with all the comments :)

---

_Comment by @potiuk on 2024-07-01 20:09_

> Also note the commentary here is about the footnote explaining of the _meaning_ of `*` not including it at all.

Understood. Yeah. That makes perfect sense.

---

_Label `needs-decision` added by @zanieb on 2024-07-04 15:53_

---
