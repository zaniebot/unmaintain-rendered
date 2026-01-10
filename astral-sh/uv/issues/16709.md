```yaml
number: 16709
title: "`uv pip compile` does not install a python interpreter if a suitable one isn't found"
type: issue
state: open
author: mikaylathompson
labels:
  - bug
assignees: []
created_at: 2025-11-12T17:40:03Z
updated_at: 2025-12-23T00:34:45Z
url: https://github.com/astral-sh/uv/issues/16709
synced_at: 2026-01-10T03:11:35Z
```

# `uv pip compile` does not install a python interpreter if a suitable one isn't found

---

_Issue opened by @mikaylathompson on 2025-11-12 17:40_

### Summary

A user may expect `uv pip compile --python 3.14` to install a 3.14 interpreter if one can't be found, for instance, but it will fall back to another version after attempting to find one instead ([in this code](https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/pip/compile.rs#L293-L319)).

Additionally, the behavior with **both** `--python` and `--python-version` flags is different than if either of them are provided individually.

Reproducible example:
```
$ uv python uninstall 3.14
...
$ echo "anyio" > requirements.in
$ uv pip compile --python-version 3.14 requirements.in
warning: The requested Python version 3.14 is not available; 3.12.9 will be used to build dependencies instead.
Resolved 3 packages in 56ms
...
$ uv pip compile --python 3.14 requirements.in
warning: The requested Python version 3.14 is not available; 3.12.9 will be used to build dependencies instead.
Resolved 3 packages in 18ms
...
$ uv pip compile --python 3.14 --python-version 3.14 requirements.in
error: No interpreter found for Python 3.14 in virtual environments, managed installations, or search path
```

The closely related behavior has been fixed for `uv pip install` and `uv pip sync`. In that case, if a user was using `--target` or `--prefix` (and thus a venv couldn't be assumed/used), uv wouldn't install an interpreter. That was changed in #16694, and this issue is a follow-up for the `pip compile` case.

### Platform

macOS 24.6 arm64

### Version

uv 0.9.3 (83635a6c4 2025-10-15)

### Python version

_No response_

---

_Label `bug` added by @mikaylathompson on 2025-11-12 17:40_

---

_Assigned to @EliteTK by @EliteTK on 2025-12-18 14:29_

---

_Comment by @EliteTK on 2025-12-22 14:30_

The reason for the different behaviour between one, the other, and both is that `--python 3.14` is treated as `--python-version 3.14` if `--python-version` isn't passed explicitly. So the first two test cases are equivalent:

<https://github.com/astral-sh/uv/blob/137edcf239f965225c72b9d9ec71dbeab94683bb/crates/uv/src/commands/pip/compile.rs#L182-L194>

The code there seems to indicate that allowing falling back to a different version to build any wheels which must be built to get metadata is acceptable. But not sure if this is still the case.

The only way to request that uv uses a specific version for builds is using `--python` and `--python-version` together to avoid triggering this fallback. In this case, this code:

<https://github.com/astral-sh/uv/blob/137edcf239f965225c72b9d9ec71dbeab94683bb/crates/uv/src/commands/pip/compile.rs#L304-L332>

Will not fall back to `find_best` and will fail.

I think making it install the version if a specific one is requested seems like an unobjectionable improvement. Regarding the fallback logic, I will ask around...

---

_Comment by @zanieb on 2025-12-22 15:30_

> The code there seems to indicate that allowing falling back to a different version to build any wheels which must be built to get metadata is acceptable. But not sure if this is still the case.

It's acceptable, but not ideal, we should continue instead of failing if Python downloads are disabled though. The logic here precedes our ability to download Python distributions.

---

_Comment by @EliteTK on 2025-12-22 15:46_

For the case that an explicit `--python` is set and didn't get converted into `--python-version` I have pushed #17216 which uses `find_or_download` for that.

The case you're describing happens when only `--python-version` is set or `--python` gets converted into `--python-version`. In this case you're saying that if python downloads and building is enabled[^1] we should use `find_or_download` and otherwise use `find_best` and warn?

I think one option is to just make the code here:

https://github.com/astral-sh/uv/blob/137edcf239f965225c72b9d9ec71dbeab94683bb/crates/uv/src/commands/pip/compile.rs#L182-L194

Not unset `python` when it sets `python_version`.

[^1]: This begs the question, should we even be trying to get an interpreter at all if building is disabled? Do we need it for anything?

---

_Comment by @zanieb on 2025-12-22 15:58_

I think the nuance is that, if we cannot find a download for the target interpreter, we still need to use the `find_best` logic instead of failing. 

Re your footnote, we need an interpreter to determine the markers for the resolution. We technically could do this without an interpreter, but the interpreter is the most reliable way to do so.

---

_Comment by @EliteTK on 2025-12-22 16:19_

So I'll follow my current PR with another which, in case downloads are disabled falls back to `find_best` for `--python` (when it doesn't turn into `--python-version`).

