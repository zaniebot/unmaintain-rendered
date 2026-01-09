---
number: 9822
title: Git submodule cloning not working when using relative paths
type: issue
state: open
author: breuninger-dgrimm
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-12-11T18:31:47Z
updated_at: 2025-05-08T05:23:49Z
url: https://github.com/astral-sh/uv/issues/9822
synced_at: 2026-01-07T13:12:18-06:00
---

# Git submodule cloning not working when using relative paths

---

_Issue opened by @breuninger-dgrimm on 2024-12-11 18:31_

Hey,

I am really enjoying `uv` because I think it's the best Python package manager around. However, I have one problem which keeps me from using it at the moment.

I might have found a bug or I am doing something wrong.

Let's assume I have a repo `mylib` with a `.gitmodule` file that  looks like this:

 ```bash
[submodule "helpers"]
	path = helpers
	url = ../../utilities/helpers.git
```

The repo is hosted on a private Gitlab instance. 

I will run

```bash
uv pip install 'mylib@git+ssh://git@gitlab.mycompany.de/.../mylib.git'
```

This fails with the following error:

```
  ├─▶ Git operation failed
  ╰─▶ process didn't exit successfully: `/usr/bin/git submodule update --recursive --init` (exit status: 1)
      --- stderr
      Submodule 'helpers' (/Users/ebuser/.cache/uv/git-v0/utilities/helpers.git) registered for path 'helpers'
      fatal: repository '/Users/ebuser/.cache/uv/git-v0/utilities/helpers.git' does not exist
      fatal: clone of '/Users/ebuser/.cache/uv/git-v0/utilities/helpers.git' into submodule path '/Users/ebuser/.cache/uv/git-v0/checkouts/9f3d7b05f0eb7565/abd7aca/helpers' failed
      Failed to clone 'helpers'. Retry scheduled
      fatal: repository '/Users/ebuser/.cache/uv/git-v0/utilities/helpers.git' does not exist
      fatal: clone of '/Users/ebuser/.cache/uv/git-v0/utilities/helpers.git' into submodule path '/Users/ebuser/.cache/uv/git-v0/checkouts/9f3d7b05f0eb7565/abd7aca/helpers' failed
      Failed to clone 'helpers' a second time, aborting
```

I am assuming that `uv` interprets the `.gitmodule` references with the relative paths as local references. However, when I clone the library manually and run `git submodule update --recursive --init` it works fine.

When I run `pip install git+ssh://git@gitlab.mycompany.de/.../mylib.git`, it works just fine.

I also never had problems with `poetry` and this library.

I am a bit lost here. Can you give me a hint if I'm doing something wrong and how I can fix it?

---

_Comment by @draustin on 2025-01-04 11:49_

I observe similar behavior using `uv tool install`. In my case, I have a monorepo `my-monorepo` containing a Poetry project in `path/my-project`. Among this project's dependencies are some editable local folders, which are Git submodules. The `.gitmodules` file at the root of `monorepo` contains entries of the form:

```
[submodule "path/other-project1"]
   path = path/other-project1
   url = ../other-project1
```

Note the lack of `.git` on the URL compared to the original post. The submodule repositories are hosted alongside `my-monorepo` in Bitbucket.

`uv tool install git+https://my-name@bitbucket.org/my-company/my-monorepo.git#subdirectory=path/my-project`

```
Updating https://my-name@bitbucket.org/my-company/my-monorepo.git (HEAD)
error: Git operation failed
  Caused by: process didn't exit successfully: `C:\Program Files\Git\cmd\git.exe submodule update --recursive --init` (exit code: 1)
--- stderr
Submodule 'path/other-project1' (C:\Users\DaneAustin\AppData\Local\uv\cache\git-v0\db/other-project1) registered for path 'path/other-project1'
Cloning into 'C:/Users/DaneAustin/AppData/Local/uv/cache/git-v0/checkouts/aebdb5132ba74ab3/fe60ad9d6/path/other-project1'...
fatal: 'C:\Users\DaneAustin\AppData\Local\uv\cache\git-v0\db/other-project1' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

The cache folder does not contain `other-project`. Here is the output of `dir C:\Users\DaneAustin\AppData\Local\uv\cache\git-v0\db`:

```
    Directory: C:\Users\DaneAustin\AppData\Local\uv\cache\git-v0\db


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          1/4/2025   3:19 AM                aebdb5132ba74ab3
```

Another possibly relevant fact: before reaching this point, I encountered an `fatal: transport 'file' not allowed` error at the cloning step. Following [this advice](https://stackoverflow.com/questions/74486167/git-clone-recurse-submodules-throws-error-on-macos-transmission-type-file-n) I did `git config --global protocol.file.allow always`, and reran `uv tool ...`. This brought me to the situation described above.

A direct `git clone https://my-name@bitbucket.org/my-company/my-monorepo.git` followed by `git submodule update --recursive --init` works fine.

