```yaml
number: 17536
title: Investigate hash checks for shellcheck installation in CI
type: issue
state: open
author: zanieb
labels:
  - internal
assignees: []
created_at: 2026-01-16T18:49:43Z
updated_at: 2026-01-16T21:49:56Z
url: https://github.com/astral-sh/uv/issues/17536
synced_at: 2026-01-16T22:15:06Z
```

# Investigate hash checks for shellcheck installation in CI

---

_@zanieb_

We should probably check that what we downloaded matches a specified hash, to prevent this being a supply chain attack route. But I'll grant you that the original action didn't do this, so this strictly isn't making things worse.

_Originally posted by @EliteTK in https://github.com/astral-sh/uv/pull/17532#discussion_r2699543921_
            

---

_Label `internal` added by @zanieb on 2026-01-16 18:49_

---

_Comment by @woodruffw on 2026-01-16 21:34_

It'll be a few releases behind, but we _could_ use the shellcheck that comes directly with GitHub's `ubuntu-*` runners: 

https://github.com/actions/runner-images/blob/0c1310995d6f016d1acbb667eeeab13f470a42e0/images/ubuntu/Ubuntu2404-Readme.md

(There's also a secret-but-supported Linuxbrew on every Ubuntu runner, which could be used to bootstrap a newer version.)

---

_Comment by @zanieb on 2026-01-16 21:35_

That'd work for me too

---

_Comment by @zanieb on 2026-01-16 21:39_

I guess then it's unpinned though?

---

_Comment by @woodruffw on 2026-01-16 21:42_

> I guess then it's unpinned though?

It'd be implicitly pinned through the runner image in the first case, and in the second it'd be pinned through the Homebrew formula:

https://github.com/Homebrew/homebrew-core/blob/880f28f21145efcfd79a2e6992d6467bb6c77424/Formula/s/shellcheck.rb#L9-L18

(I think in both instances the risk profile is small, since we already trust the runner to have a not-compromised shell and other binaries that we don't/can't hash.)

---

_Comment by @zanieb on 2026-01-16 21:49_

The runner images change though?

I don't care about risk profile with the version pin, I care about new violations showing up without an upgrade

---
