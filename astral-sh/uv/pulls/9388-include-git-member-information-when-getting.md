```yaml
number: 9388
title: Include Git member information when getting metadata from cache
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - bug
assignees: []
merged: true
base: main
head: git-cached-metadata
created_at: 2024-11-23T16:51:11Z
updated_at: 2024-12-03T05:07:21Z
url: https://github.com/astral-sh/uv/pull/9388
synced_at: 2026-01-12T16:08:47Z
```

# Include Git member information when getting metadata from cache

---

_@ericmarkmartin_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Include the `git_member` when fetching metadata from cache.

h/t to @PhilipVinc for the suggested fix

Resolves #8887 

## Test Plan

Pending


---

_Comment by @ericmarkmartin on 2024-11-23 17:09_

It looks like some of the failing tests are the "too many requests" thing I alluded to in [my comment](https://github.com/astral-sh/uv/issues/8887#issuecomment-2495273694) on the issue

---

_Comment by @charliermarsh on 2024-11-23 17:10_

Yeah I think something is up with that index. I should look into removing our dependency on it.

---

_@charliermarsh approved on 2024-11-24 02:00_

This seems right, but is there a way to test it?

---

_Review requested from @konstin by @charliermarsh on 2024-11-24 02:00_

---

_Label `bug` added by @charliermarsh on 2024-11-24 02:00_

---

_Comment by @ericmarkmartin on 2024-11-24 05:19_

> This seems right, but is there a way to test it?

Could we set up a packse like the example given in the issue (see https://github.com/PhilipVinc/uvtest/tree/dab673b4ad5c6f1335c491baf49ad4965562e876)?

Also, I generally am pretty clueless about uv's testing story (I know it's snapshot based, but that's about it) but would be eager to learn by writing some tests here

---

_Comment by @zanieb on 2024-11-25 23:23_

If it would help we could add a repository in our test org, e.g. https://github.com/astral-test/uv-submodule-pypackage

I'm not sure I understand the issue here though.

---

_Comment by @ericmarkmartin on 2024-11-26 00:56_

> If it would help we could add a repository in our test org, e.g. https://github.com/astral-test/uv-submodule-pypackage

That could be helpful. I take it that'd be easier than getting packse to serve git repos? One maybe relevant question here is whether we can use `git+` with file-scheme urls?

---

_Comment by @zanieb on 2024-11-26 01:13_

>  I take it that'd be easier than getting packse to serve git repos?

Yeah this would probably be a pain.

> One maybe relevant question here is whether we can use git+ with file-scheme urls?

I don't quite follow, perhaps you should tell me what a test case would look like given a `uv` binary and I can help construct it? Perhaps as an answer to your question, a test could just clone a repository to a temporary directory then pass that path to uv?

---

_Comment by @ericmarkmartin on 2024-11-26 01:50_

> Perhaps as an answer to your question, a test could just clone a repository to a temporary directory then pass that path to uv?

For sure, I just wasn't at my computer earlier. I just confirmed that using `git+` with `file://` urls works, though unfortunately testing this with @PhilpVinc's test repository (see linked issue) had the conflicting URL issue reoccur 😞 


---

_Comment by @konstin on 2024-11-26 21:06_

If you need inspiration for tests, there are `transitive_dep_in_git_workspace_no_root` and `transitive_dep_in_git_workspace_with_root` from previous bug fixes in the area

---

_Comment by @charliermarsh on 2024-12-02 14:09_

I'd like to get this merged today so we can release it. I'll add a test if needed.

---

_Comment by @ericmarkmartin on 2024-12-02 16:39_

> I'd like to get this merged today so we can release it. I'll add a test if needed.

Hey,

Apologies for not getting to this, I had to get ready for a vacation. I’m out of the country without my personal computer right now so I think the chances I can do this today are pretty low.

---

_Comment by @charliermarsh on 2024-12-03 04:30_

@ericmarkmartin -- No problem at all!

---

_Renamed from "include git_member when getting metadata from cache" to "Include Git member information when getting metadata from cache" by @charliermarsh on 2024-12-03 04:52_

---

_Merged by @charliermarsh on 2024-12-03 05:07_

---

_Closed by @charliermarsh on 2024-12-03 05:07_

---
