```yaml
number: 8633
title: Better trusted publishing error story
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - error messages
  - preview
assignees: []
merged: true
base: main
head: konsti/trusted-publishing-debugging
created_at: 2024-10-28T12:49:02Z
updated_at: 2024-10-28T20:13:45Z
url: https://github.com/astral-sh/uv/pull/8633
synced_at: 2026-01-12T16:08:24Z
```

# Better trusted publishing error story

---

_@konstin_

## Summary

Trusted publishing errors are a tough problem because for security reason, PyPI won't tell use the trusted publishing configuration for a repo and GitHub Actions doesn't let us see arbitrary secrets either.

These changes handle using trusted publishing while other credentials are also set (an error) and adds a hint and an error trace when it looks like the user wanted trusted publishing, but it wasn't configured.

Fixes #8223.

## Error messages

The screenshots are from https://github.com/konstin/trusted-publishing-examples/actions/runs/11555118347. Some cases, e.g. the wrong environment and the wrong workflow name, return the same error since PyPI doesn't give use more information for security reasions.

The user set the wrong workflow name, using `uv publish --trusted-publishing always`:

![image](https://github.com/user-attachments/assets/b150228e-eefd-4bba-bdcb-4496d2c347a0)

The user set the wrong workflow name, using plain `uv publish`:

![image](https://github.com/user-attachments/assets/3d32c2ef-770c-4eab-be01-ac2ecf6b2b62)

The user forgot the `id-token: write` permission:

![image](https://github.com/user-attachments/assets/541af555-caf1-4e88-933d-339ca44cfc46)

The user forgot the environment:

![image](https://github.com/user-attachments/assets/63fdbfd0-b91d-47c2-88f6-b49ad3f4ebc8)


---

_Label `enhancement` added by @konstin on 2024-10-28 12:49_

---

_Label `error messages` added by @konstin on 2024-10-28 12:49_

---

_Label `preview` added by @konstin on 2024-10-28 12:49_

---

_Review comment by @BurntSushi on `crates/uv-publish/src/lib.rs`:304 on 2024-10-28 16:20_

Would it make sense to try and combine these into one error? Otherwise, users might hit the first one, then fix it. Then hit the second one, then fix it. And so on.

---

_@BurntSushi approved on 2024-10-28 16:21_

---

_Merged by @konstin on 2024-10-28 20:13_

---

_Closed by @konstin on 2024-10-28 20:13_

---

_Branch deleted on 2024-10-28 20:13_

---
