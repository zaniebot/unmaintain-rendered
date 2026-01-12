```yaml
number: 8933
title: "Handle sigterm calls, fixes #6724"
type: pull_request
state: merged
author: shaneikennedy
labels:
  - bug
assignees: []
merged: true
base: main
head: shane/sigterm
created_at: 2024-11-08T11:45:07Z
updated_at: 2024-11-12T02:48:22Z
url: https://github.com/astral-sh/uv/pull/8933
synced_at: 2026-01-12T16:08:33Z
```

# Handle sigterm calls, fixes #6724

---

_@shaneikennedy_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR builds off of https://github.com/astral-sh/uv/pull/6738 to fix #6724 (sorry for the new PR @charliermarsh I didn't want to push to your branch, not even sure if I could). The reason the original PR doesn't fix the issue described in #6724 is because the fastapi is ran in the project context (as I assume a lot of use cases are). This PR adds an extra commit to handle the signals in the project/run.rs file

~It also addresses the comment [here](https://github.com/astral-sh/uv/pull/6738/files#r1734757548) to not use the tokio ctrl-c method since we are now handling SIGINT ourselves~ update, tokio handles SIGINT in a platform agnostic way, intercepting this ouselves makes the logic more complicated with windows, decided to leave the tokio ctrl-c handler

~[This comment](https://github.com/astral-sh/uv/pull/6738/files#r1743510140) remains unaddressed, however, the Child process does not have any other methods besides kill() so I don't see how we can "preserve" the interrupt call :/  I tried looking around but no luck.~ updated, this PR is reduced to only handling SIGTERM propagation on unix machines, and the sigterm call to the child is preserved by making use of the nix package, instead of relying on tokio which only allowed for `kill()` on a child process

## Test Plan

I tested this by building the docker container locally with these changes and tagging it "myuv", and then using that as the base image in uv-docker-example, (and ofc following the rest of the repro issues in #6724. In my tests I see that ctrl-c in the docker-compose up command exits the process almost immediately 👍 


---

_Comment by @shaneikennedy on 2024-11-08 11:49_

Also yeah, windows does not have SIGTERM, need some guidance here as I've never built for windows 😬 

---

_Comment by @bluss on 2024-11-08 12:06_

Here's one issue, provided as a test case.

Run a regular python (no uv involved) and press ctrl-C twice (just to show that the prompt sticks around):

```
python
Python 3.10.12 (main, Sep 11 2024, 15:47:36) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
KeyboardInterrupt
>>> 
KeyboardInterrupt
>>>
```

With `uv 0.4.30` same effect (good):

```
uv run -p 3.10 python
Python 3.10.14 (main, Jul 25 2024, 22:44:48) [Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
KeyboardInterrupt
>>> 
KeyboardInterrupt
>>>
```

With this PR, in current state (uv run is killed by Ctrl-C, which is a change):

```
./target/debug/uv run -p 3.10 python
Python 3.10.14 (main, Jul 25 2024, 22:44:48) [Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
(Process exited with error) 
```

(Edit: Direct run of the binary, not cargo run, to avoid cargo being involved in the test case. Didn't change the result.)


---

_Comment by @shaneikennedy on 2024-11-08 12:17_

@bluss good catch, I think as @cr-klarna points out in the original PR a simple fix isn't likely since .kill() is not what we want here, we want to propagate the signals down and this solution which built off of @charliermarsh 's original basically overrides the signal

---

_Review comment by @bluss on `crates/uv/src/commands/project/run.rs`:1016 on 2024-11-08 12:30_

`uv run` should preferably preserve the exit code from the child, not assume this was an error exit. (I'm sorry for more comments if they are superfluous, but it's better to get the issues out in the sun.)

Example application for this use case: mkdocs serve exits gracefully/with success after Ctrl-C.

---

_@bluss reviewed on 2024-11-08 12:30_

---

_Comment by @shaneikennedy on 2024-11-08 13:46_

@bluss WIP here but using cfg macros to react to sigterm based on platform, and also dropping handling of SIGINT and leaving that to tokio since that seems to be working fine

---

_Comment by @shaneikennedy on 2024-11-08 13:50_

Going to be honest I don't know windows, I might ask to punt the windows sigterm implementation so that people on unix systems can get unblocked here and let someone who actually knows windows to help out 😂 

---

_Comment by @cr-klarna on 2024-11-08 15:20_

Tested with a simple case of registering handlers and passing signals with `kill`. Confirmed that for SIGTERM the behavior seems expected and I tested with @shaneikennedy some cases we had. One thing stands out still that it seems that tokio really wants to have an interactive context before passing the SIGINT signals. This is a problem but I am not convinced it is uv's to solve, this seems more due to a bug/design decision in tokio.

@bluss I also tested the mkdocs serve and it exited gracefully after ^C was sent from the interactive context as indicated by the chance to print out the graceful shutdown message. Obvs good to check yourself ofc

```INFO    -  Building documentation...
INFO    -  Cleaning site directory
INFO    -  Documentation built in 1.97 seconds
INFO    -  [16:18:35] Watching paths for changes: 'docs', 'mkdocs.public.yml'
INFO    -  [16:18:35] Serving on http://127.0.0.1:8000/uv/
^CINFO    -  Shutting down...```

---

_Comment by @overfl0 on 2024-11-08 15:28_

> @bluss I also tested the mkdocs serve and it exited gracefully after ^C was sent from the interactive context as indicated by the chance to print out the graceful shutdown message. Obvs good to check yourself ofc

@cr-klarna My understanding is that `echo $?` is supposed to return `0` for mkdocs, when pressing Ctrl+C, which will not happen when run through uv, as it returns with `ExitStatus::Failure` hardcoded.
(although I have NOT tested that - I'm just basing my opinion on what has been written above)

---

_Comment by @cr-klarna on 2024-11-08 15:30_

@overfl0 no I get status 0 and this is because that error returned is not propagated through the parent (yet) it seems. This seems to be one of the other broad design decisions that was hinted at in the other PR

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 13:46_

---

_Label `bug` added by @charliermarsh on 2024-11-09 13:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1021 on 2024-11-09 17:14_

Is it "fine" / necessary for us to be calling `.wait()` twice?

---

_@charliermarsh reviewed on 2024-11-09 17:14_

---

_@charliermarsh approved on 2024-11-09 17:23_

---

_@shaneikennedy reviewed on 2024-11-09 17:23_

---

_Review comment by @shaneikennedy on `crates/uv/src/commands/project/run.rs`:1021 on 2024-11-09 17:23_

Not a rust expert would appreciate your guidance here, from what I see inside of `wait()` it's not expensive to call it twice and it makes the logic in the select! block easier to read. If I do something like let status = select! {...} each brand needs to return the same type, but `terminate_process` doesn't actually return anything, so I'd need to fake a return which also seems unnecessary. wdyt?

---

_@charliermarsh reviewed on 2024-11-09 17:24_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1021 on 2024-11-09 17:24_

It looks like it _is_ guaranteed to return the same result both times.

---

_Review requested from @zanieb by @zanieb on 2024-11-09 17:26_

---

_Comment by @charliermarsh on 2024-11-09 17:27_

I made some minor changes. @shaneikennedy -- do you want to re-test before I merge?

---

_@charliermarsh reviewed on 2024-11-09 17:28_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1021 on 2024-11-09 17:28_

I went with returning the result directly on unix and only calling `let status = handle.wait().await?;` on other platforms, since it didn't seem too difficult and makes the control flow a bit clearer IMO.

---

_Comment by @shaneikennedy on 2024-11-09 17:36_

> I made some minor changes. @shaneikennedy -- do you want to re-test before I merge?

Since testing this initially I have somehow completely borked my local docker setup https://github.com/astral-sh/uv/issues/8974

I can't test this atm unfortunately, the changes don't look drastically different to me though

---

_@shaneikennedy reviewed on 2024-11-09 17:37_

---

_Review comment by @shaneikennedy on `crates/uv/src/commands/project/run.rs`:1021 on 2024-11-09 17:37_

Nice, agreed this looks better 👍 

---

_Comment by @charliermarsh on 2024-11-09 17:44_

Sounds good. I think @zanieb wants to take a look so will wait for that review too.

---

_Comment by @shaneikennedy on 2024-11-09 19:08_

Alright docker is back in order on my machine, tested by building the docker image `docker build . -t myuv` and then using that image in the uv-docker-example Dockerfile as the base, and finally switching the uv-docker-example run command to run fastapi with uv (as described in the original issue), and it exits basically immediately 👍  @charliermarsh 

---

_Comment by @shaneikennedy on 2024-11-11 19:54_

@zanieb lmk if there's anything else I can do here, happy to push any fixes/changes asap if need be 👀 🙏 

---

_@zanieb approved on 2024-11-12 02:48_

Thanks! This seems like a reasonable incremental improvement. We'll need to follow up with more changes, like forwarding other signals and handling things properly on Windows.

---

_Merged by @zanieb on 2024-11-12 02:48_

---

_Closed by @zanieb on 2024-11-12 02:48_

---
