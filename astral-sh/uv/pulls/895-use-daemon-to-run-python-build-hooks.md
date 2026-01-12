```yaml
number: 895
title: Use daemon to run Python build hooks
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/hookd
created_at: 2024-01-12T04:31:00Z
updated_at: 2024-01-29T21:22:31Z
url: https://github.com/astral-sh/uv/pull/895
synced_at: 2026-01-12T16:04:16Z
```

# Use daemon to run Python build hooks

---

_@zanieb_

Closes https://github.com/astral-sh/puffin/issues/846

Spawns a single Python process per source distribution build instead of one per build backend hook we call. The Python process is a custom script providing PEP 517 / 660 build hook RPC. We use simple newline terminated messages to communicate between the parent Rust process and the child Python process. Since standard out is reserved for this communication, we redirect all output from the build backend during hook calls into a separate temporary file. The file path is sent from the Python daemon to the Rust parent so it can recover the hook's output for display on failure.

There's a great deal of complexity in this implementation which mostly arises from:
- Proper management the daemon process on the Rust side, e.g. non-blocking and no zombie processes
- Redirection of file streams, error capturing, and reloading of modules on the Python side

The goal here is to improve performance by avoiding the 10ms overhead of invoking a Python process on each hook call. When running a daemon, we only pay the 10ms overhead cost once and subsequent hook calls should have <1ms of overhead. However, we can only use a daemon for a single build since it needs to be running in the correct virtual environment. Consequently, we only make 2-3 hook calls per daemon so there's a small limit to the total reduction in overhead we can see in practice. We could explore passing the virtual environment via RPC and "hot loading" it per build; then we could re-use the daemon for many more builds and hook calls. 

Additionally, https://github.com/astral-sh/puffin/issues/846 notes that build backend imports are slow. Without a daemon, we must pay the cost of importing the build backend on each hook call. The initial daemon implementation cached imports to reduce this overhead as much as possible. However, since we add packages to the build environment _after_ the daemon is started it is actually _not_ safe to cache imports. In practice, the daemon needed to _force_ all modules to be reloaded on each hook call for builds to succeed. Perhaps we could limit the effect of this by adding a special command to reset modules and only do so after the parent changes the build environment. Regardless, this adds significant complexity to the implementation and may be brittle in the real world.

Rough benchmarks e.g. running `pip sync` with `flask==3.0.0` and the `--no-binary` flag (#956) to force source distribution builds show no clear improvement. Of course, we could spend time optimizing the Rust side of this implementation (the Python side is already relatively optimized) but I'm not sure if it's worth going deeper into at this time.

In implementing this, there are _some_ gains beyond performance. For example, we now have much more detailed error types for failures in hooks.

---

_Renamed from "Initial commit of daemon for running Python build hooks" to "Use daemon to run Python build hooks" by @zanieb on 2024-01-12 04:31_

---

_@zanieb reviewed on 2024-01-17 20:07_

---

_Review comment by @zanieb on `crates/puffin-build/src/daemon.rs`:352 on 2024-01-17 20:07_

These have file paths and we should create async background tasks to read from these streams so we can report stdout and stderr from the hook on failure.

---

_Comment by @zanieb on 2024-01-18 17:16_

Example benchmarks

Install `flask==3.0.0`

```
$ hyperfine --min-runs 20 \
    --command-name branch \
    --command-name baseline \
    './target/release/branch pip install flask==3.0.0 --no-binary --reinstall --no-cache' \
    './target/release/baseline pip install flask==3.0.0 --no-binary --reinstall --no-cache'


Benchmark 1: branch
  Time (mean ± σ):      3.663 s ±  0.075 s    [User: 3.616 s, System: 1.900 s]
  Range (min … max):    3.535 s …  3.833 s    20 runs
 
Benchmark 2: baseline
  Time (mean ± σ):      3.895 s ±  0.637 s    [User: 3.822 s, System: 1.948 s]
  Range (min … max):    3.586 s …  6.389 s    20 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Summary
  branch ran
    1.06 ± 0.18 times faster than baseline
```

Install a scenario that uses hatchling to build a trivial package

```
$ hyperfine --min-runs 20 \
    --command-name branch \
    --command-name baseline \
    './target/release/branch pip install a-614d801c==1.0.0 --extra-index-url https://test.pypi.org/simple/ --no-binary --reinstall --no-cache' \
    './target/release/baseline pip install a-614d801c==1.0.0 --extra-index-url https://test.pypi.org/simple/ --no-binary --reinstall --no-cache'

Benchmark 1: branch
  Time (mean ± σ):      5.218 s ±  0.229 s    [User: 4.849 s, System: 1.871 s]
  Range (min … max):    4.997 s …  5.966 s    20 runs
 
Benchmark 2: baseline
  Time (mean ± σ):      5.649 s ±  0.231 s    [User: 5.446 s, System: 2.080 s]
  Range (min … max):    5.336 s …  6.094 s    20 runs
 
Summary
  branch ran
    1.08 ± 0.06 times faster than baseline
```


---

_Closed by @zanieb on 2024-01-19 17:24_

---

_Review comment by @BurntSushi on `crates/puffin-build/src/daemon.rs`:96 on 2024-01-29 19:55_

In the above, I see a lot of `unwrap()` that looks related to input from the daemon response. I suspect those should be converted to errors? (Since we own the daemon though, you might insist that invalid responses from the daemon should provoke a panic in this program. Which is maybe fine... But a comment indicating that would be good hah. And I do see other error types being used for invalid input.)

---

_Review comment by @BurntSushi on `crates/puffin-build/src/daemon.rs`:13 on 2024-01-29 19:58_

Separate from the question of whether to merge imports together, I would at least say it's very common to group imports: std, then external crates and finally crate internal. But I think std is mixed with external crates above, and crate internal imports are mixed with other external crate imports here.

---

_Review comment by @BurntSushi on `crates/puffin-build/src/daemon.rs`:321 on 2024-01-29 20:01_

I think this could be a `&[&str]`. And then the `commands.append(&mut args);` call below could be changed to `commands.extend_from_slice(args);`.

---

_@BurntSushi reviewed on 2024-01-29 20:03_

Nice! I did a quick review of the Rust code in this PR, and I think at a high level everything looks pretty good to me. I think my only high level objection/concern would be the use of async I/O for something like this, but in this context, given that puffin uses async elsewhere, it probably makes sense to do it for this too.

---

_@zanieb reviewed on 2024-01-29 21:22_

---

_Review comment by @zanieb on `crates/puffin-build/src/daemon.rs`:96 on 2024-01-29 21:22_

Almost all of the unwraps are just WIP artifacts

---
