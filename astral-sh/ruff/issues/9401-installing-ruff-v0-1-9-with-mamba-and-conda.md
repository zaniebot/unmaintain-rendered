```yaml
number: 9401
title: Installing ruff v0.1.9+ with mamba and conda produces ModuleNotFoundError on Linux 
type: issue
state: closed
author: sogoodmo
labels:
  - bug
  - release
assignees: []
created_at: 2024-01-05T11:23:57Z
updated_at: 2024-01-16T13:50:56Z
url: https://github.com/astral-sh/ruff/issues/9401
synced_at: 2026-01-10T11:09:51Z
```

# Installing ruff v0.1.9+ with mamba and conda produces ModuleNotFoundError on Linux 

---

_Issue opened by @sogoodmo on 2024-01-05 11:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Mac - Works 
Linux - Fails 
Windows - Not tested 

- Importing ruff from mamba or conda on linux based machine and running python file that has `import ruff` produces `ModuleNotFound` error.
- Importing ruff from **pip** on linux based machines **does work**
- Testing with python file that includes only `import ruff` 
- Installed ruff by doing `mamba install ruff==0.1.9` **Note**: <0.1.8 versions don't produce this error
- Ruff version installed `0.1.9  py310h3d77a66_0  conda-forge/linux-64 `

Note:
- Using ruff in terminal to check a file does work on v0.1.9+ but importing it into my python file doesn't work.





---

_Renamed from "Installing ruff v0.1.9+ on mamba produces ModuleNotFoundError on Linux " to "Installing ruff v0.1.9+ from mamba produces ModuleNotFoundError on Linux " by @sogoodmo on 2024-01-05 11:25_

---

_Renamed from "Installing ruff v0.1.9+ from mamba produces ModuleNotFoundError on Linux " to "Installing ruff v0.1.9+ with mamba and conda produces ModuleNotFoundError on Linux " by @sogoodmo on 2024-01-05 11:30_

---

_Comment by @charliermarsh on 2024-01-05 15:58_

Interesting, need to see what changed here... The only thing I can see that possibly touched the release between v0.1.8 and v0.1.9 is https://github.com/astral-sh/ruff/pull/9031, but I don't see how it would've led to this error. Could something have changed in the conda-forge setup?

---

_Comment by @charliermarsh on 2024-01-05 15:58_

Hmm, no, nothing changed between those releases in https://github.com/conda-forge/ruff-feedstock.

---

_Comment by @charliermarsh on 2024-01-05 15:59_

Does `python -m ruff` work? I assume not.

---

_Label `release` added by @charliermarsh on 2024-01-05 15:59_

---

_Label `bug` added by @charliermarsh on 2024-01-05 15:59_

---

_Comment by @sogoodmo on 2024-01-05 16:14_

> Does `python -m ruff` work? I assume not.

Nope it doesn't :(. 

---

_Comment by @konstin on 2024-01-05 16:16_

The problem is that in the new release, the `ruff` folder isn't installed into the site-packages anymore, while the dist-info still is. In the env site-packages:

```console
$ micromamba install ruff==0.1.8
$ ls
_distutils_hack  distutils-precedence.pth  pip  pip-23.3.2-py3.12.egg-info  pkg_resources  README.txt  ruff  ruff-0.1.8.dist-info  setuptools  setuptools-69.0.3-py3.12.egg-info  wheel  wheel-0.42.0.dist-info
$ micromamba install ruff==0.1.9
$ ls
_distutils_hack  distutils-precedence.pth  pip  pip-23.3.2-py3.12.egg-info  pkg_resources  README.txt  ruff-0.1.9.dist-info  setuptools  setuptools-69.0.3-py3.12.egg-info  wheel  wheel-0.42.0.dist-info
```

I assume this is a conda bug since there is no , but it's hard to even find out the correct build commands to verify a regression.

---

_Comment by @sogoodmo on 2024-01-05 16:22_

Ah so is this not a ruff issue, but rather a conda bug? 

And is it just a coincidence that 0.1.9 has https://github.com/astral-sh/ruff/pull/9188 as a breaking change and the problem is related to ruff not being found in the site-packages?

---

_Comment by @konstin on 2024-01-05 16:29_

It's either a bug in the conda feedstock or in conda itself, i'm not a conda expert though; We build the wheel as basis for the conda package and we know that the wheel version works.

---

_Comment by @sogoodmo on 2024-01-16 11:04_

Hey just updating, there is an open issue https://github.com/conda-forge/ruff-feedstock/issues/153 here detailing the same problem

---

_Comment by @sogoodmo on 2024-01-16 12:10_

Update: This issue seems to be fixed with 0.1.13 version. We can close this.


![ruff-9-bad](https://github.com/astral-sh/ruff/assets/44754095/7b36f6fe-cc05-4ba8-a04b-22ee27038d8a)
![ruff-13-good](https://github.com/astral-sh/ruff/assets/44754095/1b03a1f2-c063-43b2-9071-ff59bd119e5e)


---

_Closed by @sogoodmo on 2024-01-16 13:00_

---

_Comment by @charliermarsh on 2024-01-16 13:50_

Good to hear, thanks for reporting back!

---
