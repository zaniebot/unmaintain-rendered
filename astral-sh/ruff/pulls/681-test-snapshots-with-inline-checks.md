```yaml
number: 681
title: Test snapshots with inline checks
type: pull_request
state: closed
author: squiddy
labels: []
assignees: []
draft: true
base: main
head: test-snapshots
created_at: 2022-11-11T11:56:51Z
updated_at: 2022-11-12T13:00:40Z
url: https://github.com/astral-sh/ruff/pull/681
synced_at: 2026-01-12T15:55:05Z
```

# Test snapshots with inline checks

---

_@squiddy_

This is a massive diff because I'm introducing new snapshots. The first and third commit are the relevant ones. To motivate this a bit:

When developing new lints or checking existing lints, I did one of these two things:

* manually "translated" the location from the yaml snapshot and check against the test fixture
* run cargo against the test fixtures

I also noticed that some test fixtures have inline comments/decorators to clarify what is expected.

---

What I'm doing here is to add a new snapshot (to potentially replace the existing one eventually) that inlines the checks into the source code, similar to what we'd expect with https://github.com/charliermarsh/ruff/issues/525 (which I've been looking into, hence this first step).
Looking at these snapshots it's easier to visually confirm the correct location (currently just row based though).

What I did notice is that in some cases the end location is pretty far off, due to whitespace following it. The third commit is an attempt to narrow down this location to have a closer match.

Example initial snapshot

![carbon](https://user-images.githubusercontent.com/50333/201335542-b6fed52e-cf6b-400d-862e-d66c275a89f4.png)

Example snapshot with narrowed down location

![carbon2](https://user-images.githubusercontent.com/50333/201335537-6f47f535-813b-48da-b92c-ee36459673e9.png)

---

_Comment by @squiddy on 2022-11-11 12:01_

For narrowing down I need to check whether this is actually necessary or, in case the end points to the start of a line, I'll just report the end of the check for the line before. Might just be being confused by the locations.

---

_Closed by @squiddy on 2022-11-12 13:00_

---

_Branch deleted on 2022-11-12 13:00_

---
