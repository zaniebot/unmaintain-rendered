```yaml
number: 1794
title: "Support ` --prefer-binary` in `requirements.txt` files"
type: issue
state: open
author: stinodego
labels:
  - compatibility
assignees: []
created_at: 2024-02-21T04:44:52Z
updated_at: 2024-08-13T13:52:51Z
url: https://github.com/astral-sh/uv/issues/1794
synced_at: 2026-01-10T04:53:49Z
```

# Support ` --prefer-binary` in `requirements.txt` files

---

_Issue opened by @stinodego on 2024-02-21 04:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`pip` supports including a `--prefer-binary` flag in a requirements.txt like so:
```
--prefer-binary

polars
ruff
```

However, `uv pip install` does not support this:

```
$ uv pip install -r requirements.txt 
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement in `requirements.txt` at position 0
```

This is on version `0.1.6`.

It would be nice if this syntax could be supported!

---

_Comment by @charliermarsh on 2024-02-21 05:04_

I think we support `--no-binary` and `--only-binary` on the command-line, but not `--prefer-binary` (and none are supported in requirements.txt right now).

---

_Label `compatibility` added by @zanieb on 2024-02-21 17:28_

---

_Comment by @zanieb on 2024-02-21 17:29_

We already always prefer binaries, right? I believe that's why we omitted the flag.

I'm not sure these belong in the requirements file. Just wondering, why is this preferred (other than compatibility)?

---

_Comment by @stinodego on 2024-02-21 22:43_

> We already always prefer binaries, right? I believe that's why we omitted the flag.

I'm not sure what `uv` does. I know that `pip` will prefer to install the latest version, even if there is no binary for it. Using `--prefer-binary` will make it install an older version if that means it doesn't have to build from source.

> I'm not sure these belong in the requirements file. Just wondering, why is this preferred (other than compatibility)?

Honestly, I also don't know if it belongs there. I reported this because `uv` wants to be a drop-in replacement for `pip` and I ran into this.

In general though, the benefit of having this setting in the requirements file itself is that you don't have to specify this setting in all your installers, e.g. CI / Makefile / Dockerfile / ... . The setting is in one place, together with the requirements. That does make some sense.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-04 03:56_

---

_Comment by @charliermarsh on 2024-04-08 02:40_

@zanieb -- I think `--prefer-binary` goes even further, in that it will prefer an older version of a package with a wheel over a newer version without one.

---

_Unassigned @charliermarsh by @charliermarsh on 2024-04-08 02:40_

---

_Comment by @njzjz on 2024-04-21 05:59_

I am +1 for this feature.  A use case is that h5py linux_aarch64 wheels are available in v3.10 but not available in v3.11 (see https://github.com/h5py/h5py/issues/2408). When I use pip, I can set `export PIP_PREFER_BINARY=1` in the CI service to let pip install v3.10 wheels. However, ux doesn't have such an option. The default behavior is to install from v3.11 source, which will fail as it requires HDF5 library to be installed in advance.

We need uv to support the `UV_PREFER_BINARY` environmental variable.

---

_Comment by @zanieb on 2024-04-21 15:23_

@njzjz Is there a reason that this flag is better than just adding a pin or additional constraint?

---

_Comment by @stinodego on 2024-04-21 15:40_

You may not want to pin a dependency to take advantage of the latest updates whenever they come out. However, you generally also want to avoid compiling packages from source if binaries are available. This setting unifies the two.

Constraints may not work, or I would have to micromanage these for each dependency / platform depending on the wheels that are available. A single `--prefer-binary` takes care of all that.

---

_Comment by @njzjz on 2024-04-21 22:20_

This flag will also ensure the CI is not broken in the future when a new release doesn't contain the binary. Otherwise, we need to add the pin manually every time.

---

_Comment by @notatallshaw-gts on 2024-05-08 22:47_

My use case for `--prefer-binary` is in the equivalent of a `pip compile` scenario, I am generating new pinned versions of my entire environment from my requirements.

Ideally I would use `--only-binary` but there are a handful of packages where this is still not possible, and it's not practical to go through my ~200 transitive dependencies and see which ones have not yet releases a wheel for their latest release every time I update the pinned versions.

---

_Comment by @notatallshaw-gts on 2024-06-12 20:54_

May be helpful for other users, my use case is now solved by https://github.com/astral-sh/uv/issues/4063 / https://github.com/astral-sh/uv/pull/4067.

I can specify that wheels should always be picked with `--only-binary :all:` and override that to have specific packages use sdists with `--no-binary <package>`

---

_Comment by @njzjz on 2024-06-12 22:10_

> I can specify that wheels should always be picked with `--only-binary :all:` and override that to have specific packages use sdists with `--no-binary <package>`

I created #4291 for the environment variable support request.

---

_Comment by @tpgillam on 2024-08-03 11:25_

I agree that this would be useful for compatibility purposes; I'm setting up the matplotlib test environment, and was hoping to use `uv pip sync`. However it doesn't work due to the `--prefer-binary` here:

https://github.com/matplotlib/matplotlib/blob/109ab93ba2e687a7b805cb0075efa52ef4e61cde/requirements/testing/extra.txt#L3

```
> uv pip sync requirements/dev/dev-requirements.txt
error: Error parsing included file in `requirements/dev/dev-requirements.txt` at position 79
  Caused by: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement at requirements/dev/../testing/extra.txt:3:1
```

---

_Comment by @notatallshaw-gts on 2024-08-04 20:21_

It would be good if it was in the pip the compatibility document: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md

I guess anyone could submit a PR for that?

---
