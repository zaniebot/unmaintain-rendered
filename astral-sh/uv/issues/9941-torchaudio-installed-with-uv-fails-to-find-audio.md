---
number: 9941
title: "`torchaudio` installed with `uv` fails to find audio backend on macOS when debugging"
type: issue
state: closed
author: ThomasHezard
labels:
  - question
  - macos
assignees: []
created_at: 2024-12-16T17:00:13Z
updated_at: 2025-02-13T16:27:12Z
url: https://github.com/astral-sh/uv/issues/9941
synced_at: 2026-01-10T01:24:48Z
---

# `torchaudio` installed with `uv` fails to find audio backend on macOS when debugging

---

_Issue opened by @ThomasHezard on 2024-12-16 17:00_

Hello,

I've been struggling all day with a bug on my Python environments. After a full day of investigation, I think that the problem comes from `uv`, or at least that `uv` plays a role in it. 

However I do not understand why I did not encounter the issue before. I changed my computer recently so it might be related to it, but as explained below, the problem disappears entirely if I setup my Python environment using manual `venv` and `pip` rather than `uv`.

The problem is quite annoying as it makes step-by-step debugging impossible.

### How to reproduce

Create a project with the following `pyproject.toml`
```toml
[project]
name = "uv-torchaudio-test"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
  "torchaudio>=2.5"
]
```
and the following `.python-version`
```
3.10.16
```

Call `uv sync`, it should install `torchaudio` 2.5.1, current latest release.

The issue is clearly visible with the following simple script (make sure `sox` is installed with `Homebrew` before running the script):
```python
import torchaudio, shutil
print(shutil.which("sox"))
print(torchaudio.list_audio_backends())
```

- Running the script from terminal with `uv run python script.py` (or simply with `python script.py` after activating the environment manually) gives the following output (with possibly some warning related to `numpy` which can be avoided by adding `numpy` to your project):
  ```
  /opt/homebrew/bin/sox
  ['sox']
  ```
- Running the script in debug mode from VSCode (click on "_Python Debugger: debug Python File_") or PyCharm (click on _Run current file_ or _Debug current file_), after selecting the Python interpreter in the `.venv` directory created by `uv`, gives the following output:
  ```
  /opt/homebrew/bin/sox
  []
  ```
  In other words, `torchaudio` does not manage to intialize its `sox` backend, even though `sox` is in the `PATH`. The consequence is that any code using `sox` backend in my projects fails when I debug, but running the same scripts from console works fine.

### What I tried and why I think the problem comes from `uv`.

- If I setup the environment manually with the following procedure, debugging works perfectly fine:
  - install Python 3.10.16 with `pyenv`,
  - create the virtual environment with `python -m venv .venv`,
  - activate the environment `source .venv/bien/activate`,
  - install `torchaudio` using `pip`: `pip install torchaudio`,
  - debugging the script as described above works fine, "Sox" backend is initialized properly.
- Using the `--fork-strategy fewest` (major change introduced in `uv` 0.5.9)  when syncing does not solve the issue.

### In conclusion

I have no idea what can cause this 😮   

I thought the problem came from an issue on the setup of my new computer, but after solving the problem by installing my environment with `pip`, it seems `uv` is involved somehow.

I hope my explanations are clear, let me know if I can add/provide anything else.

Thank you in advance for the help 🙏 

---

_Comment by @samypr100 on 2024-12-17 02:57_

Is the issue only with debug mode with VSCode? Does it happen without VSCode debug mode?

Can you share the output of both `RUST_LOG=uv=trace uv run python script.py` and the same with your vscode setup?

---

_Label `question` added by @samypr100 on 2024-12-17 02:57_

---

_Label `needs-mre` added by @samypr100 on 2024-12-17 02:57_

---

_Comment by @ThomasHezard on 2024-12-17 08:57_

Hello,

Yes, this only happens in debug mode, in both VSCode and PyCharm (I did not test other IDEs).

