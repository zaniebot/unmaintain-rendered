```yaml
number: 12168
title: Improve error message when a virtual environment Python symlink is broken
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/hint-about-broken-venv
created_at: 2025-03-14T14:02:27Z
updated_at: 2025-05-07T18:24:55Z
url: https://github.com/astral-sh/uv/pull/12168
synced_at: 2026-01-10T11:10:39Z
```

# Improve error message when a virtual environment Python symlink is broken

---

_Pull request opened by @konstin on 2025-03-14 14:02_

When removing a Python interpreter underneath an existing venv, uv currently shows a not found error:

```
error: Failed to inspect Python interpreter from active virtual environment at `.venv/bin/python3`
  Caused by: Python interpreter not found at `/home/konsti/projects/uv/.venv/bin/python3`
```

This is unintuitive, as the file for the Python interpreter does exist, it is a broken symlink that needs to be replaced with `uv venv`.

I've been encountering those occasionally, and I expect users that switch between versions a lot will, too, especially when they also use pyenv or a similar Python manager.

The new error hints at this solution:

```
error: Failed to inspect Python interpreter from active virtual environment at `.venv/bin/python3`
  Caused by: Broken symlink at `.venv/bin/python3`, was the underlying Python interpreter removed?

hint: To recreate the virtual environment, run `uv venv`
```

---

_Label `error messages` added by @konstin on 2025-03-14 14:02_

---

_Review requested from @zanieb by @konstin on 2025-03-14 14:02_

---

_Review requested from @jtfmumm by @konstin on 2025-03-14 14:02_

---

_Comment by @charliermarsh on 2025-03-14 14:08_

Nice. Can we add another newline before the hint, and remove the trailing period on the hint per the style guide?

---

_Comment by @konstin on 2025-03-14 14:18_

I'm considering removing `InterpreterError::NotFound`, do we have cases we want to permit that are not covered by `InterpreterError::BrokenSymlink`?

---

_Comment by @zanieb on 2025-04-16 16:44_

> I'm considering removing InterpreterError::NotFound

I think we need it for a case like `uv run -p /path/to/python3.12`, right? A missing file there is different than a broken symlink

---

_@zanieb reviewed on 2025-04-16 16:45_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:924 on 2025-04-16 16:45_

Isn't it important to report the broken symlink in this context too? I'm not sure how that'd look downstream. You could test with `uv venv foo && <uninstall python> && uv run -p foo`?

---

_@zanieb reviewed on 2025-04-16 16:46_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:924 on 2025-04-16 16:46_

I guess the hint could be wrong here if it's not a virtual environment?

---

_Renamed from "Hint for broken venv" to "Improve error message when a virtual environment Python symlink is broken" by @zanieb on 2025-04-16 16:47_

---

_@zanieb reviewed on 2025-04-16 16:48_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:16295 on 2025-04-16 16:48_

What if the virtual environment was found in a parent directory or specified some other way? I'm worried this hint might be misleading.

---

_Comment by @konstin on 2025-04-16 22:32_

> > I'm considering removing InterpreterError::NotFound
> 
> I think we need it for a case like `uv run -p /path/to/python3.12`, right? A missing file there is different than a broken symlink

I'm afraid I'm not following along what `/path/to/python3.12` means. If I do `uv run -p /usr/bin/python3.12 python` it works in either case. If I do `uv run -p /does/not/exist/python3.12 python` there's the current behavior and the one if we remove `InterpreterError::NotFound`.

Current behavior:

```
$ uv run -p /does/not/exist/python3.12 python
error: No interpreter found for path `/does/not/exist/python3.12` in virtual environments, managed installations, or search path
```

After removing `InterpreterError::NotFound`:

```
$ uv run -p /does/not/exist/python3.12 python
error: Failed to inspect Python interpreter from provided path at `/does/not/exist/python3.12`
  Caused by: Failed to query Python interpreter
  Caused by: failed to canonicalize path `/does/not/exist/python3.12`: No such file or directory (os error 2)
```

---

