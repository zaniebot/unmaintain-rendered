```yaml
number: 898
title: "bench: set log level to WARN, make --verbose raise it to INFO"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/quiet-please
created_at: 2024-01-12T14:42:54Z
updated_at: 2024-01-12T15:08:00Z
url: https://github.com/astral-sh/uv/pull/898
synced_at: 2026-01-12T16:04:16Z
```

# bench: set log level to WARN, make --verbose raise it to INFO

---

_@BurntSushi_

I personally found the output by default somewhat noisy, especially for
large requirements files. Since --verbose is already a thing, I propose
making the extra output opt-in.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-12 14:43_

---

_Review requested from @zanieb by @BurntSushi on 2024-01-12 14:43_

---

_Comment by @BurntSushi on 2024-01-12 14:43_

An alternative that I'd also be cool with is a `-q/--quiet` flag that lowers the log level, so that it stays `INFO` by default. But since `--verbose` was already there, I figured I'd try this first.

---

_Comment by @charliermarsh on 2024-01-12 14:53_

I think the main thing that `--verbose` does is it enables the hyperfine output.

---

_Comment by @charliermarsh on 2024-01-12 14:57_

Oh nevermind, I misunderstood the change.

---

_Comment by @BurntSushi on 2024-01-12 14:57_

> I think the main thing that `--verbose` does is it enables the hyperfine output.

Right. This PR throws more into the verbose bucket. But maybe you're saying you want a separate flag to control hyperfine output?

---

_@charliermarsh reviewed on 2024-01-12 14:57_

---

_Review comment by @charliermarsh on `scripts/bench/__main__.py`:524 on 2024-01-12 14:57_

Could also do this after argument parsing, and `level=logging.INFO if verbose else logging.WARN`.

---

_@charliermarsh approved on 2024-01-12 14:57_

---

_Label `internal` added by @charliermarsh on 2024-01-12 14:58_

---

_@BurntSushi reviewed on 2024-01-12 14:59_

---

_Review comment by @BurntSushi on `scripts/bench/__main__.py`:524 on 2024-01-12 14:59_

Yeah I left it as-is in case there was anything interesting added pre-arg parsing. But I don't think so. I'll go with your suggestion.

---

_Merged by @BurntSushi on 2024-01-12 15:07_

---

_Closed by @BurntSushi on 2024-01-12 15:07_

---

_Branch deleted on 2024-01-12 15:08_

---
