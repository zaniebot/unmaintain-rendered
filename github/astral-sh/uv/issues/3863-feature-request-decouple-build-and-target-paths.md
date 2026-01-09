---
number: 3863
title: "Feature request: decouple \"build\" and \"target\" paths in `install_wheel` API"
type: issue
state: closed
author: aochagavia
labels:
  - enhancement
assignees: []
created_at: 2024-05-27T12:18:18Z
updated_at: 2024-07-29T00:10:13Z
url: https://github.com/astral-sh/uv/issues/3863
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature request: decouple "build" and "target" paths in `install_wheel` API

---

_Issue opened by @aochagavia on 2024-05-27 12:18_

This is a feature request to modify the API of `install_wheel_rs::linker::install_wheel`. I'd love to hear whether you consider it to be within the scope of uv (in which case I might submit a PR) or out of scope (in which case I'll be using a workaround).

### Use case

_Note: this is somewhat simplified, but I hope it illustrates the point._

I have an HTTP API providing a "virtual environment build service". People submit a request to generate a virtual environment, based on specific pip requirements. Behind the scenes, I'm using the `uv` libraries (including the `install_wheel` function) to instantiate the environment. Once the environment is ready, I zip the resulting files and let the user download the archive.

The current implementation of `install_wheel`, however, requires that the path to the environment both in the builder and the target machine is exactly the same (e.g. if you built the environment at `/foo/bar`, which I'm calling the "build path", you need to extract it and use it at `/foo/bar`, which I'm calling the "target path"). This is limiting, because it forces the user to extract the environment in a path determined by the server, or it forces the server to let the user specify the path where the environment should be built (with some validation code to keep things secure).

Conceptually, the limitation is unnecessary. As far as I can see, there's nothing in the way of making `install_wheel` more flexible, letting it distinguish between:

1. The directory where the files end up after linking (in the machine that creates the environment), and;
2. The directory where the environment should be located when you use it (in the machine that uses the environment).

With this modification, the builder could create environments at arbitrary locations (e.g. `/tmp/env1`), while targeting different paths on the target machine (e.g. `/usr/local/my-env`).

### Current workaround