Here is the trace from terminal (in this case, `torchaudio` works correctly)
```
❯❯❯ RUST_LOG=uv=trace uv run python torchaudio_test.py                                                                                                                                                                                                                                                     
DEBUG uv 0.5.9 (Homebrew 2024-12-13)
DEBUG Found project root: `XXX/uv-torchaudio`
DEBUG No workspace root found, using project root
DEBUG Discovered project `uv-torchaudio-test` at: XXX/uv-torchaudio
DEBUG Reading Python requests from version file at `XXX/uv-torchaudio/.python-version`
DEBUG Using Python request `3.10.16` from version file at `.python-version`
TRACE Cached interpreter info for Python 3.10.16, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies `3.10.16`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-torchaudio-test @ file://XXX/uv-torchaudio
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
DEBUG Using request timeout of 30s
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("numpy"), version: "2.2.0", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/numpy-2.2.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.2.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: numpy==2.2.0
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("torchaudio"), version: "2.5.1", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/torchaudio-2.5.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.5.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: torchaudio==2.5.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("torch"), version: "2.5.1", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/torch-2.5.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.5.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: torch==2.5.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("filelock"), version: "3.16.1", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/filelock-3.16.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.16.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: filelock==3.16.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("fsspec"), version: "2024.10.0", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/fsspec-2024.10.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2024.10.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: fsspec==2024.10.0
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("jinja2"), version: "3.1.4", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/jinja2-3.1.4.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.1.4" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: jinja2==3.1.4
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("networkx"), version: "3.4.2", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/networkx-3.4.2.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.4.2" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: networkx==3.4.2
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("sympy"), version: "1.13.1", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/sympy-1.13.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.13.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: sympy==1.13.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("typing-extensions"), version: "4.12.2", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/typing_extensions-4.12.2.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "4.12.2" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: typing-extensions==4.12.2
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("markupsafe"), version: "3.0.2", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/MarkupSafe-3.0.2.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.0.2" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: markupsafe==3.0.2
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("mpmath"), version: "1.3.0", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/mpmath-1.3.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.3.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: mpmath==1.3.0
DEBUG Using Python 3.10.16 interpreter at: XXX/uv-torchaudio/.venv/bin/python3
DEBUG Running `python torchaudio_test.py`
/opt/homebrew/bin/sox
['sox']
DEBUG Command exited with code: 0
```

I don't know how to integrate the `RUST_LOG=uv=trace` flag in the debug process of PyCharm or VSCode. The IDEs do not use `uv`, I setup my projects in both IDEs to use the Python interpreter in the `. venv` directory generated by `uv`.   
However, I noticed that VSCode launches its debugger the following way:
```bash
cd XXX/uv-torchaudio ; /usr/bin/env XXX/.venv/bin/python XXX/.vscode/extensions/ms-python.debugpy-2024.14.0-darwin-arm64/bundled/libs/debugpy/adapter/../../debugpy/launcher 49820 -- XXX/uv-torchaudio/torchaudio_test.py
```

I am not familiar with the `env` command, but if I do `/usr/bin/env uv run python torchaudio_test.py`, I have the same problem as with VSCode and Pycharm's debuggers, `torchaudio` is unable to build its `sox` backend event though the `sox` executable is found:
```
❯❯❯ RUST_LOG=uv=trace /usr/bin/env  uv run python torchaudio_test.py
DEBUG uv 0.5.9 (Homebrew 2024-12-13)
DEBUG Found project root: `XXX/uv-torchaudio`
DEBUG No workspace root found, using project root
DEBUG Discovered project `uv-torchaudio-test` at: XXX/uv-torchaudio
DEBUG Reading Python requests from version file at `XXX/uv-torchaudio/.python-version`
DEBUG Using Python request `3.10.16` from version file at `.python-version`
TRACE Cached interpreter info for Python 3.10.16, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies `3.10.16`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-torchaudio-test @ file://XXX/uv-torchaudio
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
DEBUG Using request timeout of 30s
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("numpy"), version: "2.2.0", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/numpy-2.2.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.2.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: numpy==2.2.0
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("torchaudio"), version: "2.5.1", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/torchaudio-2.5.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.5.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: torchaudio==2.5.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("torch"), version: "2.5.1", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/torch-2.5.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.5.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: torch==2.5.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("filelock"), version: "3.16.1", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/filelock-3.16.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.16.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: filelock==3.16.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("fsspec"), version: "2024.10.0", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/fsspec-2024.10.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2024.10.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: fsspec==2024.10.0
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("jinja2"), version: "3.1.4", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/jinja2-3.1.4.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.1.4" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: jinja2==3.1.4
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("networkx"), version: "3.4.2", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/networkx-3.4.2.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.4.2" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: networkx==3.4.2
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("sympy"), version: "1.13.1", path: "/XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/sympy-1.13.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.13.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: sympy==1.13.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("typing-extensions"), version: "4.12.2", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/typing_extensions-4.12.2.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "4.12.2" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: typing-extensions==4.12.2
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("markupsafe"), version: "3.0.2", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/MarkupSafe-3.0.2.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.0.2" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: markupsafe==3.0.2
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("mpmath"), version: "1.3.0", path: "XXX/uv-torchaudio/.venv/lib/python3.10/site-packages/mpmath-1.3.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.3.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: mpmath==1.3.0
DEBUG Using Python 3.10.16 interpreter at: XXX/uv-torchaudio/.venv/bin/python3
DEBUG Running `python torchaudio_test.py`
/opt/homebrew/bin/sox
[]
DEBUG Command exited with code: 0
```

It seems that the `/usr/bin/env` is what causes the issue in VSCode. I don't know exactly how PyCharm triggers its debug session so I don't know if the problem is similar in PyCharm.    

