```yaml
number: 1314
title: "cli: conventionally treat `-` as \"read from stdin\""
type: pull_request
state: merged
author: BurntSushi
labels:
  - compatibility
assignees: []
merged: true
base: main
head: ag/requirements-on-stdin
created_at: 2024-02-15T15:54:11Z
updated_at: 2024-02-15T16:25:57Z
url: https://github.com/astral-sh/uv/pull/1314
synced_at: 2026-01-12T16:04:35Z
```

# cli: conventionally treat `-` as "read from stdin"

---

_@BurntSushi_

Basically, when a path to a requirements file is `-`, then we should
read its contents from `stdin` instead of the file path named `-`.

Fixes #1313


---

_Review requested from @mitsuhiko by @BurntSushi on 2024-02-15 15:54_

---

_Review requested from @zanieb by @BurntSushi on 2024-02-15 15:54_

---

_@zanieb approved on 2024-02-15 15:55_

Love it

---

_Comment by @zanieb on 2024-02-15 15:55_

Do we need a docs change?

---

_Review comment by @charliermarsh on `crates/puffin-fs/src/lib.rs`:38 on 2024-02-15 15:57_

Should this switching instead happen as a new `RequirementsSource`? (I don't feel strongly.)

---

_@charliermarsh reviewed on 2024-02-15 15:57_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:195 on 2024-02-15 15:58_

This should be impossible to hit, I think, since we only go down this path if the filename is exactly `pyproject.toml`.

---

_@charliermarsh reviewed on 2024-02-15 15:58_

---

_@BurntSushi reviewed on 2024-02-15 16:00_

---

_Review comment by @BurntSushi on `crates/puffin/src/requirements.rs`:195 on 2024-02-15 16:00_

That's what I figured, but I think that's more "an artifact of how the current code behaves" rather than "a guarantee provided by the module's API." So for example, if in the future one could create a `PyprojectToml` via some other means, then the case of reading from stdin will be handled automatically.

---

_@BurntSushi reviewed on 2024-02-15 16:01_

---

_Review comment by @BurntSushi on `crates/puffin-fs/src/lib.rs`:38 on 2024-02-15 16:01_

I honestly don't know. It looked to me that `RequirementsSource` was more about the format than the delivery.

---

_Comment by @BurntSushi on 2024-02-15 16:07_

> Do we need a docs change?

Yeah good call. Done!

---

_Label `compatibility` added by @BurntSushi on 2024-02-15 16:08_

---

_Comment by @charliermarsh on 2024-02-15 16:15_

What happens if `-` is passed multiple times?

---

_Comment by @BurntSushi on 2024-02-15 16:19_

> What happens if `-` is passed multiple times?

In that case, `stdin` will be read from multiple times. But after the first read, stdin is fully consumed and so subsequent reads will return no data. In other words, it will be the equivalent of `cat requirements.txt | puffin pip compile - some-empty-file`. Since empty files are valid requirements files, it ends up just being ignored.

---

_Comment by @BurntSushi on 2024-02-15 16:21_

Other tools behave similarly:

```
$ cat scripts/requirements/black.in | rg black -
black
$ cat scripts/requirements/black.in | rg black - -
<stdin>
1:black
$ cat scripts/requirements/black.in | rg black - - -
<stdin>
1:black
$ cat scripts/requirements/black.in | rg black - - - -
<stdin>
1:black

$ cat scripts/requirements/black.in | grep black -
black
$ cat scripts/requirements/black.in | grep black - -
(standard input):black
$ cat scripts/requirements/black.in | grep black - - -
(standard input):black
$ cat scripts/requirements/black.in | grep black - - - -
(standard input):black
```

We could potentially report an error instead, but I'm not sure if that's worth doing?

---

_Comment by @charliermarsh on 2024-02-15 16:22_

Okay great. I was mostly worried that it would hang on the second stdin call.

---

_Merged by @BurntSushi on 2024-02-15 16:25_

---

_Closed by @BurntSushi on 2024-02-15 16:25_

---

_Branch deleted on 2024-02-15 16:25_

---