But I guess the question still is, if someone passes a `--python` and it turns into `--python-version` the previously hard requirement on that specific version gets turned into a soft one. If we just don't set `python` to `None` (after the change mentioned above) I think we get the intended behaviour where `--python` will implicitly set `--python-version` but an explicit `--python` will still use the correct version when downloads are enabled rather than just falling back to the "best". I think that would address the situation here and we could drop that caveat in the comment?

---

_Comment by @zanieb on 2025-12-22 16:40_

I'm honestly having a hard time following what you've described. It's not sufficient to special case "downloads are disabled", because we also need to handle "download does not exist".

---

_Comment by @EliteTK on 2025-12-22 17:06_

Okay, fair enough, also handling "download does not exist" shouldn't be a problem.

Regarding the other part, I'll try to clarify:

Currently:

* If you pass `--python`, and:
* If you don't pass `--python-version`, and:
* `PythonVersion::from_str` succeeds (meaning that it is a "simple Python version request"), then:
* We treat this as if you had passed `--python-version` instead of `--python`, meaning that:
* We will satisfy the `--python` request with `find_best` instead of `find_or_download`[^1].

Is this behaviour correct? If not, I can think of two good alternatives:

1. Passing only `--python` _also_ implies `--python-version` if it is a simple version request, meaning that to find the requested interpreter we still use `find_or_download`[^1].
2. Passing only `--python-version` uses `find_or_download`[^1] instead of `find_best`.

[^1]: With the PR for handling "download is disabled" and "download does not exist" then the behaviour would instead be: `find_or_download` and if downloads are disabled or downloading fails then warn and fall back to `find_best`.

---

_Comment by @zanieb on 2025-12-22 21:46_

> find_or_download and if downloads are disabled or downloading fails then warn and fall back to find_best

Wouldn't this mean that we perform interpreter discovery twice? e.g., we could traverse the `PATH` twice? It sounds a little sloppy performance-wise, though we could say it's niche? I think some distros (e.g., Debian) disable Python downloads in uv by default though.

---

_Comment by @zanieb on 2025-12-22 21:47_

> Is this behaviour correct?

I don't think it's "incorrect" but using a simple version like `--python 3.12` is by far the common case so I think if we want to improve the behavior we should change something here. Other than my caveat at https://github.com/astral-sh/uv/issues/16709#issuecomment-3684286254, I think that your proposed approach conceptually makes sense?

---

_Comment by @zanieb on 2025-12-22 21:55_

I think instead of just discussing which internal "method" we're going to invoke to discover an interpreter it might be worth talking about concrete user outcomes? 

Aren't we really just talking about:

1. When an interpreter is not found, error
2. When an interpreter is not found, use an alternative one with overridden version markers

Where (1) is implied by `--python` unless it is coerced to mean `--python-version`? And (2) is implied by `--python-version`?

