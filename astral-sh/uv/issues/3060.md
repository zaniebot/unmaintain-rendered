```yaml
number: 3060
title: "Allow the `--python` flag in `uv pip install` to take the location of a venv"
type: issue
state: closed
author: pfmoore
labels:
  - enhancement
assignees: []
created_at: 2024-04-16T10:43:12Z
updated_at: 2024-04-25T15:12:02Z
url: https://github.com/astral-sh/uv/issues/3060
synced_at: 2026-01-10T05:31:37Z
```

# Allow the `--python` flag in `uv pip install` to take the location of a venv

---

_Issue opened by @pfmoore on 2024-04-16 10:43_

Pip allows you to install into a given virtual environment using `pip install --python /path/to/.venv ...`. This is a minor convenince feature because it lets you specify the environment without needing to consider the platform difference of where the interpreter is located `bin/python` or `Scripts/python.exe`

Could `uv` add the same feature?

(From a brief scan of the code, it appears that this would simply involve an extra branch in `find_python::find_requested_python`)

---

_Comment by @zanieb on 2024-04-16 13:13_

This seems reasonable to me but may have some nuances. There's some relevant discussion in https://github.com/astral-sh/uv/issues/1795 where we are unclear about how to select a Python interpreter found in a virtual environment.

For my own notes, we'd probably want to track this in the design at #2386 

---

_Label `enhancement` added by @zanieb on 2024-04-16 13:13_

---

_Comment by @charliermarsh on 2024-04-16 13:17_

Yeah this seems reasonable to support. I think it should be straightforwardish? If we're passed a directory, assume it's a virtualenv, in which case the Python must be at `.venv/bin/python` or `.venv/Scripts/python.exe` depending on the platform.

---

_Comment by @pfmoore on 2024-04-16 13:35_

> This seems reasonable to me but may have some nuances.

I don't think so - if the argument to `--python` is a directory, look for `<supplied_dir>/bin/python` (on Unix) or `<supplied_dir>/Scripts/python.exe` (on Windows)[^1] and use that in its place. We only do this if the user explicitly supplies `--python`, so there's no implication on the environment discovery code.

#1795 is about `uv venv` - I wasn't thinking of this for `uv venv` but even so, I don't think it will make a difference. The same code still runs, we just pre-process the `--python` argument value.

> If we're passed a directory, assume it's a virtualenv, in which case the Python must be at .venv/bin/python or .venv/Scripts/python.exe depending on the platform.

Yep, that's what pip does. I'm happy to try to contribute a PR if you'd like. I'm pretty new at Rust, though, so it could take me a little while to set things up. And I've no experience writing tests in Rust, so I may need some help there.

[^1]: I'd actually check both, just to cover things like Cygwin. 

---

_Comment by @zanieb on 2024-04-16 13:38_

Ah I missed that this was _just_ for `pip install` that seems really straight-forward.

---

_Comment by @pfmoore on 2024-04-16 14:58_

There *is* an interesting nuance here, though. `python -m pip install -p .venv ...` currently means "search for a Python executable called `.venv` in `PATH`", because the path search is triggered by the lack of a separator in the value. Changing that would be a behaviour change (although I'd have trouble imagining someone calling their virtual environment `python.exe`, so it's unlikely to matter in practice).

I'd propose the following behaviour:

1. Check for a version number as at present.
2. Otherwise, if the `--python` value is a path to an existing file, use that as the interpreter.
3. If it's a directory, assume it's a venv and use the venv interpreter.
4. Otherwise, if it contains a path separator, fail with an error.
5. Finally (a filename with no path part) try a search on `PATH`.

Does this sound reasonable?

---

_Comment by @charliermarsh on 2024-04-16 15:00_

Yeah, it does sound reasonable to me.

---

_Comment by @pfmoore on 2024-04-16 15:28_

I've had a go at writing a PR: https://github.com/astral-sh/uv/pull/3064 It's very basic, so feel free to ignore it if you prefer.

---

_Closed by @charliermarsh on 2024-04-16 18:39_

---

_Comment by @zanieb on 2024-04-25 14:35_

@pfmoore when you say path separator here... do you mean `PATH` separator or file system path separator? lol

---

_Comment by @pfmoore on 2024-04-25 14:48_

Sorry, I meant `/` on Unix, and `\` or `/` on Windows - so *filesystem* separator (`os.sep`/`os.altsep`, rather than `os.pathsep`).

---

_Comment by @zanieb on 2024-04-25 14:53_

Great thanks I figured so :) Might want to error on both even.

---

_Comment by @pfmoore on 2024-04-25 15:12_

At the end of the day, anything that isn't an executable on `$PATH` will fail with an error anyway, so this is mostly an optimisation to fail fast on things that can't possibly be the name of a Python interpreter.

---