I don't know what to think about this. Do you have any idea what is going on?

This makes me think there is something wrong on my setup.   
But again, if I replace `uv` by a manual `venv` and `pip install`, no problem at all when I debug or when I launch the scrcipt using `/usr/bin/env python torchaudio_test.py`.

---

_Comment by @ThomasHezard on 2024-12-17 12:47_

If that can help, here is the environment installed by `pip` (Python 3.10.16 installed with `pyenv`, in the context of a venv created with `python -m venv .venv`), which makes the debuggers work fine.    
It seems that the resolution ends up with exactly the same packages versions.
```
❯❯❯ pip install numpy torchaudio
Collecting numpy
  Using cached numpy-2.2.0-cp310-cp310-macosx_14_0_arm64.whl.metadata (62 kB)
Collecting torchaudio
  Using cached torchaudio-2.5.1-cp310-cp310-macosx_11_0_arm64.whl.metadata (6.4 kB)
Collecting torch==2.5.1 (from torchaudio)
  Using cached torch-2.5.1-cp310-none-macosx_11_0_arm64.whl.metadata (28 kB)
Collecting filelock (from torch==2.5.1->torchaudio)
  Using cached filelock-3.16.1-py3-none-any.whl.metadata (2.9 kB)
Collecting typing-extensions>=4.8.0 (from torch==2.5.1->torchaudio)
  Using cached typing_extensions-4.12.2-py3-none-any.whl.metadata (3.0 kB)
Collecting networkx (from torch==2.5.1->torchaudio)
  Using cached networkx-3.4.2-py3-none-any.whl.metadata (6.3 kB)
Collecting jinja2 (from torch==2.5.1->torchaudio)
  Using cached jinja2-3.1.4-py3-none-any.whl.metadata (2.6 kB)
Collecting fsspec (from torch==2.5.1->torchaudio)
  Using cached fsspec-2024.10.0-py3-none-any.whl.metadata (11 kB)
Collecting sympy==1.13.1 (from torch==2.5.1->torchaudio)
  Using cached sympy-1.13.1-py3-none-any.whl.metadata (12 kB)
Collecting mpmath<1.4,>=1.1.0 (from sympy==1.13.1->torch==2.5.1->torchaudio)
  Using cached mpmath-1.3.0-py3-none-any.whl.metadata (8.6 kB)
Collecting MarkupSafe>=2.0 (from jinja2->torch==2.5.1->torchaudio)
  Using cached MarkupSafe-3.0.2-cp310-cp310-macosx_11_0_arm64.whl.metadata (4.0 kB)
Using cached numpy-2.2.0-cp310-cp310-macosx_14_0_arm64.whl (5.4 MB)
Using cached torchaudio-2.5.1-cp310-cp310-macosx_11_0_arm64.whl (1.8 MB)
Using cached torch-2.5.1-cp310-none-macosx_11_0_arm64.whl (63.9 MB)
Using cached sympy-1.13.1-py3-none-any.whl (6.2 MB)
Using cached typing_extensions-4.12.2-py3-none-any.whl (37 kB)
Using cached filelock-3.16.1-py3-none-any.whl (16 kB)
Using cached fsspec-2024.10.0-py3-none-any.whl (179 kB)
Using cached jinja2-3.1.4-py3-none-any.whl (133 kB)
Using cached networkx-3.4.2-py3-none-any.whl (1.7 MB)
Using cached MarkupSafe-3.0.2-cp310-cp310-macosx_11_0_arm64.whl (12 kB)
Using cached mpmath-1.3.0-py3-none-any.whl (536 kB)
Installing collected packages: mpmath, typing-extensions, sympy, numpy, networkx, MarkupSafe, fsspec, filelock, jinja2, torch, torchaudio
Successfully installed MarkupSafe-3.0.2 filelock-3.16.1 fsspec-2024.10.0 jinja2-3.1.4 mpmath-1.3.0 networkx-3.4.2 numpy-2.2.0 sympy-1.13.1 torch-2.5.1 torchaudio-2.5.1 typing-extensions-4.12.2
```

---

_Comment by @ThomasHezard on 2024-12-17 13:23_

One more interesting experiment.

