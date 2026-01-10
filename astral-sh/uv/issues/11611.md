```yaml
number: 11611
title: Support --compile-bytecode for only the installed packages
type: issue
state: closed
author: shixuan-fan
labels: []
assignees: []
created_at: 2025-02-19T01:30:09Z
updated_at: 2025-03-14T06:47:01Z
url: https://github.com/astral-sh/uv/issues/11611
synced_at: 2026-01-10T03:50:31Z
```

# Support --compile-bytecode for only the installed packages

---

_Issue opened by @shixuan-fan on 2025-02-19 01:30_

### Summary

Today when we do `--compiled-bytecode`, the entire `site-packages` folder is compiled. This is not always desirable because we have our own managed download/install pipeline and each worker handles its own install and potentially uninstalls. During our migration to uv, we found out that when uninstall happens, the other installs with --compile will fail because a path in site-packages no longer exists during the installation:

```
Installed 1 package in 0.46ms
DEBUG Starting 16 bytecode compilation workers
DEBUG Released lock at `/tmp/uv-f641cf93176e3e54.lock`
error: Failed to bytecode-compile Python file in: /usr/lib/python3.11/site-packages
  Caused by: Failed to list files in `site-packages`
  Caused by: IO error for operation on /usr/lib/python3.11/site-packages/importlib_metadata-8.5.0.dist-info: No such file or directory (os error 2)
  Caused by: No such file or directory (os error 2)
```

This behavior is different from pip so an option to only compile the installed packages, which is pip-compatible, would be very helpful for our drop-in replacement! :)

### Example

_No response_

---

_Label `enhancement` added by @shixuan-fan on 2025-02-19 01:30_

---

_Comment by @zanieb on 2025-02-19 03:39_

cc @konstin 

---

_Label `enhancement` removed by @konstin on 2025-02-19 08:47_

---

_Label `needs-mre` added by @konstin on 2025-02-19 08:47_

---

_Comment by @konstin on 2025-02-19 08:48_

I haven't seen this error before, can you share commands that reproduce this problem?

---

_Comment by @charliermarsh on 2025-02-19 12:18_

It sounds like the issue is that they have multiple workers operating on the environment concurrently. Each worker tries to compile the entire site-packages, including packages that might be uninstalled by another worker. The ask is to only compile bytecode for relevant packages. We could also just make this robust to concurrent manipulation and treat this as non-fatal.

---

_Comment by @konstin on 2025-02-19 12:38_

Are those download/install commands separate from uv? We should still be holding an environment lock when bytecode compiling that prevents parallel modification.

---

_Comment by @shixuan-fan on 2025-02-19 16:42_

Our use case is special because for security reasons we have to run these uv commands from a sandbox (e.g. NsJail). Each sandbox has a tmpfs mounted to their `/tmp` location. This means these uv commands cannot see each other's lock because the `/tmp` location is not shared.

Even if the uv commands share the `/tmp` and lock works, this does not seem efficient because ideally if these commands are installing different packages with `--compile-bytecode` flag, they should not block each other. This can be achieved by only compiling for the installed package, similar to what pip does.

---

_Comment by @zanieb on 2025-02-19 18:20_

Yet another reason we should be co-locating those locks, cc @charliermarsh 

> this does not seem efficient because ideally if these commands are installing different packages with --compile-bytecode flag, they should not block each other

It's not safe to install into an environment concurrently. Even if the packages are different, there can be transitive overlap. And even if there's not transitive overlap, Python packages can install into arbitrary conflicting paths in your environment.


---

_Comment by @charliermarsh on 2025-02-19 18:34_

Yeah, we could consider colocating the lock in the system environment. That seems reasonable (if we have write access).

I think it would also be fine to make these robust to deletion...

---

_Comment by @shixuan-fan on 2025-02-19 18:44_

Out pipeline looks like:

```
Solve (pip dry run) -> a list of wheels -> pipeline for wheel 1 -> download -> install
                                        -> pipeline for wheel 2...
```

The transitive overlap is unlikely for us. We are using `--no-deps` to install each package separately, because the environment that uv runs also don't have network access so we are feeding it wheel files. The package to install is determined by a solve process which today we are still using pip because uv does not have `--report` equivalent yet.

The arbitrary conflicting paths is interesting, and IIRC there are some crypto libs that have this issue. I don't think there is a safe way to install it regardless how we do the install because to my best understanding it is always last-writer-win. I don't think it is the installer's responsibility to handle this not-working scenarios.

---

_Comment by @zanieb on 2025-02-19 18:51_

Interesting. Why not just compile bytecode at the end?

> I don't think there is a safe way to install it regardless how we do the install because to my best understanding it is always last-writer-win. I don't think it is the installer's responsibility to handle this not-working scenarios.

