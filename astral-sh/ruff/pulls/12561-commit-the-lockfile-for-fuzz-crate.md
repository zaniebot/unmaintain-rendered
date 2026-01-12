```yaml
number: 12561
title: "Commit the lockfile for `fuzz` crate"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
assignees: []
base: main
head: dhruv/fuzz-lock
created_at: 2024-07-29T04:42:51Z
updated_at: 2024-07-30T02:47:54Z
url: https://github.com/astral-sh/ruff/pull/12561
synced_at: 2026-01-12T15:55:41Z
```

# Commit the lockfile for `fuzz` crate

---

_@dhruvmanila_

## Summary

This PR adds the `Cargo.lock` file back to the `fuzz` crate.

I'm not exactly sure why this was removed in https://github.com/astral-sh/ruff/pull/5008 but I suspect that it causes problems in upgrading the dependencies (https://github.com/astral-sh/ruff/pull/12558).

I generated the lockfile using:
```
cargo generate-lockfile --manifest-path fuzz/Cargo.toml
```

Another way which produces the same lockfile:
```
cargo update --manifest-path fuzz/Cargo.toml
```


---

_Label `internal` added by @dhruvmanila on 2024-07-29 04:42_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-29 05:34_

---

_Comment by @MichaReiser on 2024-07-29 06:18_

Hmm not sure. I could see that re-adding the lock file could be annoying because we then have to update two lock files whenever we change any dependency (once in Ruff, and once in the fuzzer). Maybe we should just tweak renovate to not update the fuzzer project?

---

_Comment by @dhruvmanila on 2024-07-29 07:16_

> I could see that re-adding the lock file could be annoying because we then have to update two lock files whenever we change any dependency (once in Ruff, and once in the fuzzer).

Wouldn't Renovate update both of them which is what it's doing in https://github.com/astral-sh/ruff/pull/12558? Or, is your concern related to local development?

---

_Comment by @MichaReiser on 2024-07-29 09:25_

My concern is about local development and I'm not sure how important a lockfile is for `fuzz`, because we don't ship it.

---

_Comment by @dhruvmanila on 2024-07-30 02:47_

> My concern is about local development and I'm not sure how important a lockfile is for `fuzz`, because we don't ship it.

Yeah, makes sense.

---

_Closed by @dhruvmanila on 2024-07-30 02:47_

---