The fix we're discussing here is mostly whether "when an interpreter is not found" includes attempting a download, right? I'd expect us to attempt a download in either scenario before moving to the next step.


---

_Comment by @EliteTK on 2025-12-22 21:58_

> Wouldn't this mean that we perform interpreter discovery twice? e.g., we could traverse the `PATH` twice?

Yes, it does. Well I did consider adding a `find_or_download_or_best`, maybe an alternative option is to cache this information...?

---

_Comment by @zanieb on 2025-12-22 21:58_

Re https://github.com/astral-sh/uv/issues/16709#issuecomment-3684286254 maybe the fix is just to make it possible for `find_best` to perform downloads?

---

_Comment by @zanieb on 2025-12-22 22:00_

`find_best` is only used once, it seems fine to just change it?

We do cache interpreter queries when we can, which is the most expensive part. Caching the rest doesn't seem worth the complexity.

---

_Comment by @EliteTK on 2025-12-22 22:02_

It wouldn't be applicable in the `--python` case, since we want to prioritise the version we asked for unless we have concluded we can't find or download it, and only then fall back to `find_best`.

But regardless, as a separate issue, `find_best` not being able to download is itself a problem, and I think addressing that would make sense for the `--python-version` on a system with no installed python case.

---

_Comment by @EliteTK on 2025-12-22 23:13_

Regarding <https://github.com/astral-sh/uv/issues/16709#issuecomment-3684302324>. I was under the impression that we probably don't want to attempt a download as a first resort for the `--python-version` case but we do want to attempt a download as a first resort for the `--python` case?

If we want to attempt a download as a first resort for either case then the suggested change to `find_best` makes sense and the whole fallback thing doesn't matter, because we can just implement the right logic in `find_best` and use it for both cases.

---

_Comment by @zanieb on 2025-12-22 23:25_

> we probably don't want to attempt a download as a first resort for the --python-version case but we do want to attempt a download as a first resort for the --python case?

What do you mean by "as a first resort"? We should follow our normal logic (which is, to only download when there's not an appropriate match for the request). We don't ever eagerly download.

---

_Comment by @zanieb on 2025-12-22 23:27_

I don't see why we should use, e.g., 3.13 when `--python-version 3.12` was requested and downloads are enabled though. 

---

_Comment by @EliteTK on 2025-12-23 00:28_

Re first resort: Yes I mean the first action we take if the request can't be satisfied with what we can find already installed.

Regarding `--python-version`. I was not aware before, but there is precedent to making it download an interpreter if it can't find a matching one. For example, `uv sync` in a project which requests 3.12 will download 3.12 unless I pass `--python` and specify another newer version I already have installed.

I think we could do something like this (for each line, 3.14 is the installed default version):

| `--python` | `--python-version` | Version downloaded | Wheels built with | Picked packages must support |
| ----------:| ------------------:| ------------------:| -----------------:| ----------------------------:|
|            |                    |                    |              3.14 |                         3.14 |
|       3.13 |                    |               3.13 |              3.13 |                         3.13 |
|            |               3.13 |               3.13 |              3.13 |                         3.13 |
|       3.13 |               3.10 |               3.13 |              3.13 |                         3.10 |

Furthermore, if I use `--python` to request an older version than `--python-version` we should complain like `uv sync` does in similar scenarios.

But to explain why it might make sense _not_ to download the `--python-version` requested interpreter if it can't be found: `--python-version` isn't used for building, it's only used by the resolver to ensure we pick packages which support the specified version. We allow you to specify `--python` and `--python-version` together and I think it would be surprising if both were downloaded in that case (since the version specified by `--python-version` isn't going to get ran at any point during the process). As we support that, it makes some sense to not be particularly strict about using the proper version when `--python-version` is specified on its own.

But that kind of design change should probably be applied globally (e.g., we could also apply this logic to `uv sync` etc).

(Edited for clarity)

---
