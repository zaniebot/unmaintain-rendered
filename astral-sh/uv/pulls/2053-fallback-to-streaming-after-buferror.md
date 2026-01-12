```yaml
number: 2053
title: "Fallback to streaming after `BufError`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - compatibility
  - needs-design
assignees: []
base: main
head: charlie/buf
created_at: 2024-02-28T20:34:47Z
updated_at: 2024-05-23T15:07:29Z
url: https://github.com/astral-sh/uv/pull/2053
synced_at: 2026-01-12T16:04:51Z
```

# Fallback to streaming after `BufError`

---

_@charliermarsh_

## Summary

Undecided on whether to merge this. Looking at the linked issue, something is going wrong with the client-server interaction, but I don't know what, who's at fault, or how to fix it. This change would merely fall back to _not_ performing range requests in this case.

See: https://github.com/astral-sh/uv/issues/1710.


---

_Label `compatibility` added by @charliermarsh on 2024-02-28 20:34_

---

_Label `needs-design` added by @charliermarsh on 2024-02-28 20:34_

---

_Comment by @humanzz on 2024-02-29 19:29_

@charliermarsh I'm happy to pull this PR, to build it and try it out in my environment, check the logs and see if the issue still shows up. I'm just wondering if there's any instructions for me to follow. I checked https://github.com/astral-sh/uv/blob/main/CONTRIBUTING.md, but am not sure (again with my Rust ignorance) how the development `uv` binary can be created. If that knowledge gap of mine can be closed, then I should be able to test that binary out and report back what I've observed.

---

_Comment by @charliermarsh on 2024-02-29 20:50_

@humanzz - Thanks! You could run `cargo build` in the `uv` directory, and then the `uv` binary will exist at `./target/debug/uv` (e.g., `./target/debug/uv pip install ...`).

---

_Comment by @zanieb on 2024-03-01 15:57_

Can we emit a warning? I'm also a little hesitant but it _is_ more resilient.

---

_Comment by @humanzz on 2024-03-04 21:04_

Unfortunately, whatever testing am doing is not conclusive. I managed to build this PR, and gave it a try a few times... it worked most times, but it still fails from time to time.

I've compared using `pip` and in the proxy logs I'm definitely not seeing any exceptions, but when using `uv` I see the exceptions... but sometimes the overall build succeeds, and some times it doesn't.

I got a bit busy a bit, but I'll still try to look into this. The failures are either the already reported `BufError` or sometimes a
`  × No solution found when resolving dependencies:`. It's not clear if that might be hiding a `BufError` under the hood, or these are just completely unrelated issues.

---

_Comment by @charliermarsh on 2024-05-23 15:07_

Going to close for now.

---

_Closed by @charliermarsh on 2024-05-23 15:07_

---