My `.zprofile` file is quite heavy. In order to check if this was to cause of the problem, I tried to `uv sync` in a bash session (I don't have any `.bash_profile` or `.profile` file).

In this bash session with a much simpler and cleaner environment, when launching the Python script from terminal:
- environment created by `uv` (`uv sync`) => `torchaudio` can not load `sox` backend, even without any debugger involved, even without `/usr/bin/env`, simply calling `uv run python torchaudio_test.py` or `python torchaudio_test.py` after manually activating the venv created by `uv sync`
- environment created by `venv` and `pip` => `torchaudio` loads the `sox` backend without any issue.

---

_Comment by @ThomasHezard on 2024-12-18 09:32_

Yet a bit more context.

I opened [a discussion on torchaudio's forum](https://discuss.pytorch.org/t/please-help-torchaudio-does-not-find-the-sox-backend-on-macos/214313/2).   

The problem is that `torchaudio` fails to load `libsox.dylib`, which is made available by `homebrew` in `/opt/homebrew/lib.libsox.dylib`, when loading its own `libtorchaudio_sox.so`[here](https://github.com/pytorch/pytorch/blob/9283c40ba8e6adf55db3d12f7451d86bb9c68632/torch/_ops.py#L1356), with the following error:
```
('dlopen(/XXX/.venv/lib/python3.10/site-packages/torchaudio/lib/libtorchaudio_sox.so, 0x0006): Library not loaded: @rpath/libsox.dylib
  Referenced from: <56CD0903-8343-3EFF-AFBF-9F60216E7F09> /XXX/.venv/lib/python3.10/site-packages/torchaudio/lib/libtorchaudio_sox.so
  Reason: no LC_RPATH's found',)
```

Any idea why this is the case with `uv` (only in debug) and not with `venv`/`pip`?

---

_Comment by @ThomasHezard on 2024-12-20 18:08_

Hello, 

Any insight about my issue?
I just tested the latest version (0.5.11), no change.

The only workaround I found for now is to switch to `venv` and `pip` during debugging. Not very practical 😉

Thanks for your help 🙏

---

_Comment by @samypr100 on 2024-12-22 00:31_

Hmm, I tried the `/usr/bin/env` approach and didn't get issue with uv. I'm used Python 3.12.8 and uv 0.5.11.

```
$ /usr/bin/env uv run torchaudio_test.py
/opt/homebrew/bin/sox
['ffmpeg', 'sox']
```

---

_Comment by @ThomasHezard on 2024-12-22 08:39_

Hello,

Thank you for trying.

I finally found what seems to be the source of the issue.
It appears that [SIP](https://developer.apple.com/documentation/security/disabling-and-enabling-system-integrity-protection?language=objc) sanitizes the `DYLD_LIBRARY_PATH` environment variable when using `/usr/bin/env`. I tried to deactivate SIP and indeed it solves my problem (not sure I will keep this solution though, not the safest option...).

Still a few things I do not understand:
- Why I did not encounter the issue in the past? => This may be an evolution of macOS 15.
- Why does the environment works fine when created with `venv` and `pip` and not with `uv`? => Do you have any idea? This seems a bit problematic to me.

Thanks for your help 🙏 

---

_Label `needs-mre` removed by @samypr100 on 2024-12-22 17:03_

---

_Label `macos` added by @samypr100 on 2024-12-22 17:03_

---

_Comment by @ThomasHezard on 2025-01-05 20:07_

Hello,

Back on this issue after a few days far from my computer ^^

[TL;DR]    
The issue disappeared after uninstalling and re-installing `uv`'s Python interpreters.

You can close the issue if you think it is not relevant anymore. Do not hesitate to contact me if you want to investigate this further.

[More details]  
From what I understood after some time digging the web and debugging `import torchaudio` step-by-step, the problem came from an `@rpath` resolution failing when loading  `libtorchaudio_sox.so` —in the `torchaudio` Python package, inside my `.venv`—, which tries to load `@rpath/libsox.dylib` —located in `/opt/homebrew/lib` on my computer—.    

When checking with `otool -l`, I found out that the `LC_RPATH` entry with `/opt/homebrew/lib` for the `python` executables installed with `uv` was present on a colleague's computer with a setup very similar to mine, but not on mine.    

I deleted all my python interpreters installed with `uv` and re-installed them, and everything is now working fine (I did try to clean all the `uv` cache earlier but not the python interpreters themselves).

I am not sure what happened here. My guess is that there was something wrong in my `.zprofile` (libraries paths not setup correctly maybe) at the time I downloaded `python` interpreters with `uv`, and I corrected it later without realising it, as I modified my `.zprofile` a lot during the first few days with my new macBook to make everything work fine and fast on macOS 15. But I am not sure about that.

Unfortunately I cannot tell you with which `uv` version the interpreters were installed initially. I am currently using the latest 0.5.14 (installed with homebrew).

Thank you for your help, and sorry if this was a false alert making you loose time 🙏 

---

_Comment by @charliermarsh on 2025-01-05 20:10_

No worries. We made a bunch of improvements to the Python installs in the last few releases, so this might be something we actually fixed.

---

_Closed by @charliermarsh on 2025-01-05 20:10_

---

_Comment by @kingychiu on 2025-02-13 16:27_

I am adding one more case for this. I was also trying to setup torchaudio with sox or ffmpeg.

The backends cannot be loaded if my `uv run python script` is under a Makefile.
When I run directly on command line, the backends can be loaded.


---