_@konstin reviewed on 2025-04-16 22:35_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_compile.rs`:16295 on 2025-04-16 22:35_

Should we make the hint more generic, like "hint: The Python interpreter seems to have been uninstalled, consider recreating the virtual environment"?

---

_Comment by @zanieb on 2025-04-16 22:39_

I see. I think that might cause other problems for discovery, I'd need to look carefully at the downstream effects. I can do that though..

---

_@zanieb reviewed on 2025-04-16 22:40_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:16295 on 2025-04-16 22:40_

That seems more correct. You could sniff for `pyenv.cfg` too? If this was handled in the caller instead (as it sort of was before this PR?) you would have information about whether it's a virtual environment or not?

---

_@konstin reviewed on 2025-04-16 22:44_

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:924 on 2025-04-16 22:44_

For this case, I get the same behavior with and without this PR:

```
$ uv venv botched && rm botched/bin/python && ln -s /does/not/exist/python botched/bin/python && uv run -p botched
Using CPython 3.13.2
Creating virtual environment at: botched
Activate with: source botched/bin/activate
error: No interpreter found for directory `botched` in virtual environments, managed installations, or search path
$ uv-pr venv botched && rm botched/bin/python && ln -s /does/not/exist/python botched/bin/python && uv-pr run -p botched
Using CPython 3.13.2
Creating virtual environment at: botched
Activate with: source botched/bin/activate
error: No interpreter found for directory `botched` in virtual environments, managed installations, or search path
```

The error is imo misleading, we found an interpreter, but it's broken, though that looks like a different problem. 

---

_@konstin reviewed on 2025-04-16 22:52_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_compile.rs`:16295 on 2025-04-16 22:52_

@jtfmumm you got an opinion on that from the other work on healing broken venvs wrt `pyvenv.cfg`?

---

_Comment by @konstin on 2025-04-16 22:55_

It would be interesting to know if this has downstream effects, it would be nice if we could scope the special case down to only broken symlinks, which tightly corresponds to uninstalled python interpreters.

---

_@zanieb reviewed on 2025-04-16 23:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:16295 on 2025-04-16 23:29_

i.e., we have https://github.com/astral-sh/uv/blob/7d12b6af00fea7b45d0c0e1e2d0f9431b3cf9b9c/crates/uv-python/src/discovery.rs#L819-L828

---

_Comment by @konstin on 2025-05-06 11:20_

@zanieb Could you find out if we still need `InterpreterError::NotFound` with `InterpreterError::BrokenSymlink`, or if there are downstream problems?

---

_Comment by @zanieb on 2025-05-06 12:27_

I can try, but it's going to be hard to find time as it's not very critical. Do we need to remove it? It's kind of high risk to remove error variants that are used in Python discovery.

---

_Comment by @konstin on 2025-05-06 14:03_

If we don't have the clarity about `InterpreterError::NotFound`, can we move ahead with just adding `InterpreterError::BrokenSymlink`?

---

_@zanieb reviewed on 2025-05-07 17:28_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:687 on 2025-05-07 17:28_

Doesn't specializing this case mean we're not improving the error message when it's not a virtual environment? I'm curious why you chose this instead of having conditional display of the hint? Either with `BrokenSymlink(PathBuf, bool)` or by deferring that `pyvenv.cfg` lookup to error display time. (adding another dedicated variant is an option, of course, but seems like more work downstream of here)

---

_@zanieb approved on 2025-05-07 17:58_

---

_Comment by @zanieb on 2025-05-07 17:58_

Thanks for your patience

---

_@zanieb reviewed on 2025-05-07 18:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:17384 on 2025-05-07 18:06_

The specialization for that would be at

https://github.com/astral-sh/uv/blob/060be9cef197d83e5546fb1be10e8e8373a1cfc6/crates/uv-python/src/discovery.rs#L897-L906

It's not great right now. I think we should tackle that separately though.

---

_@konstin reviewed on 2025-05-07 18:07_

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:687 on 2025-05-07 18:07_

I've only seen the error show in uv for venvs, but it doesn't hurt to cover the non-venv case, too.

In the test the error looks suboptimal too (we're ignoring the explicitly passed python interpreter is unexpected, I checked both `uv venv` and `uv pip compile`), but that's a different problem (and the behavior for this case does not change).

---

_@zanieb reviewed on 2025-05-07 18:10_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:17384 on 2025-05-07 18:10_

Looking at an aspect of this briefly... https://github.com/astral-sh/uv/pull/13335

---

_Merged by @zanieb on 2025-05-07 18:24_

---

_Closed by @zanieb on 2025-05-07 18:24_

---

_Branch deleted on 2025-05-07 18:24_

---
