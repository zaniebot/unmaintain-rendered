```yaml
number: 11477
title: Make the cache dir  wherever uv is called from when --no-cache specified
type: pull_request
state: open
author: shaneikennedy
labels: []
assignees: []
base: main
head: main
created_at: 2025-02-13T12:45:06Z
updated_at: 2025-06-24T13:58:19Z
url: https://github.com/astral-sh/uv/pull/11477
synced_at: 2026-01-10T11:10:38Z
```

# Make the cache dir  wherever uv is called from when --no-cache specified

---

_Pull request opened by @shaneikennedy on 2025-02-13 12:45_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

As brought up [here](https://github.com/astral-sh/uv/issues/11385), using the tempfile::env::temp_dir default per system is not ideal for cases where directories are on different mounts, or simply in the context where the system user doesn't have write permissions the temp_dir default.

I read the initial issue and @charliermarsh 's [follow up comment](https://github.com/astral-sh/uv/issues/11385#issuecomment-2649540234) and I think the coupling to the virtual env directory is not necessary, and instead I think we can simply default the tempdir to the directory where uv is being invoked from. I think this is a sensible default since it is common to run the project from the project root AND the .venv directory is created in the project root so we should have write permissions here.

~In case for some reason the directory that uv is invoked from **also** doesn't have write permissions (or you just don't want the temp cache there for some reason?), I added a commit that will respect UV_CACHE_DIR so that these users can provide a directory for a temp cache when using --no-cache~


## Test Plan

```
uv git:main*
❯ target/debug/uv cache dir --no-cache
/Users/shane.kennedy/dev/shane/uv/.tmp4b6zap
```

Also used the uv build for building/syncing some test projects and it seems to work fine, I think the test suite for uv will be more comprehensive, however.


---

_Marked ready for review by @shaneikennedy on 2025-02-13 13:42_

---

_Renamed from "Make the cache dir inside project when --no-cache specified" to "Make the cache dir  wherever uv is called from when --no-cache specified" by @shaneikennedy on 2025-02-13 14:10_

---

_Comment by @shaneikennedy on 2025-02-17 14:22_

@edmorley Hey :) Since this PR attempts to cover your use-case in the issue you submitted re:--no-cache, would you mind taking a look at the PR description and letting me know if you think this would resolve the issue for you at Heroku, please? I think I understood your problem correctly but want to double check

I know your suggestion was to put the temp dir inside the venv but I understood that the main problem was hard linking and not wanting to do manual cleanup, which should be satisfiable here either by the default (temp dir inside directory where uv is invoked from) or by using UV_CACHE to create the temp dir somewhere that works for you

---

_Comment by @shaneikennedy on 2025-02-27 09:47_

@zanieb @charliermarsh in the issue it seems like you two are aligned on fixing this, wdyt?

---

_Comment by @shaneikennedy on 2025-03-20 11:47_

Needed a rebase due to some merge conflicts, @zanieb what do you think about this change?

There's a failing test for all platforms but seems unrelated to this PR as all recent build are failing like this. I can rebase again later to trigger another build 👍 
> × No solution found when resolving dependencies:
>  ╰─▶ Because iniconfig<=2.0.0 needs to be downloaded from a registry and you require iniconfig, we can conclude that your requirements are unsatisfiable.

---

_Comment by @zanieb on 2025-03-20 13:39_

Sorry for not getting back to you sooner. I'm pretty hesitant to pollute the working directory.

I think we also can't respect `UV_CACHE_DIR` with `--no-cache` as I think the expectation there is that we won't write to the configured cache directory.

I'm not sure what the solution is here yet, it seems like we'll have to explore the problem space more.

---

_Comment by @shaneikennedy on 2025-03-31 08:26_

Agree on the CACHE_DIR for sure, I put it up incase a workaround was preferable but it's definitely confusing, I popped that commit 👍 

I don't understand what the problem is with a temp dir in the project root since that's where the uv cache goes by default when you don't use --no-cache, out of curiosity what is the concern there?

Is putting the cache dir in the virtualenv preferable? it sounded like that was an acceptable solution in the issue but the coupling between cache and virtualenv feels unecessary

---

_Comment by @daniel-albuschat on 2025-06-24 12:29_

@shaneikennedy See my comment on the related ticket: https://github.com/astral-sh/uv/issues/11385#issuecomment-3000172952
It's essential to have `uv` operate on read-only file-systems, with the exception of user-configurable directories like `UV_CACHE_DIR`.

---

_Comment by @shaneikennedy on 2025-06-24 13:58_

> @shaneikennedy See my comment on the related ticket: [#11385 (comment)](https://github.com/astral-sh/uv/issues/11385#issuecomment-3000172952) It's essential to have `uv` operate on read-only file-systems, with the exception of user-configurable directories like `UV_CACHE_DIR`.

makes sense, but specifically for the topic of caching in uv it probably doesn't make sense for you to use --no-cache anyways with the current way it works since write access is needed in _some_ directory to write the tempfile to that is subject to change by the uv maintainers. I tried to accommodate this in a previous commit in this PR but it's definitely confusing to have --no-cache + a cache CACHE_DIR setting, so I removed it



---
