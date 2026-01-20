```yaml
number: 17533
title: Change chocolatey system test to ensure uv uses the right python
type: pull_request
state: open
author: EliteTK
labels:
  - bug
  - "test:system"
assignees: []
base: main
head: tk/fix-ci-system-chocolatey
created_at: 2026-01-16T17:40:54Z
updated_at: 2026-01-20T14:30:57Z
url: https://github.com/astral-sh/uv/pull/17533
synced_at: 2026-01-20T14:40:58Z
```

# Change chocolatey system test to ensure uv uses the right python

---

_@EliteTK_

## Summary

Fix #17524. 

## Test Plan

N/A

---

_Label `bug` added by @EliteTK on 2026-01-16 17:40_

---

_Label `test:system` added by @EliteTK on 2026-01-16 17:40_

---

_Marked ready for review by @EliteTK on 2026-01-16 17:47_

---

_Review requested from @zanieb by @EliteTK on 2026-01-16 17:47_

---

_Converted to draft by @EliteTK on 2026-01-16 17:48_

---

_Comment by @EliteTK on 2026-01-16 17:49_

Seems like the python path printing is borked now...

---

_Marked ready for review by @EliteTK on 2026-01-16 17:56_

---

_Comment by @zanieb on 2026-01-16 18:17_

I think using `UV_TEST_PYTHON_PATH` isn't okay in the context of the system tests.

---

_Comment by @zanieb on 2026-01-16 18:18_

I think the interpreter that invokes the system test script is the one that should be used, right?

---

_Comment by @EliteTK on 2026-01-16 18:54_

The test script interpreter is the one whose parent directory I am putting in `UV_TEST_PYTHON_PATH`. Although I agree it's kind of jank in retrospect. It could hide a bug in our path parsing.

As an alternative we could prepend to `$env:PATH` [^gh-path] before running the script.

I think the real weird thing here is that there's no actual guarantee that `py -3.9` would pick the chocolatey version if some other 3.9 was installed, but I couldn't find any good way to verify this invariant.

But at least I feel that setting `$env:PATH` prior to running the verification script would at least ensure that the script and uv both use the same python version.

Lastly, I could try to investigate why chocolatey's path changes aren't being picked up although I have a feeling that the answer is going to be either: "the registry changes can't be propagated to the runner without restarting the runner" or "github runners explicitly sanitize that somehow so the environment change doesn't persist".

[^gh-path]: GITHUB_PATH seems to be for appending only, which isn't helpful here.

---

_Comment by @zanieb on 2026-01-16 19:04_

> I think the real weird thing here is that there's no actual guarantee that py -3.9 would pick the chocolatey version if some other 3.9 was installed, but I couldn't find any good way to verify this invariant.

Can't we just use the path that chocolatey is at instead of `py -3.9`? Maybe I'm missing something?

---

_Comment by @zanieb on 2026-01-16 19:05_

But yeah I think a point of these tests is uv's discovery of system Python versions, so overriding that with our custom test path seems janky. That variable shouldn't really be used in more places, it's quite the blemish already imo (I added it, alas)

---

_Comment by @EliteTK on 2026-01-16 19:26_

> Can't we just use the path that chocolatey is at instead of `py -3.9`? Maybe I'm missing something?

Yeah, sorry, I just realised there was a key bit of context I forgot:

It seems you can tell chocolatey where you'd like to put the installation, but it won't listen if that version is already installed (it will overwrite at the existing location). There also doesn't seem to be any good way of asking chocolatey where it did install it either.

This means using `C:\Python39\` (the default path) could break if we revert the runner image or a future image starts shipping python 3.9 again.

But, having thought about it, prepending `C:\Python39\` and then using `C:\Python39\python.exe .\script` is probably the safest option: it will fail if anything weird happens to the image which could cause us to e.g. start testing a non-chocolatey python version.

What might also work: We could try to set `$env:PATH` from the registry and invoke `python` instead of `py -3.9`. But we'd have to verify the version matches what we expect.

---

_Renamed from "Set `UV_TEST_PYTHON_PATH` for the chocolatey system test to ensure uv uses the right python" to "Change chocolatey system test to ensure uv uses the right python" by @EliteTK on 2026-01-19 15:39_

---

_Comment by @EliteTK on 2026-01-20 12:32_

@zanieb I went with the approach of refreshing the path from the registry. Although this means that `GITHUB_PATH` won't work properly...

---

_@zanieb reviewed on 2026-01-20 14:25_

---

_Review comment by @zanieb on `.github/workflows/test-system.yml`:549 on 2026-01-20 14:25_

Should we just put this into `check_system_python.py` with a `--python-version` flag?

---

_Review comment by @zanieb on `.github/workflows/test-system.yml`:553 on 2026-01-20 14:25_

Can we add a comment explaining why this is needed?

---

_@zanieb reviewed on 2026-01-20 14:25_

---

_@zanieb reviewed on 2026-01-20 14:26_

---

_Review comment by @zanieb on `.github/workflows/test-system.yml`:527 on 2026-01-20 14:26_

Does it make sense to include the patch version here? Doesn't that mean when new security releases are out our test will break?

---

_Review comment by @EliteTK on `.github/workflows/test-system.yml`:527 on 2026-01-20 14:30_

Including the patch hasn't broken so far for this specific test (as in, this isn't a new addition), and 3.9 doesn't receive security patches any more. I think the bigger risk is that chocolatey just entirely drops it. But that could be the case with any version we pick.

---

_@EliteTK reviewed on 2026-01-20 14:30_

---
