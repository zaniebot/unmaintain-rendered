---
number: 10966
title: Choose which scripts entrypoints are installed in the virtual environment when installing packages
type: issue
state: open
author: Coruscant11
labels:
  - enhancement
assignees: []
created_at: 2025-01-26T12:21:56Z
updated_at: 2025-02-02T20:01:04Z
url: https://github.com/astral-sh/uv/issues/10966
synced_at: 2026-01-10T01:24:59Z
---

# Choose which scripts entrypoints are installed in the virtual environment when installing packages

---

_Issue opened by @Coruscant11 on 2025-01-26 12:21_

### Summary

Hello! 🙂 

While working with environment encountering issues with dependencies scripts, (in my case, having `httpx` installing a `httpx.exe` on Windows, raising some issues like #9531), I was wondering:

In fact, and I probably think for most people this is the case, I was using `httpx` as a library. I just wanted to call its code.
However, I do not need [the entrypoint included](https://github.com/encode/httpx/blob/master/pyproject.toml#L60).
Could we avoid, if we want, installing a script (`.exe` on windows)?
Or for example, choose which script we want to allow inside our virtual environment?

In my case, I encounter the issue while using `uv tool install`. Maybe in the case of `uv tool` we could want only one command of the installed package for example.

An option like choosing exactly the name of the scripts allowed for example would be great. For example, what if I would want to install `uv` without `uvx`?
Is it an option that could be available in every install related commands or only this one? (Maybe we want to limit where it is since this is most probably not defined in a PEP).

What is your opinion about it? I don't really know if this kind of option is a good idea in fact, since of course you have a better global picture of pip related constraints than me.

Cheers!

### Example

_No response_

---

_Label `enhancement` added by @Coruscant11 on 2025-01-26 12:21_

---

_Renamed from "Add a `--no-script` or `--with-script` to `tool install` and maybe `pip install` and other similar commands" to "Add a `--no-script`, `--with-script` like commands to `tool install` and maybe `pip install` and other similar commands" by @Coruscant11 on 2025-01-26 12:26_

---

_Renamed from "Add a `--no-script`, `--with-script` like commands to `tool install` and maybe `pip install` and other similar commands" to "Add `--no-script`, `--with-script` like commands to `tool install` and maybe `pip install` and other similar commands" by @Coruscant11 on 2025-01-26 12:26_

---

_Renamed from "Add `--no-script`, `--with-script` like commands to `tool install` and maybe `pip install` and other similar commands" to "Choose which scripts entrypoints are installed in the virtual environment when installing packages" by @Coruscant11 on 2025-01-26 12:27_

---

_Comment by @zanieb on 2025-01-26 16:49_

I think we want something like this for `uv tool install`. We could perhaps extend it to other interfaces. I worry a little about packages that expect their entrypoint to be available where removing it will break some functionality in the library, but it's hard to say if that's realistic or not.

---

_Comment by @Coruscant11 on 2025-01-27 20:00_

@zanieb Thanks for the quick answer!
In my view, once you explicitly choose which scripts to enable, this shouldn't be an issue. If an executable is missing, it would be fairly straightforward to fix by simply enabling the required script. While the process may not be completely intuitive, we're dealing with a rather niche use case here, right? What do you think?

Would you prefer to keep this task reserved for the maintainers, or would you welcome a pull request from me? (If I managed to do so 😜)
Thanks again!

---

_Comment by @zanieb on 2025-01-28 18:05_

You could definitely try to implement it, but we should make sure we're on the same page about the interface first.

How do you propose exposing this to users? e.g., `uv pip install --no-executable <name>` or `--no-bin <name>` seems fine, but.. how would this be exposed elsewhere? We'd also probably want `--no-bin-package <package>` to exclude all entrypoints for a package.

---

_Comment by @Coruscant11 on 2025-02-02 20:01_

@zanieb Sorry for the late answer! And thanks for yours.

> but we should make sure we're on the same page about the interface first.

Totally agree!
I had exactly the first same idea as yours, with `--no-bin` and `--no-bin-package`.
This was what I had in mind.
This is also something that could be configurable from configuration file.

I think that excluding packages is easier and safer than only choosing what will be installed. (We may forget some required one)

I think I will try to play on my side, and then it will be easier to catch any missing parts regarding the interface.
To be clear, I do not mind at all if you are not satisfied with the interface at the end! I just love writing some Rust and trying to contribute, so it will not be a waste of time at all on my side.

---
