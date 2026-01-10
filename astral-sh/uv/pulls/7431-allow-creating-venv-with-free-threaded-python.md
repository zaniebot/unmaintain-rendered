```yaml
number: 7431
title: Allow creating venv with free-threaded python builds
type: pull_request
state: merged
author: bschoenmaeckers
labels: []
assignees: []
merged: true
base: main
head: venv-free-threaded
created_at: 2024-09-16T15:51:43Z
updated_at: 2024-09-24T04:28:03Z
url: https://github.com/astral-sh/uv/pull/7431
synced_at: 2026-01-10T12:53:47Z
```

# Allow creating venv with free-threaded python builds

---

_Pull request opened by @bschoenmaeckers on 2024-09-16 15:51_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

closes #4828

First iteration for an implementation. I need to add more tests but wanted your opinion on the implementation first.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Currently tested using the following command but will add tests shortly:

```console
D:\repo\uv> cargo run venv -p 3.13t && .venv\Scripts\python.exe
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.52s
     Running `target\debug\uv.exe venv -p 3.13t`
Using Python 3.13.0rc1 interpreter at: C:\Users\bschoen\AppData\Local\Programs\Python\Python313\python3.13t.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
Python 3.13.0rc1 experimental free-threading build (tags/v3.13.0rc1:e4a3e78, Jul 31 2024, 21:06:58) [MSC v.1940 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```


---

_Comment by @bschoenmaeckers on 2024-09-16 18:08_

Working on the unix implementation now.

---

_Assigned to @zanieb by @zanieb on 2024-09-16 18:10_

---

_Comment by @bschoenmaeckers on 2024-09-16 19:27_

> Working on the unix implementation now.

Tested it on linux using the `quay.io/pypa/manylinux2014_x86_64` docker image and I seems to work correctly already. Going to write some tests next.

---

_Comment by @bschoenmaeckers on 2024-09-16 20:06_

I find it quiet hard to add a test without a free-threaded build available in the CI. So maybe we should add a full integration test later when the downloadable build is available.

---

_Comment by @bschoenmaeckers on 2024-09-17 08:26_

Should version requests like `>=3.13t`, `>=3.13t,<3.14t` or `>=3.12,<3.14t` be valid?

---

_Comment by @zanieb on 2024-09-17 13:33_

> Should version requests like `>=3.13t`, `>=3.13t,<3.14t` or `>=3.12,<3.14t` be valid?

Maybeee. It seems safe to add support for that later if people need it. 

> I find it quiet hard to add a test without a free-threaded build available in the CI. So maybe we should add a full integration test later when the downloadable build is available.

You can probably add test coverage in `crates/uv-python/src/lib.rs`, we support creating mock interpreters there.


---

_Comment by @zanieb on 2024-09-17 13:49_

Oh also regarding an integration test, is there a Docker image with a free-threaded build? or a system that has it packaged? You can create a new `integration-test` CI job that does whatever so we get some real-world coverage.

---

_Comment by @bschoenmaeckers on 2024-09-18 14:07_

I've added a smoke test in the ci using a pre-compiled python 3.13 [package](https://launchpad.net/%7Edeadsnakes/+archive/ubuntu/ppa/+packages) installed using apt-get on a bare ubuntu instance. Furthermore I added a mocked interpreter test.

---

_@zanieb reviewed on 2024-09-18 14:08_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:608 on 2024-09-18 14:08_

We could probably set a more aggressive timeout here.

```suggestion
    timeout-minutes: 5
```

---

_@zanieb reviewed on 2024-09-18 14:18_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 14:18_

Doing some internal debate about this right now — this is sort of the "easiest" way to add support but it might be better to added a `freethreaded: bool` to each relevant variant instead? Like `FreeThreaded(Any)` shouldn't be allowed nor `FreeThreaded(FreeThreaded(...))` right? Want to share some more about why you went with this pattern?

---

_@zanieb reviewed on 2024-09-18 14:21_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1482 on 2024-09-18 14:21_

e.g., if you didn't use a recursive enum we could just generate the names properly in `VersionRequest::default_names` instead of mutating the names here.

---

_Review comment by @zanieb on `crates/uv/tests/python_find.rs`:460 on 2024-09-18 14:22_

Nit: We should probably use `3.12` here so it doesn't collide with our error message for unsupported Python versions if we change where that's enforced.

---

_@zanieb reviewed on 2024-09-18 14:22_

---

_Review comment by @bschoenmaeckers on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 14:26_

