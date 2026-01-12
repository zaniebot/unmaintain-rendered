```yaml
number: 4037
title: "Bug: wrong ordering with `@commitID` and `#subdirectory` after `uv pip compile`"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
assignees: []
created_at: 2024-06-05T03:17:49Z
updated_at: 2024-06-05T03:53:24Z
url: https://github.com/astral-sh/uv/issues/4037
synced_at: 2026-01-12T15:58:47Z
```

# Bug: wrong ordering with `@commitID` and `#subdirectory` after `uv pip compile`

---

_@jamesbraza_

We have a package installed from GitHub private repo in a `requirements.in`:

```none
foo @ git+ssh://git@github.com/org/foo.git
```

This `foo` package depends on another GitHub private repo (that is not listed in `requirements.in`) called `spam`. Coincidentally, `spam` is in a subdirectory, so you have to specify `#subdirectory=spam`.

Running `uv pip compile requirements.in -o requirements.txt` with `uv==0.2.5`, it outputs this:

```none
foo @ git+ssh://git@github.com/org/foo.git@abc123
    # via -r requirements.in
spam @ git+ssh://git@github.com/org/spam.git#subdirectory=spam@def456
    # via foo
```

This _almost_ was perfect, except for the fact that `spam @ git+ssh://git@github.com/org/spam.git#subdirectory=spam@def456` is not valid, per the ordering of `#subdirectory` and `@commitID`.

Thus, the bug is that `#subdirectory=spam` needs to come after `@def445`.

Reference: https://pip.pypa.io/en/stable/topics/vcs-support/#git

---

_Comment by @charliermarsh on 2024-06-05 03:29_

Thanks.

---

_Label `bug` added by @charliermarsh on 2024-06-05 03:29_

---

_Comment by @charliermarsh on 2024-06-05 03:36_

This looks like a bug in `apply_redirect`.

---

_Comment by @charliermarsh on 2024-06-05 03:37_

I will fix tomorrow, thanks.

---

_Closed by @charliermarsh on 2024-06-05 03:53_

---
