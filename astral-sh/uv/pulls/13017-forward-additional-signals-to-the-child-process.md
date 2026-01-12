```yaml
number: 13017
title: "Forward additional signals to the child process in `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/all-sigs
created_at: 2025-04-21T15:08:37Z
updated_at: 2025-04-21T19:47:33Z
url: https://github.com/astral-sh/uv/pull/13017
synced_at: 2026-01-12T16:10:30Z
```

# Forward additional signals to the child process in `uv run`

---

_@zanieb_

As I suspected quite some time ago (https://github.com/astral-sh/uv/pull/6738#issuecomment-2315466033), it's problematic that we don't handle _every_ signal here. This PR adds handling for all of the Unix signals except `SIGCHLD`, `SIGIO`, and `SIGPOLL` which seem incorrect to forward. Also notable, we _cannot_ handle `SIGKILL` so if someone sends that to the PID instead of the PGID, they will leave dangling subprocesses.

Instead, we could use `exec` and avoid this handling. However, we'd lose the ability to add nice error message on failure (e.g., as someone is trying to add in https://github.com/astral-sh/uv/pull/12201) and, more critically, we'd need to figure out how to clean up resources properly (i.e., temporary directories) which currently happens on `Drop`. In the long-term, we'll probably want an option to use `exec` — but we'll need to figure out when to clean up resources or accept that they will dangle. This was last discussed in https://github.com/astral-sh/uv/issues/3095 — discussion on that approach should continue there.

A note on the implementation: I spent time time trying to write the handler using a tokio stream, so we could dynamically iterate over a list of signals instead of copy/pasting the implementation — I couldn't get it to work though and it didn't seem critical.

Closes https://github.com/astral-sh/uv/issues/12830

---

_Label `bug` added by @zanieb on 2025-04-21 15:51_

---

_Marked ready for review by @zanieb on 2025-04-21 15:51_

---

_Review requested from @charliermarsh by @zanieb on 2025-04-21 17:44_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/run.rs`:97 on 2025-04-21 19:32_

Nit: "can be have"

---

_@charliermarsh reviewed on 2025-04-21 19:32_

---

_@charliermarsh approved on 2025-04-21 19:34_

Nice, thanks.

---

_@zanieb reviewed on 2025-04-21 19:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:97 on 2025-04-21 19:34_

```suggestion
        // The following signals are terminal by default, but can have user defined handlers.
```

---

_Closed by @zanieb on 2025-04-21 19:36_

---

_Reopened by @zanieb on 2025-04-21 19:36_

---

_Comment by @zanieb on 2025-04-21 19:36_

GitHub actions so annoying lately.

---

_Merged by @zanieb on 2025-04-21 19:47_

---

_Closed by @zanieb on 2025-04-21 19:47_

---

_Branch deleted on 2025-04-21 19:47_

---
