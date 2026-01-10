```yaml
number: 9849
title: "Use `sys.path` for package discovery"
type: pull_request
state: open
author: pradyunsg
labels: []
assignees: []
draft: true
base: main
head: sys-path-for-discovery
created_at: 2024-12-12T19:14:16Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/9849
synced_at: 2026-01-10T11:10:34Z
```

# Use `sys.path` for package discovery

---

_Pull request opened by @pradyunsg on 2024-12-12 19:14_

Resolves #2500
Intend to resolve #4466 before coming out of draft mode here.

x-ref <https://github.com/astral-sh/uv/pull/2413>, since this PR removes an assumption introduced in that PR that a missing site-packages directory means that there are _no_ packages to be found.

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
- Change installed package discovery logic to use `sys.path` rather than inferred paths based on `sysconfig`'s `purelib` and `platlib` directories.
- [TODO] Change uninstallation logic to skip over packages that are installed outside the working environment.
- [TODO] Remove any dead code, if the `site_packages` we were computing for this is not used anywhere else.

## Test Plan

<!-- How was it tested? -->
Only manually tested as of now.

The plan for automated tests is: Use a `.pth` file as an extra path to be injected to the virtual environment being modified, with the extra path pointing to site-packages from another virtual environment.

This should be less complicated than the proposed approaches of using `sitecustomize.py` as well as relying on system-site-packages with a mock system interpreter -- all these approaches modify the `sys.path` in a manner visible to the interpreter introspection script and using a `.pth` file is likely to be the easiest to reason about.


---

_Comment by @charliermarsh on 2024-12-12 20:15_

Wow, new contributor just dropped!