It was at the time of implementing the easiest option to wrap every variant with a FreeThreaded version because it didn't felt right to clutter every variant with yet another flag. You are right that that `FreeThreaded(Any)` does not make sense but the `FreeThreaded(FreeThreaded(...))` should not be possible for the parser to generate, even though it would not be a very big problem because it would behave like a regular FreeThreaded(...) variant. But I understand the debate and would happily switch to a extra flag on every variant if you want me to.

---

_@bschoenmaeckers reviewed on 2024-09-18 14:26_

---

_@bschoenmaeckers reviewed on 2024-09-18 14:37_

---

_Review comment by @bschoenmaeckers on `crates/uv/tests/python_find.rs`:460 on 2024-09-18 14:37_

Oops you're right, this should be 3.12t.

---

_@zanieb reviewed on 2024-09-18 14:51_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 14:51_

The parser can't generate it but this is a "public" enum so someone could construct that variant — which is a bit awkward. I think I have a preference for adding a boolean to the variants, it looks like it would make some of the downstream logic simpler. It is a bit sad that we'll need to add it everywhere we construct a `VersionRequest` manually but I'm not sure what else to do there. As a small note, it may need to be `Option<bool>` for cases in which the free-threaded build is fine, e.g., and can be used if it's first on the `PATH`.

---

_@bschoenmaeckers reviewed on 2024-09-18 14:57_

---

_Review comment by @bschoenmaeckers on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 14:57_

Fine by me. How would you parse a `3.13` request? Does the user allow a free-threaded build or not?

---

_@zanieb reviewed on 2024-09-18 15:00_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 15:00_

I'm not sure.

cc @henryiii / @carljm who may have helpful opinions on that.

---

_@zanieb reviewed on 2024-09-18 15:26_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 15:26_

I think generally no, unless it's the only one available — like we do for pre-releases #7300

We might want to be more strict than that but I'm not sure.

---

_@henryiii reviewed on 2024-09-18 18:40_

---

_Review comment by @henryiii on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 18:40_

I think you want to make sure this can change for a future Python version, but yes, I think currently you want free-threaded to be opt-in. In the future, if it works well, it might be opt-out instead, maybe as soon as 3.15 (which may be called 3.27).

---

_@zanieb reviewed on 2024-09-18 18:41_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 18:41_

What if you have a `python` executable at the front of your path and it's free-threaded?

---

_@henryiii reviewed on 2024-09-18 18:43_

---

_Review comment by @henryiii on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 18:43_

If it's called `python` or `python3.13`, then yes, but usually it's only called `python3.13t`?

---

_@henryiii reviewed on 2024-09-18 18:45_

---

_Review comment by @henryiii on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 18:45_

Same as pypy. If there's a `python` that's actually pointing at `pypy`, then that should be picked up, otherwise pypy should only be opt-in?

---

_@zanieb reviewed on 2024-09-18 18:45_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:140 on 2024-09-18 18:45_

Unfortunately people do weird things and we have to think about these edge-cases

---

_Comment by @zanieb on 2024-09-20 20:39_

I think this is failing because it's not searching for the executable name properly. I'm going to tweak the `possible_names` and `default_names` code — it' really unwieldy (not from this pull request).

---

_Comment by @bschoenmaeckers on 2024-09-20 21:24_

> I think this is failing because it's not searching for the executable name properly. I'm going to tweak the `possible_names` and `default_names` code — it' really unwieldy (not from this pull request).

I was thinking so as well, I did not had time to debug this properly. The only thing I knew was that it is working properly on windows.

---

_Comment by @bschoenmaeckers on 2024-09-21 11:02_

@zanieb, I fixed the tests. So it is ready for review again. 

---

_@zanieb approved on 2024-09-23 22:35_

---

_Comment by @zanieb on 2024-09-23 22:36_

Thanks! I resolved the conflicts with #7649 

---

_Merged by @zanieb on 2024-09-23 22:36_

---

_Closed by @zanieb on 2024-09-23 22:36_

---

_Comment by @basnijholt on 2024-09-23 22:42_

Awesome! Does this mean that there will be a 3.13t version available at the next release of `uv`?

---

_Comment by @zanieb on 2024-09-23 23:50_

Unfortunately not, that's much harder and can be tracked at https://github.com/indygreg/python-build-standalone/pull/326

With this, we'll just let you ask uv to use a free-threaded build you already have installed.

---

_Comment by @henryiii on 2024-09-24 04:28_

FYI, this means you can say "3.13t" and get it; you can already use uv with python3.13t as long as you give it via path. (At least I hope so, for example cibuildwheel does that)

---
