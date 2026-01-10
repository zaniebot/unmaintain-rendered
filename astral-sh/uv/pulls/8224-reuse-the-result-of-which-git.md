```yaml
number: 8224
title: "Reuse the result of `which git`"
type: pull_request
state: merged
author: j178
labels:
  - performance
assignees: []
merged: true
base: main
head: git
created_at: 2024-10-15T16:18:13Z
updated_at: 2024-10-15T18:28:54Z
url: https://github.com/astral-sh/uv/pull/8224
synced_at: 2026-01-10T12:54:05Z
```

# Reuse the result of `which git`

---

_Pull request opened by @j178 on 2024-10-15 16:18_

## Summary

Cache the path to git executable in a `LazyLock` and reuse it throughout the process. This might reduce some costs on finding the git executable.


---

_Label `performance` added by @zanieb on 2024-10-15 16:24_

---

_Review requested from @konstin by @charliermarsh on 2024-10-15 16:28_

---

_Comment by @konstin on 2024-10-15 16:32_

Sounds like a good idea, do you have some numbers how much overhead finding git is?

---

_Comment by @j178 on 2024-10-15 17:22_

I wrote a simple script to measure the overhead of finding git. Running it 50 times reduces the time by ~39ms on my machine.

```rust
use std::time::{Duration, Instant};
use std::process::Command;

fn run(git: &str, n: u32) -> Duration {
    let start = Instant::now();
    for _ in 0..n {
        let status = Command::new(git)
            .arg("--version")
            .stdout(std::process::Stdio::null())
            .status()
            .unwrap();
        assert!(status.success());
    }
    start.elapsed()
}

fn main() {
    let n = 50;
    let d1 = run("git", n);
    let d2 = run("/opt/homebrew/bin/git", n);
    println!("diff: {}ms", (d1 - d2).as_millis());
}
```

---

_Comment by @j178 on 2024-10-15 17:44_

The overhead depends on the path's position in `PATH`. If I move the Git path to the end (18th) of `PATH`, the reduced time can increase to approximately 500ms.

---

_Comment by @charliermarsh on 2024-10-15 17:45_

Looks reasonable to me.

---

_Merged by @charliermarsh on 2024-10-15 17:50_

---

_Closed by @charliermarsh on 2024-10-15 17:50_

---

_Branch deleted on 2024-10-15 18:28_

---
