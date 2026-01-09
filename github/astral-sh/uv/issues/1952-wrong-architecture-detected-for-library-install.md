---
number: 1952
title: Wrong architecture detected for library install with Nix on M1
type: issue
state: closed
author: LazyYuuki
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2024-02-24T17:14:47Z
updated_at: 2024-03-19T02:08:27Z
url: https://github.com/astral-sh/uv/issues/1952
synced_at: 2026-01-07T13:12:16-06:00
---

# Wrong architecture detected for library install with Nix on M1

---

_Issue opened by @LazyYuuki on 2024-02-24 17:14_

I discover a niche problem with uv in my use case with Nix on MacOS M1 machine.

I first install uv using nix command:

```bash
nix profile install nixpkgs#uv
```

The version of uv on nix package is 0.1.8. Then I create a venv using uv:

```bash
uv venv --seed
```

And I activate the venv. I tried to open up a .ipynb in VSCode and install the ipykernel with:

```bash
uv pip install ipykernel
```

And try to run the code inside the Jupyter Notebook, but then I got this error.

```
The file '.venv/lib/python3.10/site-packages/psutil/_psutil_osx.abi3.so, 0x0002' seems to be overriding built in modules and interfering with the startup of the kernel. Consider renaming the file and starting the kernel again.
```

I then tried to install:

```bash
uv pip install jupyter
```

I have a problem where the package installed is in x86 format not arm64 so it is not compatible and cannot be run. The same happened with the Gradio package.

I delete the old .venv and created a brand new .venv this time I use pip inside the venv to install all the package, it run fine with the correct package architecture.

I then uninstall uv from my nix environtment, install uv using the method recommended on the page instead, and now everything work as intended.

I think there is something happened when uv is installed onto nix in MacOS M1 that caused it to mistake the arch as x86 meanwhile it is actually arm64. It is quite a niche problem, hope you can get a look into it as other might come pass it in the future.


---

_Comment by @charliermarsh on 2024-02-24 17:44_

Do you see the correct behavior if you install uv outside of nix? (We fixed this bug in https://github.com/astral-sh/uv/pull/1843, but that went out in v0.1.7, so perhaps there's something nix-specific there.)

---

_Label `needs-mre` added by @charliermarsh on 2024-02-24 17:44_

---

_Comment by @LazyYuuki on 2024-02-24 17:54_

> Do you see the correct behavior if you install uv outside of nix? (We fixed this bug in #1843, but that went out in v0.1.7, so perhaps there's something nix-specific there.)

Yes the correct behaviour is observed outside of nix using the method recommended to install for MacOS using the install.sh script. It only happened when I am trying to use uv inside nix or a nix shell.

---

_Comment by @charliermarsh on 2024-02-24 22:07_

Great, thanks! Sounds like it's time for me to install Nix...

---

_Label `bug` added by @charliermarsh on 2024-02-26 20:48_

---

_Referenced in [astral-sh/uv#2330](../../astral-sh/uv/issues/2330.md) on 2024-03-10 14:02_

---

_Comment by @charliermarsh on 2024-03-18 20:51_

I actually think this _might_ be fixed. Can you give it a try with the latest release?

---

_Comment by @LazyYuuki on 2024-03-19 02:08_

Hi @charliermarsh, it works!!!

Thanks a lot for the help. I have been recommending everyone that I know of and all the people in my workshop to use uv recently (albeit windows is a real headache with the path resolution for beginner in my workshop), absolutely love what you all are building. 

Keep up the good work, I am waiting for the Cargo for Python dream soon!

---

_Closed by @LazyYuuki on 2024-03-19 02:08_

---