According to [the PyPA documentation](https://packaging.python.org/en/latest/specifications/binary-distribution-format/#installing-a-wheel-distribution-1-0-py32-none-any-whl), the only changes made to files during wheel installation is replacing shebangs at the beginning of python scripts (to make them point to the right interpreter). If you move the environment to a different place in the filesystem, the paths in the python scripts get broken. However, you can work around this problem by manually going through the scripts and re-rewriting the shebangs to the new path. Not ideal, but it works.

### Prior art

Though focused on the conda ecosystem, the [rattler](https://github.com/mamba-org/rattler) collection of crates offers an API that decouples the "build" and "target" paths of a conda prefix, in an analogous way as described in this feature request. The relevant function is [link_package](https://docs.rs/rattler/0.26.0/rattler/install/fn.link_package.html).

---

_Comment by @charliermarsh on 2024-05-27 16:36_

This seems very similar to https://github.com/astral-sh/uv/issues/3669 -- what do you think?

---

_Comment by @charliermarsh on 2024-05-27 16:41_

Like I wonder if we can solve this by allowing an opt-in setting to make everything relocatable, rather than decoupling the paths.

---

_Label `enhancement` added by @charliermarsh on 2024-05-27 16:41_

---

_Comment by @aochagavia on 2024-05-27 17:58_

Thanks for thinking with me ;)

Another alternative would be an option to leave the shebangs as they were originally, and letting the user do the rewriting (though I can imagine it's an uncommon use case). I see it as an improved version of my workaround, and it would probably be much easier to implement than my original proposal.

---

_Comment by @paveldikov on 2024-07-22 15:16_

I'm interested in this as well. I am thinking, a `uv pip install --relocatable` flag could work quite nicely? Should be straightforward to add this CLI flag and then poke its value through to `install_wheel()` and `write_script_entrypoints()`. Let me know if you agree with this approach and I can take a stab at a PR.

Would probably have to caveat that this only works for 'pure' script entrypoints, as opposed to arbitrary executables/scripts. (e.g. `venv/(bin|Scripts)/uv(.exe)?` itself)

Also x-referencing #3587 which is similar but on the venv side. In fact, if/when a venv is created in relocatable mode (can we/should we persist this info in `pyvenv.cfg`?), the `--relocatable` flag can be auto-inferred from the underlying venv.

---

_Comment by @charliermarsh on 2024-07-22 15:52_

I'm open to something like a `--relocatable` flag. I'm not certain how it should interact with the `venv` flag. It would be nice to know why it was removed from `virtualenv`.

---

_Comment by @paveldikov on 2024-07-22 16:36_

>It would be nice to know why it was removed from `virtualenv`.

Found some reading: pypa/virtualenv#1473 and pypa/virtualenv#1549.

IMO it appears that the problem was largely caused by the virtual environment provisioner (`virtualenv`) being a totally separate entity from the package installer (`distlib`/`pip`). As a result, `virtualenv` could only do a half-hearted (and post-hoc) job at making virtual environments relocatable. And it's not just a matter of sequencing of operations -- the owners of `virtualenv` would have had to play catch-up with the various different platform implementations (e.g. trampoline binaries on Windows). Overall a very fragile design, and a pretty thankless job maintaining such a feature.

So I totally understand why they decided to axe the feature -- it didn't really belong in `virtualenv`.

But `uv` is IMO different because it encompasses both virtual environment provisioning (`uv venv`) as well as package installation (`uv pip`). It controls both ends of the equation, and can do a much better job at this. And we can have end-to-end tests to prevent this functionality from randomly regressing.

---

_Comment by @paveldikov on 2024-07-22 22:01_

I did some experiments in my local checkout. The good news is that the change to `get_script_executable()` would be trivial -- I added a `relocatable: bool` arg, an `if`-`else` that forces the path to relative, and it 'just' works. I managed to poke this new arg all the way out through the call stack, right down to the CLI handler. The whole thing works as expected -- yay, relocatable entrypoints on demand!

The ugly part is that, since I am pretty new to this codebase (and Rust in general), I was playing it pretty fast and loose. Since `Installer` is used in so many places (e.g. `uv add`, `uv sync`, ...), adding this otherwise innocuous boolean creates a real domino/fan-out effect. I ended up hardcoding it to `false` for all commands except `uv pip install`. Not ideal, but hey, it worked.

Self-deprecation aside, to me it became clear that this merits a more involved architectural discussion. `relocatable: bool` _could_ sit in `PipInstallSettings` as does in my scratch workspace, but then what about other the install-flavoured commands such as (`add`, `sync` et al)? Do they need their own copy of this flag? Is this good UX or does this create a footgun? Or do we normalise this up the way into `PipSettings` -- still a footgun? Or perhaps all signs indeed point towards this being a property of the venv itself? Really quite unsure...

---

_Comment by @ceache on 2024-07-25 14:31_

Just a thought but wouldn't make more sense to make the `relocatable` flag a venv config attribute? Something akin to `uv venv --relocatable ...`

The fact that each install action (`install`, `sync`, `add`) against a given venv needs to remember to pass a `relocatable` option to be consistent seems like a poor UX / good footgun.

(this was, in IMHO, one of the worse aspects of the virtualenv package relocatable implementation: you had to go back and patch/fix packages after each new pip install)

Making it somehow a venv attribute would alleviate this UX issue

---

_Referenced in [astral-sh/uv#5515](../../astral-sh/uv/pulls/5515.md) on 2024-07-28 14:26_

---

_Comment by @paveldikov on 2024-07-28 14:29_

It does seem that all roads lead to this being a flag to `uv venv`. I gave it another shot and the code change looks _a lot_ tidier now -- it's almost as if it's telling me that this is the right level of abstraction :-)

I have opened a draft PR @ #5515. It still has a few things needing done at the periphery, but the basic workings of it are pretty much ready to review.

---

_Closed by @charliermarsh on 2024-07-29 00:10_

---

_Closed by @charliermarsh on 2024-07-29 00:10_

---
