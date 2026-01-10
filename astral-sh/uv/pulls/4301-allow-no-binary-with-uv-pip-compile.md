```yaml
number: 4301
title: "Allow `--no-binary` with `uv pip compile`"
type: pull_request
state: merged
author: brianmego
labels:
  - cli
assignees: []
merged: true
base: main
head: no_binary_compile
created_at: 2024-06-13T04:01:47Z
updated_at: 2024-06-13T13:59:03Z
url: https://github.com/astral-sh/uv/pull/4301
synced_at: 2026-01-10T13:54:02Z
```

# Allow `--no-binary` with `uv pip compile`

---

_Pull request opened by @brianmego on 2024-06-13 04:01_

## Summary

This i still a draft, but it gets some of the work started on issue #4064 , which requests --no-binary functionality similar to `uv pip install` to exist on `uv pip compile`.

So far this has moved the command line shape from install and cloned it over to compile. The actual functionality of respecting --no-binary <package> and generating the resulting line in requirements.txt I could use a hand with.

My understanding is we want to create a requirements.in file such as:

```
yt
```

Then when running  `cargo run -- pip compile --no-binary yt requirements.in`
we want the file to have this line in it:

```
yt==4.3.1 --no-binary yt
```

## Test Plan

Existing unit tests continue to pass.
No new unit tests have been created yet.
The new command line options do show up when testing with `cargo run -- pip compile -h`

---

_Assigned to @charliermarsh by @zanieb on 2024-06-13 13:12_

---

_Comment by @charliermarsh on 2024-06-13 13:46_

I think this is actually "done". I'll add a test to demonstrate it.

---

_Renamed from "Draft: Copies some of the install no-binary code over to compile (#4064)" to "Allow `--no-binary` with `uv pip compile`" by @charliermarsh on 2024-06-13 13:50_

---

_Label `cli` added by @charliermarsh on 2024-06-13 13:50_

---

_@charliermarsh approved on 2024-06-13 13:51_

---

_Merged by @charliermarsh on 2024-06-13 13:59_

---

_Closed by @charliermarsh on 2024-06-13 13:59_

---
