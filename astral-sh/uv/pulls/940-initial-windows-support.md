```yaml
number: 940
title: Initial windows support
type: pull_request
state: merged
author: konstin
labels:
  - windows
assignees: []
merged: true
base: main
head: konsti/initial-windows-support
created_at: 2024-01-16T15:57:22Z
updated_at: 2024-02-16T19:16:31Z
url: https://github.com/astral-sh/uv/pull/940
synced_at: 2026-01-12T16:04:18Z
```

# Initial windows support

---

_@konstin_

## Summary

First batch of changes for windows support. Notable changes:

* Fixes all compile errors and added windows specific paths.
* Working venv creation on windows, both from a base interpreter and from a venv. This requires querying `stdlib` from the sysconfig paths to find the launcher.
* Basic url/path conversion handling for windows.
* `if cfg!(...)` instead of `#[cfg()]`. This should make it easier to keep everything compiling across platforms.

## Outlook

Test summary: 402 tests run: 299 passed (15 slow), 103 failed, 1 skipped

There are various reason for the remaining test failure:
* Windows-specific colorama and tzdata dependencies that change the snapshot slightly. This is by far the biggest batch.
* Some url-path handling issues. I fixed some in the PR, some remain.
* Lack of the latest python patch versions for older pythons on my machine, since there are no builds for windows and we need to register them in the registry for them to be picked up for `py --list-paths` (CC @zanieb RE #1070).
* Lack of entrypoint launchers.
* ... likely more

---

_Comment by @konstin on 2024-01-18 16:40_

I'll split this into a PR that adds `puffin venv -p 3.x` support and one that does the initial windows support.

---

_Converted to draft by @konstin on 2024-01-18 16:40_

---

_@konstin reviewed on 2024-01-24 14:29_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:743 on 2024-01-24 14:29_

This is only expanding on the hack, but `Url::to_file_path` lowercases everything, which breaks our env var handling (unless we want to make the case insenstive).

---

_@konstin reviewed on 2024-01-24 14:29_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:1031 on 2024-01-24 14:29_

I've split this out from a panicking intergration test

---

_@konstin reviewed on 2024-01-24 14:31_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:62 on 2024-01-24 14:31_

That might be user feedback dependent, i don't know how people set up their path on windows.

---

_Marked ready for review by @konstin on 2024-01-24 14:55_

---

_Label `windows` added by @konstin on 2024-01-24 14:55_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:867 on 2024-01-24 15:07_

Given this, should we just `cfg` these tests for each platform? It seems more robust and self-documenting/

---

_Review comment by @charliermarsh on `crates/gourgeist/src/bare.rs`:136 on 2024-01-24 15:07_

Can you say a bit more here?

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/python_query.rs`:62 on 2024-01-24 15:08_

Some overlap here with @zanieb's PR, right?

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_sync.rs`:3192 on 2024-01-24 15:11_

I feel like all the changes to the test files could be a separate PR that we could commit independently of the Windows support, but not a big deal.

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:748 on 2024-01-24 15:12_

Is there a generic solution to this problem?

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:739 on 2024-01-24 15:12_

Nit: use `Cow` for this? We shouldn't need to clone here, right?

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:737 on 2024-01-24 15:13_

Is there a generic solution for this?

---

_@charliermarsh approved on 2024-01-24 15:13_

---

_@konstin reviewed on 2024-01-24 15:15_

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:136 on 2024-01-24 15:15_

The `RELATIVE_SITE_PACKAGES` have the unix value; We probably want to do a pass over the activate scripts at some point. I use the bash activator on linux and the powershell activator on windows and they both work.

---

_Comment by @charliermarsh on 2024-01-24 15:48_

Running on my Windows box, I currently get:

```
  x failed to canonicalize path `C:\Users\crmar\AppData\Local\Microsoft\WindowsApps\python.exe`
  `-> The file cannot be accessed by the system. (os error 1920)
error: process didn't exit successfully: `target\debug\puffin.exe venv` (exit code: 1)
```

---

_Comment by @charliermarsh on 2024-01-24 15:50_

Is that expected? Is there something I need to do?

---

_@charliermarsh reviewed on 2024-01-24 15:53_

@BurntSushi - Do you mind reviewing this as well given your Windows / pathing experience?

---

_Review comment by @BurntSushi on `crates/distribution-types/src/lib.rs`:867 on 2024-01-24 15:58_

I'm okay with just picking an upper bound personally. The main idea of these tests I think is that the types don't get a lot bigger without us knowing about it. But like if Unix increased by 8 bytes and thus still fell within this upper bound, I think that'd be fine. As long as it's close.

---

_Review comment by @BurntSushi on `crates/puffin-interpreter/src/interpreter.rs`:352 on 2024-01-24 16:05_

Does it make sense to just put `#[cfg(unix)]` on the `mod tests` itself? Then you can remove it from the `use` and the test below.

---

_@BurntSushi approved on 2024-01-24 16:19_

Overall LGTM. The conditional compilation that's spread out in places is a little worrying, but I think it's fine to move forward.

Does it make sense to enable Windows in CI in this PR? Or maybe that's for a follow-up.

---

_Comment by @charliermarsh on 2024-01-24 16:29_

Yeah we should def enable Windows CI.

---

_@konstin reviewed on 2024-01-24 17:05_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:62 on 2024-01-24 17:05_

yep, also with the bootstrapped python versions PR

---

_@konstin reviewed on 2024-01-24 17:09_

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:748 on 2024-01-24 17:09_

Not that i'm aware of, i only affect this one test case

---

_@konstin reviewed on 2024-01-24 17:11_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:737 on 2024-01-24 17:11_

`Url::to_file_path`, but it lower cases the env vars too

---

_Merged by @konstin on 2024-01-24 17:27_

---

_Closed by @konstin on 2024-01-24 17:27_

---

_Branch deleted on 2024-01-24 17:27_

---

_Comment by @ofek on 2024-02-16 17:34_

What was the rationale for using the `py` launcher? You won't have that on servers and the average user on a desktop is not guaranteed to have that.

---

_Comment by @zanieb on 2024-02-16 17:47_

We're going to use the registry eventually, it's just a bigger task.

---

_Comment by @ofek on 2024-02-16 17:56_

Hmm, what do you mean by using the registry?

---

_Comment by @zanieb on 2024-02-16 18:12_

https://peps.python.org/pep-0514/

---

_Comment by @ofek on 2024-02-16 18:22_

Yes but when the time comes please consider PATH in the resolution, otherwise it's very unexpected. For example, right now if you unpack a build from https://github.com/indygreg/python-build-standalone on Windows there is literally no way to use it.

---

_Comment by @zanieb on 2024-02-16 18:29_

We should consider PATH, we search for `python.exe`

---

_Comment by @ofek on 2024-02-16 18:32_

![image](https://github.com/astral-sh/uv/assets/9677399/fe6a474d-3dc4-4296-88ca-0b2d7bd63666)


---

_Comment by @MichaReiser on 2024-02-16 18:32_

Considering path might also solve the issues with pyenv-win

---

_Comment by @zanieb on 2024-02-16 18:39_

@ofek are you using an old version? I fixed searching for `python.exe` in #1381 

---

_Comment by @ofek on 2024-02-16 18:59_

![image](https://github.com/astral-sh/uv/assets/9677399/9d5197b4-2784-483f-8e2a-5199c26b01d4)


---

_Comment by @MichaReiser on 2024-02-16 19:16_

I'm sorry you're still running into this. Let's track this in https://github.com/astral-sh/uv/issues/1310

---