It's the difference between two partially installed packages (with arbitrarily mixed overrides) and one fully installed package with another partially overridden. Unfortunately, this is something we need to consider as some packages do this intentionally.

---

_Comment by @shixuan-fan on 2025-02-19 19:04_

> Why not just compile bytecode at the end?

Great minds think alike :-) That's what we are now considering. The pipeline install does the no-compile install, and have a final "compiler". 

Is there a uv command that compiles the entire site-packages for a given python env via `--python`? What I was considering is to do a `uv pip install --compile` on any one package that triggers the compile all. This does not seem very clean to me so trying to see if uv experts have some other suggestions.

---

_Comment by @zanieb on 2025-02-19 19:04_

https://github.com/astral-sh/uv/pull/11633 isn't what you asked for, but seems like it would unblock you.

---

_Comment by @zanieb on 2025-02-19 19:05_

We don't have a command for it no, it does seem fairly reasonable (and easy) to expose as a "plumbing command" though.

We already have a copy of it in-tree https://github.com/astral-sh/uv/blob/41cd4bee5861cdda4d826c29cd413b97638186cb/crates/uv-dev/src/compile.rs

---

_Comment by @shixuan-fan on 2025-02-19 19:06_

> https://github.com/astral-sh/uv/pull/11633 isn't what you asked for, but seems like it would unblock you.

Thanks. Let me cherry-pick and test :)

---

_Comment by @shixuan-fan on 2025-02-20 06:06_

Unfortunately I don't think the patch helps :( I'm not a rust expert, but looks like we are doing the file existence check at the entrance of the producer, but the file can be deleted afterwards. I wonder if we need to do the following:

- In producer, we also allow `if entry.metadata()?.is_file() && entry.path().extension().is_some_and(|ext| ext == "py")` to fail with OS error. Maybe we already handle it and it's just my lack of understanding in rust.
- Should we do the same on the consumer side? It's possible that when consumer reads the file, they no longer exists. Maybe somewhere in https://github.com/astral-sh/uv/blob/a08c2ead6d58001cccccd4a40f96b43501b985f8/crates/uv-installer/src/compile.rs#L339.

I'll try to see if I could get a portable repro from linux, but it might be hard given the environment we use.

---

_Label `needs-mre` removed by @konstin on 2025-02-20 08:46_

---

_Comment by @konstin on 2025-02-20 08:54_

> Is there a uv command that compiles the entire site-packages for a given python env via `--python`? What I was considering is to do a `uv pip install --compile` on any one package that triggers the compile all. This does not seem very clean to me so trying to see if uv experts have some other suggestions.

Usually, the uv steps happen sequentially, and you'd add `--compiled-bytecode` to the last command, so that everything is compiled on (container) startup.

---

_Comment by @zanieb on 2025-02-20 14:21_

Hm I was going off this error "Caused by: Failed to list files in `site-packages`"

Was there another error?

---

_Comment by @zanieb on 2025-02-20 14:29_

I think you're right the `metadata()` call could fail too, though it's an even-tighter race condition. I added handling for that in https://github.com/astral-sh/uv/pull/11633/commits/860c9a2e5261887560a31a4ca8e71d43e2916070

I'm not sure what happens if the worker fails to compile a file. It seems like it'd ignore it

https://github.com/astral-sh/uv/blob/a6602ad416a922060efb3c5f19daf4977c50ace3/crates/uv-installer/src/pip_compileall.py#L61-L65

---

_Comment by @konstin on 2025-02-20 14:38_

Yeah, mirroring pip we try to compile all `.py` files while ignoring all compilation errors, the error sources should only be in the rust source where we read directories and entry metadata.

---

_Comment by @shixuan-fan on 2025-02-20 19:04_

The second patch works for us (or at least works when I tested for ~10 times locally lol). Will do more CI/CD and proper testing. Thanks a lot for the quick workaround!

---

_Comment by @zanieb on 2025-02-20 19:27_

Thanks for giving it a test!

---

_Comment by @shixuan-fan on 2025-02-21 21:11_

Not urgent, but do you happen to know when could we expect 0.6.3? If it is soon enough then we don't have to create our own patched version :) Based on the release cadence I observe in this repo it seems pretty frequent.

---

_Comment by @charliermarsh on 2025-02-21 22:24_

I’d expect we’ll release no later than Monday.

---

_Comment by @charliermarsh on 2025-03-14 00:40_

@shixuan-fan -- Do you think we can close this one out?

---

_Comment by @shixuan-fan on 2025-03-14 06:47_

Yes. Closing. Thanks a lot for the quick turnaround!

---

_Closed by @shixuan-fan on 2025-03-14 06:47_

---
