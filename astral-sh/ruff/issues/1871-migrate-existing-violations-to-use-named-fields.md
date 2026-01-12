```yaml
number: 1871
title: Migrate existing violations to use named fields
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - internal
assignees: []
created_at: 2023-01-14T16:48:46Z
updated_at: 2023-01-29T18:30:22Z
url: https://github.com/astral-sh/ruff/issues/1871
synced_at: 2026-01-12T15:54:41Z
```

# Migrate existing violations to use named fields

---

_@charliermarsh_

Purely a mechanical refactor, but right now all the structs use anonymous fields.

Check out #1887 for an example of the kind of conversion we want to apply, to all structs in `violations.rs`.

---

_Label `internal` added by @charliermarsh on 2023-01-14 16:48_

---

_Label `good first issue` added by @charliermarsh on 2023-01-15 06:49_

---

_Comment by @sladyn98 on 2023-01-18 20:25_

@charliermarsh I read your tweet and would like to get started with contributing to this repository, is this issue still up for grabs. If yes I would like to give this a shot.


---

_Comment by @charliermarsh on 2023-01-18 21:22_

@sladyn98 - That'd be great! Feel free to do it in pieces if you'd like, a few structs or even one-at-a-time, whatever you prefer.

---

_Comment by @sladyn98 on 2023-01-23 21:33_

@charliermarsh For example for this struct if I want to make changes to this one do i have to make changes to another file that defines this struct,  like you did in this example https://github.com/charliermarsh/ruff/pull/1887

```
    pub struct UnusedImport(pub String, pub bool, pub bool);
```

---

_Comment by @charliermarsh on 2023-01-23 21:44_

Yeah, in that case, you'd need to change `pub struct UnusedImport(pub String, pub bool, pub bool)` in `violations.rs`, and then the `violations::UnusedImport(full_name.to_string(), ignore_init, multiple)` references in `src/checkers.rs`.

---

_Comment by @sladyn98 on 2023-01-24 19:44_

Thanks Created a PR

---

_Comment by @charliermarsh on 2023-01-29 18:30_

Done by #2317!

---

_Closed by @charliermarsh on 2023-01-29 18:30_

---
