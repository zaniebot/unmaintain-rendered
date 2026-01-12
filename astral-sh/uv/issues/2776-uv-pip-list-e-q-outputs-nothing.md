```yaml
number: 2776
title: "`uv pip list -e -q` outputs nothing."
type: issue
state: closed
author: charlesnicholson
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-04-02T14:06:07Z
updated_at: 2024-04-19T14:45:28Z
url: https://github.com/astral-sh/uv/issues/2776
synced_at: 2026-01-12T15:58:40Z
```

# `uv pip list -e -q` outputs nothing.

---

_@charlesnicholson_

This one is more entertaining than serious- we deploy Python artifacts as a set of wheels, a constraints file, and a bunch of shell/batch scripts that possibly create the venv and install the wheels etc before launching the corresponding entry point script in the venv.

These shell / batch scripts run from a non-deployed development repository as well, so they have to decide whether to `uv pip install` anything or just lean on the existing editable-installs of our first-party packages.

To do this, we run `uv pip list -e` and search the process's stdout: if there are no editable installations matching the requested package name, we install from the wheel file.

Amusingly, our scripts are also quiet by default, so we pass `-q` to everything.

So, we run `uv -q pip list -e`, which always prints nothing and quietly messes up our strategy because the output with the "quiet" flag is indistinguishable from the lack of output when `uv pip list` finds nothing.

Removing the `-q` flag of course fixes it trivially. I'm totally happy with this being "user error", but I thought it was an interesting / funny way to hold `uv` wrong.

![3caedb8a-1692-4c89-945e-791386768c3d_text](https://github.com/astral-sh/uv/assets/3010295/8fcb882f-50d4-4d0e-9541-b3c3b5eae552)


---

_Comment by @zanieb on 2024-04-02 14:39_

We agree it probably shouldn't be _that_ quiet :)

Open to suggestions on what should change if anything when `--quiet` is used in `uv pip list`

---

_Label `cli` added by @zanieb on 2024-04-02 14:39_

---

_Comment by @charlesnicholson on 2024-04-02 14:47_

If it's interesting, my opinion is that if a tool only exists to dump info to stdout/stderr, a quiet flag doesn't make sense and I'd probably not offer it as an option. I'd prefer to be forced to simply not call it, instead of having an option that can confound the meaning of "there was no output".

But it's easy to see the other side too: users are grown-ups, and the tool is giving them what they've explicitly asked for :)

---

_Comment by @zanieb on 2024-04-02 14:51_

`--quiet` is just a global CLI flag that adjusts the verbosity of our logging, I'm not sure if it makes sense to turn off for a single command although I agree it doesn't really make sense in a "show me things" command.

---

_Comment by @zanieb on 2024-04-02 19:19_

We should emit this to stdout instead of stderr

---

_Label `help wanted` added by @zanieb on 2024-04-02 19:19_

---

_Comment by @charliermarsh on 2024-04-03 01:10_

It looks like `pip list --quiet` _also_ doesn't print :D

---

_Comment by @charlesnicholson on 2024-04-05 13:37_

Any appetite for something like a `--json` flag that emits the repository installed package manifest to stdout in json format? That way it's easily machine-parseable (unlike the current output), and there's a difference between "no output whatsoever" (`-q`) and "no output because of requested filters or the venv is just empty"- the latter would emit `{\n}\n`.

That might be a nice way to disambiguate the cases and make `uv pip list` easier to tool against, all at the same time, without breaking compatibility with `pip list`.

---

_Comment by @zanieb on 2024-04-05 14:48_

If we emit to stdout instead of stderr it will not be silenced by `-q`.

There's appetite for machine readable / JSON output throughout the CLI though, yeah.

---

_Comment by @BurntSushi on 2024-04-19 14:28_

I think probably the `-q/--quiet` flag should be respected here and `uv` shouldn't emit any output. I think this matches the behavior of other CLI tools like `grep`. Typically, the utility in a `-q/--quiet` flag though is that you want to run the command just to determine whether it was successful or not by querying its exit status. I'm not sure that's a meaningful thing to ask of `uv pip list`, though it conceivably could be I suppose.

---

_Comment by @charliermarsh on 2024-04-19 14:29_

Yeah, that makes sense to me.

---

_Comment by @zanieb on 2024-04-19 14:43_

Thanks!

---

_Closed by @zanieb on 2024-04-19 14:43_

---