For context: I am investigating `uv` as a way of sharing my Python-based tools with non-programmer colleagues in a small manufacturing startup.

---

_Label `bug` added by @zanieb on 2025-01-07 19:09_

---

_Label `help wanted` added by @zanieb on 2025-01-07 19:09_

---

_Comment by @jeffmelville on 2025-01-09 19:13_

I'm having the same issue. I defined a `[tool.uv.source]` as a git repository. The git repository has submodules (which I do not need - related #10130) 

It looks like the checkout in `~/.cache/uv/git-v0/...` is configured with a remote of `~/.cache/uv/git-v0/db/...` rather than the original upstream URL. When the submodule init happens, it tries to evaluate the submodule paths relative to this remote URL rather than the original upstream URL and it fails. 

Not sure what the fix would be without changing how git checkouts are cloned or fixing up the .git/modules in between init and update

I'm uv 0.5.13 on Darwin

---

_Referenced in [astral-sh/uv#12156](../../astral-sh/uv/pulls/12156.md) on 2025-03-13 23:22_

---

_Comment by @watsonjj on 2025-05-08 05:23_

I've hit this a few times now. For example, when trying to install [vunit](https://github.com/VUnit/vunit) from the github repo:
```
uv pip install vunit-hdl@git+https://github.com/VUnit/vunit.git@acf7e7f

  × Failed to download and build `vunit-hdl @ git+https://github.com/VUnit/vunit.git@acf7e7f`
  ├─▶ Git operation failed
  ╰─▶ process didn't exit successfully: `/usr/local/bin/git submodule update --recursive --init` (exit status: 1)
      --- stderr
      Submodule 'vunit/vhdl/JSON-for-VHDL' (/Users/Jason/.cache/uv/git-v0/Paebbels/JSON-for-VHDL.git) registered for path 'vunit/vhdl/JSON-for-VHDL'
      Submodule 'vunit/vhdl/osvvm' (/Users/Jason/.cache/uv/git-v0/OSVVM/OSVVM.git) registered for path 'vunit/vhdl/osvvm'
      fatal: repository '/Users/Jason/.cache/uv/git-v0/Paebbels/JSON-for-VHDL.git' does not exist
      fatal: clone of '/Users/Jason/.cache/uv/git-v0/Paebbels/JSON-for-VHDL.git' into submodule path '/Users/Jason/.cache/uv/git-v0/checkouts/9b32958e04bf51f8/acf7e7f2/vunit/vhdl/JSON-for-VHDL' failed
      Failed to clone 'vunit/vhdl/JSON-for-VHDL'. Retry scheduled
      fatal: repository '/Users/Jason/.cache/uv/git-v0/OSVVM/OSVVM.git' does not exist
      fatal: clone of '/Users/Jason/.cache/uv/git-v0/OSVVM/OSVVM.git' into submodule path '/Users/Jason/.cache/uv/git-v0/checkouts/9b32958e04bf51f8/acf7e7f2/vunit/vhdl/osvvm' failed
      Failed to clone 'vunit/vhdl/osvvm'. Retry scheduled
      fatal: repository '/Users/Jason/.cache/uv/git-v0/Paebbels/JSON-for-VHDL.git' does not exist
      fatal: clone of '/Users/Jason/.cache/uv/git-v0/Paebbels/JSON-for-VHDL.git' into submodule path '/Users/Jason/.cache/uv/git-v0/checkouts/9b32958e04bf51f8/acf7e7f2/vunit/vhdl/JSON-for-VHDL' failed
      Failed to clone 'vunit/vhdl/JSON-for-VHDL' a second time, aborting
```

Which works fine if I instead use `pip`. I hope a fix (maybe #12156) can be merged soon!

---
