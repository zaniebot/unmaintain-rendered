```yaml
number: 12255
title: Continue on client errors when fetching wheel metadata
type: pull_request
state: open
author: charliermarsh
labels:
  - enhancement
  - resolver
assignees: []
base: main
head: charlie/continue
created_at: 2025-03-18T01:09:17Z
updated_at: 2025-10-02T18:09:37Z
url: https://github.com/astral-sh/uv/pull/12255
synced_at: 2026-01-12T16:10:12Z
```

# Continue on client errors when fetching wheel metadata

---

_@charliermarsh_

## Summary

This PR allows the resolver to continue (backtrack) when wheel metadata fetches fail due to a client error (like a 403). We may want this to be opt-in?

Closes https://github.com/astral-sh/uv/issues/5260.

If _all_ distributions can't be fetched, you get something like this:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because all versions of flask could not be fetched from the network (`404 Not Found`) and you require flask, we
      can conclude that your requirements are unsatisfiable.

      hint: Pre-releases are available for `flask` in the requested range (e.g., 2.0.0rc2), but pre-releases weren't
      enabled (try: `--prerelease=allow`)

      hint: Metadata for `flask` (v3.1.0) could not be fetched; the server returned: `404 Not Found`
```


---

_Label `enhancement` added by @charliermarsh on 2025-03-18 01:09_

---

_Label `resolver` added by @charliermarsh on 2025-03-18 01:09_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-18 02:26_

---

_Review requested from @konstin by @charliermarsh on 2025-03-18 02:26_

---

_@konstin approved on 2025-03-18 12:30_

Code looks good, slight preference for making this opt-in

---

_Comment by @charliermarsh on 2025-03-18 20:07_

I need to see what this looks like when you _fail_ to authenticate with all wheels for a given server.

---

_Marked ready for review by @charliermarsh on 2025-03-18 20:50_

---

_Comment by @zanieb on 2025-03-18 21:49_

Can we make this an opt-in setting per index? I'm not sure what to call it yet.

---

_Comment by @charliermarsh on 2025-03-18 22:00_

Oh no, a name...

---

_@paveldikov reviewed on 2025-03-20 13:33_

---

_Review comment by @paveldikov on `crates/uv/tests/it/edit.rs`:10118 on 2025-03-20 13:33_

Ooooh, this is terrific. That error message by itself is quite a potent troubleshooting aide.

---

_@paveldikov reviewed on 2025-03-20 13:34_

---

_Review comment by @paveldikov on `crates/uv/tests/it/edit.rs`:10118 on 2025-03-20 13:34_

P.S. if this test case were cloned with `--prerelease=allow`, it should succeed, right?

---

_Comment by @bind-cannon on 2025-10-02 18:03_

This would be a huge win since this issue happens in all large organizations with blocking artifactory products like nexus and jfrog. What is the next step to getting this merged?

---

_Comment by @charliermarsh on 2025-10-02 18:09_

I think I need to make it opt-in per-index.

---
