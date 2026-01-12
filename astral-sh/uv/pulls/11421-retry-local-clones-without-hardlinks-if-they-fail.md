```yaml
number: 11421
title: retry local clones without hardlinks if they fail
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/harder
created_at: 2025-02-11T14:47:18Z
updated_at: 2025-02-12T00:42:25Z
url: https://github.com/astral-sh/uv/pull/11421
synced_at: 2026-01-12T16:09:50Z
```

# retry local clones without hardlinks if they fail

---

_@Gankra_

Fixes #11420 

---

_Comment by @Gankra on 2025-02-11 14:48_

I need to setup BeeGFS to check that this really works.

---

_Label `bug` added by @Gankra on 2025-02-11 14:48_

---

_Comment by @PhilipVinc on 2025-02-11 18:47_

If you give me a built linux executable I can try for you, if you want.

---

_Comment by @Gankra on 2025-02-11 19:09_

Oh great! The PR CI already built one:

https://github.com/astral-sh/uv/actions/runs/13266446923/artifacts/2572423342

---

_Comment by @PhilipVinc on 2025-02-11 21:12_

Dammit. Unfortunately on my system I have only GLIBC 2.17 available, but you require 2.18...

---

_Comment by @PhilipVinc on 2025-02-11 21:15_

@Gankra I managed to use the musl builds.

It seems there is a typo:
```bash
  Caused by: process didn't exit successfully: `/usr/bin/git clone --no-hard-links /mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/db/292f78b3ecf26bf6 /mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/checkouts/292f78b3ecf26bf6/f1db6bb` (exit status: 129)
--- stderr
error: unknown option `no-hard-links'
usage: git clone [options] [--] <repo> [<dir>]

    -v, --verbose         be more verbose
    -q, --quiet           be more quiet
    --progress            force progress reporting
    -n, --no-checkout     don't create a checkout
    --bare                create a bare repository
    --mirror              create a mirror repository (implies bare)
    -l, --local           to clone from a local repository
    --no-hardlinks        don't use local hardlinks, always copy
    -s, --shared          setup as shared repository
```

It should be `--no-hardlinks` but you specified `--no-hard-links`

---

_Comment by @charliermarsh on 2025-02-11 21:15_

You can use the musl wheel, hopefully? It's under https://github.com/astral-sh/uv/actions/runs/13266446923/artifacts/2572463068.

---

_@PhilipVinc reviewed on 2025-02-11 21:17_

---

_Review comment by @PhilipVinc on `crates/uv-git/src/git.rs`:440 on 2025-02-11 21:17_

```suggestion
                .arg("--no-hardlinks")
```

---

_Comment by @charliermarsh on 2025-02-11 21:20_

Thanks. I triggered a rebuild.

---

_Comment by @charliermarsh on 2025-02-11 21:44_

The updated musl version is https://github.com/astral-sh/uv/actions/runs/13272737821/artifacts/2574639236.

---

_Comment by @PhilipVinc on 2025-02-11 22:03_

tested. It works. thanks!

---

_@charliermarsh approved on 2025-02-11 22:16_

---

_Comment by @charliermarsh on 2025-02-11 22:16_

Great, thanks! I'll leave to @Gankra to merge.

---

_Marked ready for review by @charliermarsh on 2025-02-12 00:42_

---

_Merged by @charliermarsh on 2025-02-12 00:42_

---

_Closed by @charliermarsh on 2025-02-12 00:42_

---

_Branch deleted on 2025-02-12 00:42_

---

_Comment by @charliermarsh on 2025-02-12 00:42_

(Confirmed in Discord.)

---
