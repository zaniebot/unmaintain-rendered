---
number: 15032
title: "uv tool install: \"--reinstall\" option broken"
type: issue
state: open
author: hwine
labels:
  - question
assignees: []
created_at: 2025-08-02T19:08:27Z
updated_at: 2025-08-12T16:27:59Z
url: https://github.com/astral-sh/uv/issues/15032
synced_at: 2026-01-10T01:25:51Z
---

# uv tool install: "--reinstall" option broken

---

_Issue opened by @hwine on 2025-08-02 19:08_

### Summary

`uv tool install --help` shows an option `--reinstall` to reinstall _all_ packages. However, this option requires a single package:

```bash
❯ uv tool install --help | grep -- --reinstall
      --reinstall                              Reinstall all packages, regardless of whether they're already installed. Implies `--refresh`
      --reinstall-package <REINSTALL_PACKAGE>  Reinstall a specific package, regardless of whether it's already installed. Implies

❯ uv tool install --reinstall
error: the following required arguments were not provided:
  <PACKAGE>

Usage: uv.exe tool install --reinstall <PACKAGE>

For more information, try '--help'.
```

Use case: trying to "fix" installation after removing the version of python they were installed with. (No warning or error was given during `uv python uninstall 3.13.2`.)

See also #14814, which speaks to more of conflicting information about this use case.

### Platform

Windows version: 10.0.26200.5722, arm64

### Version

uv 0.8.4 (e176e1714 2025-07-30)

### Python version

❯ uv python list | grep -v available 
> cpython-3.13.3-windows-aarch64-none C:\Users\hwine\AppData\Local\Programs\Python\Python313-arm64\python.exe 
> cpython-3.10.16-windows-x86_64-none                   C:\Users\hwine\AppData\Roaming\uv\python\cpython-3.10.16-windows-x86_64-none\python.exe

---

_Label `bug` added by @hwine on 2025-08-02 19:08_

---

_Comment by @charliermarsh on 2025-08-02 20:00_

I think `uv.exe tool install --reinstall <PACKAGE>` is saying that you need to pass `<PACKAGE>` to `uv tool install`. Like `uv tool install` on its own requires a package as a positional argument (like `uv tool install ruff`).

In the context of that CLI copy, "Reinstall all packages" means "Reinstall all the packages that are required for the command". E.g., `uv tool install flask --reinstall` would reinstall `flask` and all of its dependencies, as opposed to `uv tool install flask --reinstall-package click`, which would only reinstall `click`.

---

_Label `bug` removed by @charliermarsh on 2025-08-02 20:00_

---

_Label `question` added by @charliermarsh on 2025-08-02 20:00_

---

_Comment by @hwine on 2025-08-02 22:27_

> In the context of that CLI copy, "Reinstall all packages" means "Reinstall all the packages that are required for the command". E.g., `uv tool install flask --reinstall` would reinstall `flask` and all of its dependencies, as opposed to `uv tool install flask --reinstall-package click`, which would only reinstall `click`.

fair -- I think the documentation (`--help`) could be improved then, as it sounds like the term "package" has 2 different meanings in the context of this command:
- `<PACKAGE>` as referenced as a required parameter for the `install` command ("flask" in your example).
- `package` or `packages` as referenced in the help, which are only valid as part of options ("click" in your example). In many python contexts, these would be more likely to be referenced as "dependencies" or "requirements" of the main `<PACKAGE>`

I think changing "package" to "dependency" in the help text would be a no-code-change-needed to help.

---

_Comment by @mikeckennedy on 2025-08-12 01:09_

Hi guys. Not sure if piggybacking on this issue is the place for this or creating a new issue. 

Any time I uninstall a Python version that had previously been used install a tool, I can't run or update it. I can run reinstall for that tool. But I'm hoping there is a way to complete reinstall everything on my system. Basically I have this error if I clean out my system. How can I reset all of them and rebind them to the newer Python (say 3.134->3.13.5 after deleting 3.13.4):

<img width="759" height="310" alt="Image" src="https://github.com/user-attachments/assets/bf52915b-2e4a-4075-a7a8-a5499261e52d" />

---

_Comment by @zanieb on 2025-08-12 02:14_

@mikeckennedy for that, see https://github.com/astral-sh/uv/pull/12937. You probably want to use our transparent upgrade feature though: https://docs.astral.sh/uv/concepts/python-versions/#upgrading-python-versions

---

_Comment by @mikeckennedy on 2025-08-12 16:27_

Thanks @zanieb, that is related but not what I want. I don't want to be warned that it will remove the version of Python I'm using (I want that version removed). I want a way when seeing the screenshot above, to just tell uv "let's reinstall these with the latest Python." 

IIRC, there is `uv tool upgrade black --reinstall` but no way to say do that for everything I've currently have installed, but with the latest Python now that the older one is gone. There is also no way to say `uv tool install TOOL1 TOOL2` and have them both installed separately. I have to run separate commands to fix each error shown. This is error prone as I might forget to reinstall something.

I guess I'll just make a bash script to run `uv tool install TOOL1`, `uv tool install TOOL2`, ..., exhaustively until something gets smoothed out here.

Sorry if I'm missing a feature that already handles this. Thanks again for taking the time to respond.

---