![Screenshot 2024-12-12 at 3 15 03 PM](https://github.com/user-attachments/assets/5d81232a-86e0-4ead-a5dd-78dfaac2218e)


---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-12 20:15_

---

_Comment by @pradyunsg on 2024-12-13 13:16_

_sigh_

Locally, I'm hitting https://github.com/rust-lang/rust-analyzer/issues/16959 trying to diagnose what's happening in the two spots that the most recent commit has added more granular `bail!` logs to, and looks like I can't step through with a debugger on this without resorting to hackery around `CARGO_MANIFEST_DIR`. 😞 

All the tests run via the VS Code UI for debug end up panicking before running with:

```
---- tool_upgrade::tool_upgrade_multiple_names stdout ----
thread 'tool_upgrade::tool_upgrade_multiple_names' panicked at crates/uv/tests/it/common/mod.rs:292:84:
called `Result::unwrap()` on an `Err` value: NotPresent
```

Looks like I'll be falling back to using regular `println!` to diagnose things. 🌈 


---

_Comment by @pradyunsg on 2024-12-13 18:53_

Doing a brain dump since I need a break now -- it took me a bit of time to separate a bunch of tooling failures/noise, and actually understand what's broken here...

https://github.com/astral-sh/uv/blob/5903ce5759770755373a962c8b51ee2534de50aa/crates/uv-virtualenv/src/lib.rs#L71

The virtual environment's `Interpreter` object is created based on the base environments' `Interpreter` object. This step, notably, does not update the information about `sys.path`, which keeps pointing to paths from the base environment. A bunch of the failures in tests are happening since the logic ends up using a "stale" `sys.path` information from the base environment when working with the virtual environment's interpreter.

An easy reproducer for this, with this PR, is using `uv tool install <whatever>` for a tool that doesn't already have an environment for it. You'll see it fail to find the distributions that were _just installed_ in the virtualenv created.

This used to be fine earlier since the logic didn't care at all about sys.path and the interpreter's prefix was being updated which was effectively "updating" the site-packages path that'd be used for the lookups. Ignoring to update sys.path is not viable, at least, not with the goal of this change of using appropriately using sys.path for package discovery.

Still thinking through what to do here... The "slow" option here would be to recreate the `Interpreter` object with the interpreter query logic again (caching that as appropriate). However, the only attribute that's incorrect in a manner that matters semantically today is `sys.path` and all the other attributes could be inferred from the base Python. Plus, this approach is also gonna make nearly all locations where uv works with a virtualenv it just created meaningfully slower by requiring it to wait on the Python interpreter query logic ~every time (it's very unlikely that the user will have the newly created interpreters' information cached).


---

_Comment by @pradyunsg on 2024-12-13 19:05_

Ok, an alternative that I haven't fully thought through the implications of... I think this is more of a reasonable guess of what sys.path will be, but it might just work. Here's the idea:

Instead of querying the interpreter again, use the `base_interpreter.sys_path_after_site_import + [virtualenv.site_packages]` when system-site-packages is true / use `base_interpreter.sys_path_before_site_import + [virtualenv.site_packages]` when system-site-packages is false. This, of course, relies on the fact that `python` lets us disable `site` on startup & uv has control over the complete lifetime/invocation for its interpreter introspection logic.

```sh-session
❯ /opt/pradyunsg/bin/python3.12 -S -c "import sys; print(sys.path)"                                                
['', '/opt/pradyunsg/lib/python312.zip', '/opt/pradyunsg/lib/python3.12', '/opt/pradyunsg/lib/python3.12/lib-dynload']
❯ /opt/pradyunsg/bin/python3.12 -c "import sys; print(sys.path)" 
['', '/opt/pradyunsg/lib/python312.zip', '/opt/pradyunsg/lib/python3.12', '/opt/pradyunsg/lib/python3.12/lib-dynload', '/opt/pradyunsg/lib/python3.12/site-packages']
```

Let me know if this seems reasonable, and also... I'd be very happy to hear if someone can think of a scenario where this might be broken!

---

_Comment by @paveldikov on 2024-12-15 00:34_

> I'd be very happy to hear if someone can think of a scenario where this might be broken!

Broken might be a bit harsh, but 'incomplete'?
* this will indeed work for `system-site-packages`, since the paths all come from the base interpreter anyway
* but I don't think it will work for the broader pattern of modifying the module search path via `sitecustomize.py`/`.pth` files?
    - This is commonly used to implement 'layered'/'stacked' venvs ([see comment](https://github.com/astral-sh/uv/issues/2500#issuecomment-2189503090))

I do note that the former usage is much more common than the latter.

If the 'correct' thing to do is unacceptably slow -- and the guess is good enough for most use cases -- maybe an acceptable compromise is to do the 'good enough' thing by default and provide an option for those who really need that extra correctness?

---

_Comment by @pradyunsg on 2024-12-16 10:14_

> `sitecustomize.py`/`.pth` files

You won't have these in an environment that was just-created in-process by uv, as I understand it. You could have these in the base environment though, so the "guess" model doesn't work in general for environments with `include-system-site-packages = true`.

I think that's fine. We could stick with this computational approach for environments with `include-system-site-packages = false`, since that's likely the 90% case and fall back to doing the subprocess for environments with `include-system-site-packages = true`.


---

_Comment by @charliermarsh on 2024-12-29 01:45_

I think it's ok to re-query information if we have to. We cache it anyway, and ideally we only do this when `system-site-packages = true`.

---

_Comment by @charliermarsh on 2024-12-29 01:45_

Do you need any guidance on the code changes, beyond that?

---

_Comment by @paveldikov on 2024-12-29 18:22_

> I think it's ok to re-query information if we have to. We cache it anyway, and ideally we only do this when `system-site-packages = true`.

A-ha! Maybe an elegant solution would be to tune the cache key for `sys.path`?

That way the 'expensive'/side-effectful recomputation will be done _at most_ once, and ideally not at all (ideally cache entry populated at end of venv creation)

The cache key could be some sort of serialised struct that encapsulates the myriad things that could influence `sys.path`:
* base prefix
* venv site-packages path
* boolean value of `system-site-packages`
* hashes of any/all `sitecustomize.py`, `usercustomize.py`, `*.pth` files found. (Or, if feeling cheap, just list of filenames found)

---

_Comment by @pradyunsg on 2024-12-30 12:47_

> Do you need any guidance on the code changes, beyond that?

Not at the moment. I'll only be able to pick this back up early next year.


---

_Comment by @pradyunsg on 2025-01-13 16:45_

~Whelp, looks like I broke something about regular virtualenv package discovery with the most recent commit. 🙃~

🤦🏽 Figured it out.

---

_Comment by @pradyunsg on 2025-01-13 20:34_

```
{purelib:"/Users/pgedam/Developer/github/astral-sh/uv/workarea.ignore/.venv/opt/homebrew/Cellar/python@3.13/3.13.0_1/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages", platlib:"/Users/pgedam/Developer/github/astral-sh/uv/workarea.ignore/.venv/opt/homebrew/Cellar/python@3.13/3.13.0_1/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages", scripts:"/Users/pgedam/Developer/github/astral-sh/uv/workarea.ignore/.venv/opt/homebrew/Cellar/python@3.13/3.13.0_1/Frameworks/Python.framework/Versions/3.13/bin", data:"/Users/pgedam/Developer/github/astral-sh/uv/workarea.ignore/.venv/opt/homebrew/Cellar/python@3.13/3.13.0_1/Frameworks/Python.framework/Versions/3.13", include:"/Users/pgedam/Developer/github/astral-sh/uv/workarea.ignore/.venv/include/site/python3.13"}
```

It looks like the `virtualenv.scheme`'s value is incorrect when it gets passed into `Interpreter.with_virtualenv`. I'm gonna look into what's going on with the path composition for those schemes tomorrow.

---

_Comment by @pradyunsg on 2025-01-14 15:22_

Looks like it's wrong at the point of querying the interpreter.

```
        virtualenv: Scheme {
            purelib: "opt/homebrew/Cellar/python@3.13/3.13.0_1/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages",
            platlib: "opt/homebrew/Cellar/python@3.13/3.13.0_1/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages",
            scripts: "opt/homebrew/Cellar/python@3.13/3.13.0_1/Frameworks/Python.framework/Versions/3.13/bin",
            data: "opt/homebrew/Cellar/python@3.13/3.13.0_1/Frameworks/Python.framework/Versions/3.13",
            include: "include/site/python3.13",
        },
```

The most recent commit "fixes" that to:

```
        virtualenv: Scheme {
            purelib: "lib/python3.13/site-packages",
            platlib: "lib/python3.13/site-packages",
            scripts: "bin",
            data: "",
            include: "include/site/python3.13",
        },
```


---

_Unassigned @charliermarsh by @konstin on 2025-02-10 17:31_

---

_Assigned to @konstin by @konstin on 2025-02-10 17:31_

---

_Comment by @henryiii on 2025-02-14 18:47_

By the way, I think this might fix `uv pip list` on PyPy, due to stdlib installs of packages:

```console
$ uv venv --seed
Using PyPy 3.10.16 interpreter at: /usr/local/bin/python
Creating virtual environment with seed packages at: .venv
 + pip==25.0.1
 + setuptools==75.8.0
 + wheel==0.45.1

$ uv pip list
Package    Version
---------- -------
pip        25.0.1
setuptools 75.8.0
wheel      0.45.1

$ .venv/bin/pip list
Package    Version
---------- -----------
cffi       1.18.0.dev0
greenlet   0.4.13
hpy        0.9.0
pip        25.0.1
readline   6.2.4.1
setuptools 75.8.0
wheel      0.45.1
```

And, by the same token, might break `uv sync` on PyPy just like it is [broken on Poetry](https://github.com/python-poetry/poetry/issues/10180), since it would be confused by non-extistent and uninstallable versions of packages. 🙄

---

_Unassigned @konstin by @konstin on 2025-03-19 17:30_

---

_Comment by @konstin on 2025-03-19 17:33_

As a semi-update on this, I tried a few ways to finish this, including `site.getsitepackage()` in https://github.com/astral-sh/uv/pull/11670, but this raised a number of additional problems each time (internal deduplication, fedora using different paths, how to deal with not-uninstallable packages in `uv sync`, etc.) we need to resolve to eventually land this.

---
