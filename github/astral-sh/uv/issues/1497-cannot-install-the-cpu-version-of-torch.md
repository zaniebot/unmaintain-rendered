---
number: 1497
title: Cannot install the CPU version of torch
type: issue
state: closed
author: Nishikoh
labels:
  - bug
assignees: []
created_at: 2024-02-16T14:57:19Z
updated_at: 2025-02-09T14:16:27Z
url: https://github.com/astral-sh/uv/issues/1497
synced_at: 2026-01-07T13:12:16-06:00
---

# Cannot install the CPU version of torch

---

_Issue opened by @Nishikoh on 2024-02-16 14:57_

I tried to install the CPU version of torch but could not.
```bash
uv pip install torch==2.1.0+cpu --find-links https://download.pytorch.org/whl/torch_stable.html
# and next command gives the same result.
uv pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu
```

```log
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.1.0+cpu and you require torch==2.1.0+cpu, we can conclude that the requirements are unsatisfiable.
```

uv version: v0.1.2
Python 3.11.7
Ubuntu 20.4
X86_64 Architecture

---
By the way, the install of the GPU version of torch is successful.
```bash
uv pip install torch==2.1.0
# success
```

---

_Comment by @charliermarsh on 2024-02-16 14:58_

Thanks! Will take a look.

---

_Label `bug` added by @charliermarsh on 2024-02-16 14:58_

---

_Comment by @zanieb on 2024-02-16 15:02_

Here's verbose output on my machine

```
❯ echo "torch==2.1.0+cpu" | uv pip compile --find-links https://download.pytorch.org/whl/torch_stable.html -
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.1.0+cpu and you require torch==2.1.0+cpu, we can conclude that the requirements are unsatisfiable.
❯ echo "torch==2.1.0+cpu" | uv pip compile --find-links https://download.pytorch.org/whl/torch_stable.html - -v
    0.000491s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /Users/mz/eng/src/astral-sh/uv/.venv
    0.000781s DEBUG uv_interpreter::interpreter Using cached markers for: /Users/mz/eng/src/astral-sh/uv/.venv/bin/python
    0.000791s DEBUG uv::commands::pip_compile Using Python 3.9.6 interpreter at /Users/mz/eng/src/astral-sh/uv/.venv/bin/python for builds
 uv_client::cached_client::get_serde 
   uv_client::cached_client::get_cacheable 
     uv_client::cached_client::read_and_parse_cache file=/Users/mz/Library/Caches/uv/flat-index-v0/html/76f6661a0037bb05.msgpack
        0.010378s   0ms  WARN uv_client::cached_client Broken cache entry at /Users/mz/Library/Caches/uv/flat-index-v0/html/76f6661a0037bb05.msgpack, removing: failed to open file `/Users/mz/Library/Caches/uv/flat-index-v0/html/76f6661a0037bb05.msgpack`
        0.010766s   0ms DEBUG uv_client::cached_client No cache entry for: https://download.pytorch.org/whl/torch_stable.html
     uv_client::cached_client::fresh_request url="https://download.pytorch.org/whl/torch_stable.html"
     uv_client::cached_client::new_cache file=/Users/mz/Library/Caches/uv/flat-index-v0/html/76f6661a0037bb05.msgpack
     uv_client::flat_index::parse_flat_index_html url=https://download.pytorch.org/whl/torch_stable.html
       uv_client::html::parse url=Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("download.pytorch.org")), port: None, path: "/whl/torch_stable.html", query: None, fragment: None }
    0.318613s DEBUG uv_client::flat_index Found 2243 packages in `--find-links` entry: https://download.pytorch.org/whl/torch_stable.html
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.322186s   1ms DEBUG uv_resolver::resolver Solving with target Python version 3.9.6
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.322468s   0ms DEBUG uv_resolver::resolver Adding direct dependency: torch==2.1.0+cpu
   uv_resolver::resolver::choose_version package=torch
     uv_resolver::resolver::package_wait package_name=torch
 uv_resolver::resolver::process_request request=Versions torch
   uv_client::registry_client::simple_api package=torch
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/mz/Library/Caches/uv/simple-v0/pypi/torch.rkyv
 uv_resolver::resolver::process_request request=Prefetch torch ==2.1.0+cpu
          0.323335s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/torch
 uv_resolver::version_map::from_metadata 
        0.324165s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of torch (==2.1.0+cpu)
      0.324187s   3ms DEBUG uv_resolver::resolver No compatible version found for: torch
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.1.0+cpu and you require torch==2.1.0+cpu, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @zanieb on 2024-02-16 15:24_

You can see these versions _are_ available at https://download.pytorch.org/whl/torch_stable.html

Adding a debug log and it looks like we're filtering these out before candidate selection

```
        0.382322s   4ms DEBUG uv_resolver::resolver Searching for a compatible version of torch (==2.1.0+cpu)
        0.382340s   4ms DEBUG uv_resolver::candidate_selector Checking 2.2.0
        0.382353s   4ms DEBUG uv_resolver::candidate_selector Checking 2.1.2
        0.382366s   4ms DEBUG uv_resolver::candidate_selector Checking 2.1.1
        0.382378s   4ms DEBUG uv_resolver::candidate_selector Checking 2.1.0
        0.382391s   4ms DEBUG uv_resolver::candidate_selector Checking 2.0.1
        0.382404s   4ms DEBUG uv_resolver::candidate_selector Checking 2.0.0
        0.382417s   4ms DEBUG uv_resolver::candidate_selector Checking 1.13.1
        0.382429s   4ms DEBUG uv_resolver::candidate_selector Checking 1.13.0
        0.382442s   4ms DEBUG uv_resolver::candidate_selector Checking 1.12.1
        0.382454s   4ms DEBUG uv_resolver::candidate_selector Checking 1.12.0
        0.382466s   4ms DEBUG uv_resolver::candidate_selector Checking 1.11.0
        0.382479s   4ms DEBUG uv_resolver::candidate_selector Checking 1.10.2
        0.382491s   4ms DEBUG uv_resolver::candidate_selector Checking 1.10.1
        0.382503s   4ms DEBUG uv_resolver::candidate_selector Checking 1.10.0
        0.382516s   4ms DEBUG uv_resolver::candidate_selector Checking 1.9.1
        0.382528s   4ms DEBUG uv_resolver::candidate_selector Checking 1.9.0
        0.382540s   4ms DEBUG uv_resolver::candidate_selector Checking 1.8.1
        0.382552s   4ms DEBUG uv_resolver::candidate_selector Checking 1.8.0
        0.382564s   4ms DEBUG uv_resolver::candidate_selector Checking 1.7.1
        0.382577s   4ms DEBUG uv_resolver::candidate_selector Checking 1.7.0
        0.382589s   4ms DEBUG uv_resolver::candidate_selector Checking 1.6.0
        0.382601s   4ms DEBUG uv_resolver::candidate_selector Checking 1.5.1
        0.382613s   4ms DEBUG uv_resolver::candidate_selector Checking 1.5.0
        0.382625s   4ms DEBUG uv_resolver::candidate_selector Checking 1.4.0
        0.382638s   4ms DEBUG uv_resolver::candidate_selector Checking 1.3.1
        0.382650s   4ms DEBUG uv_resolver::candidate_selector Checking 1.3.0.post2
        0.382663s   4ms DEBUG uv_resolver::candidate_selector Checking 1.3.0
        0.382675s   4ms DEBUG uv_resolver::candidate_selector Checking 1.2.0
        0.382687s   4ms DEBUG uv_resolver::candidate_selector Checking 1.1.0.post2
        0.382699s   4ms DEBUG uv_resolver::candidate_selector Checking 1.1.0
        0.382712s   4ms DEBUG uv_resolver::candidate_selector Checking 1.0.1.post2
        0.382725s   4ms DEBUG uv_resolver::candidate_selector Checking 1.0.1
        0.382737s   4ms DEBUG uv_resolver::candidate_selector Checking 1.0.0
        0.382749s   4ms DEBUG uv_resolver::candidate_selector Checking 0.4.1.post2
        0.382762s   4ms DEBUG uv_resolver::candidate_selector Checking 0.4.1
        0.382774s   4ms DEBUG uv_resolver::candidate_selector Checking 0.4.0
        0.382786s   4ms DEBUG uv_resolver::candidate_selector Checking 0.3.1
        0.382798s   4ms DEBUG uv_resolver::candidate_selector Checking 0.3.0.post4
        0.382812s   4ms DEBUG uv_resolver::candidate_selector Checking 0.3.0.post3
        0.382825s   4ms DEBUG uv_resolver::candidate_selector Checking 0.3.0.post2
        0.382841s   4ms DEBUG uv_resolver::candidate_selector Checking 0.3.0
        0.382854s   4ms DEBUG uv_resolver::candidate_selector Checking 0.2.0.post3
        0.382867s   4ms DEBUG uv_resolver::candidate_selector Checking 0.2.0.post2
        0.382881s   4ms DEBUG uv_resolver::candidate_selector Checking 0.2.0.post1
        0.382894s   4ms DEBUG uv_resolver::candidate_selector Checking 0.1.12.post2
        0.382907s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.12.post1
        0.382921s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.11.post5
        0.382933s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.11.post4
        0.382946s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.10.post2
        0.382960s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.10.post1
        0.382974s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.9.post2
        0.382987s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.9.post1
        0.383001s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.8.post1
        0.383015s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.7.post2
        0.383029s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.6.post22
        0.383042s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.6.post20
        0.383082s   5ms DEBUG uv_resolver::candidate_selector Checking 0.1.6.post17
```

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-16 15:25_

---

_Referenced in [open-webui/open-webui#758](../../open-webui/open-webui/pulls/758.md) on 2024-02-16 20:41_

---

_Comment by @justinh-rahb on 2024-02-16 20:43_

Ya there are definitely versions that should be installable available but I'm not getting any luck on Linux x86_64, worked fine on an M1 MacBook though.

```bash
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
```

---

_Comment by @Nishikoh on 2024-02-16 21:52_

Oops, I guess I didn't provide enough information about my environment. I use the following environment.

---
Python 3.11.7
Ubuntu 20.4
X86_64 Architecture

---

_Comment by @Nishikoh on 2024-02-16 22:01_

There is a way to install torch by typing the URL directly. I tried that too, but it failed.
```
$ uv pip install torch@https://download.pytorch.org/whl/cpu/torch-2.1.0%2Bcpu-cp311-cp311-linux_x86_64.whl#sha256=5954924ce74bc7e6a6c811e3fa4bdda9936d9889f6369fd068420c444bfd1cae

error: The wheel filename "torch-2.1.0%2Bcpu-cp311-cp311-linux_x86_64.whl" has an invalid version part: after parsing 2.1.0, found "%2Bcpu" after it, which is not part of a valid version
```

---

_Comment by @Nishikoh on 2024-02-16 22:06_

Converting the string "%2Bcpu" to "+cpu" and executing it produces another error.
```
$ uv pip install torch@https://download.pytorch.org/whl/cpu/torch-2.1.0+cpu-cp311-cp311-linux_x86_64.whl#sha256=5954924ce74bc7e6a6c811e3fa4bdda9936d9889f6369fd068420c444bfd1cae

error: Failed to download: torch @ https://download.pytorch.org/whl/cpu/torch-2.1.0+cpu-cp311-cp311-linux_x86_64.whl#sha256=5954924ce74bc7e6a6c811e3fa4bdda9936d9889f6369fd068420c444bfd1cae
  Caused by: HTTP status client error (403 Forbidden) for url (https://download.pytorch.org/whl/cpu/torch-2.1.0+cpu-cp311-cp311-linux_x86_64.whl#sha256=5954924ce74bc7e6a6c811e3fa4bdda9936d9889f6369fd068420c444bfd1cae)
```


---

_Comment by @charliermarsh on 2024-02-16 22:08_

I think part of the problem here is solved by https://github.com/astral-sh/uv/pull/1544 (but perhaps not all of it). Do you mind trying again once that's out later today?

---

_Comment by @Nishikoh on 2024-02-16 22:22_

Thank you for working on the problem!


> I think part of the problem here is solved by https://github.com/astral-sh/uv/pull/1544 (but perhaps not all of it). Do you mind trying again once that's out later today?

I'm actually going on a two-day trip starting today. So I won't be able to check it until after that. Sorry about that. If anyone is tracking this issue, I would be grateful if you could check on my behalf.

---

_Comment by @justinh-rahb on 2024-02-16 22:29_

> I'm actually going on a two-day trip starting today. So I won't be able to check it until after that. Sorry about that. If anyone is tracking this issue, I would be grateful if you could check on my behalf.

I got you fam 👌


---

_Comment by @charliermarsh on 2024-02-17 01:48_

Do you mind giving this at try (at your convenience) with v0.1.3?

---

_Comment by @justinh-rahb on 2024-02-17 04:31_

Just using the `uv` tool on my bare Linux machine is working now, but I'm still having a heck of a time making things work in Docker containers. Keeps trying to install `six` from the PyTorch index, which of course fails because there is no such package there. I did try to explicitly set the version of torch as well, no luck.

Verbose log: https://pastebin.com/SQh4cLQS

---

_Comment by @Nishikoh on 2024-02-19 02:45_

I have tried three different INSTALL methods and have confirmed that they all work 👏 

uv version: v0.1.4
```
$ uv pip install torch@https://download.pytorch.org/whl/cpu/torch-2.1.0%2Bcpu-cp311-cp311-linux_x86_64.whl#sha256=5954924ce74bc7e6a6c811e3fa4bdda9936d9889f6369fd068420c444bfd1cae

Resolved 9 packages in 4.46s
Downloaded 1 package in 4.10s
Installed 9 packages in 149ms
 + filelock==3.13.1
 + fsspec==2024.2.0
 + jinja2==3.1.3
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.1.0+cpu (from https://download.pytorch.org/whl/cpu/torch-2.1.0%2Bcpu-cp311-cp311-linux_x86_64.whl#sha256=5954924ce74bc7e6a6c811e3fa4bdda9936d9889f6369fd068420c444bfd1cae)
 + typing-extensions==4.9.0
```

The next two methods worked in the same way 👍 
```
$ uv pip install torch==2.1.0+cpu --index-url https://download.pytorch.org/whl/cpu
$ uv pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu
```

Great project and a big thank you to the maintainers!

---
edit

The following command failed
```
$ uv pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu
```

The following command succeeds
```
uv pip install torch==2.1.0+cpu --find-links https://download.pytorch.org/whl/torch_stable.html
```


---

_Comment by @Nishikoh on 2024-02-19 02:50_

> Verbose log: https://pastebin.com/SQh4cLQS

I tried to check this log on my end, but also failed. I tried explicitly specifying the version and cpu, but no luck.

```
$ uv pip install torchvision==0.16.1+cpu torchaudio==2.1.1+cpu torch==2.1.1+cpu --index-url https://download.pytorch.org/whl/cpu

  x No solution found when resolving dependencies:
  `-> Because torchvision==0.16.1+cpu depends on torch==2.1.1 and you require torch==2.1.1+cpu, we can conclude that root==0a0.dev0 and torchvision==0.16.1+cpu are incompatible.
      And because you require torchvision==0.16.1+cpu, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @baskervilski on 2024-02-19 14:08_

I have a similar problem when trying to use `uv pip compile`:

![image](https://github.com/astral-sh/uv/assets/7703701/db46cf10-25bd-4f27-a541-399bba7c210c)

In `pyproject.toml` I have the `+cpu` dependencies specifed, and they are available on our internal Artifactory (installation works without an issue), but the compilation fails for some reason...

EDIT: Using v0.1.5 btw.

EDIT2: Identical error and message happen when trying to install a package with `torch+cpu` dependencies, although "direct" installation of +cpu dependencies (`uv pip install ...`) works without issues.

---

_Comment by @zanieb on 2024-02-19 15:35_

Just to clarify, `uv pip install` works but `uv pip compile` does not? What's the exact `compile` invocation and input file?

---

_Comment by @baskervilski on 2024-02-19 17:22_

So.

- `uv pip install torch==2.1.1+cpu` => works 🟢 
- `uv pip install -e .` (where the local project has `torch==2.1.1+cpu` as a dependency) => fails. 🔴 
- `uv pip compile -o requirements.txt pyproject.toml` (where the local project has `torch==2.1.1+cpu` as a dependency) => fails. 🔴 

Error message (eviaea-api is my project):
![305975352-db46cf10-25bd-4f27-a541-399bba7c210c](https://github.com/astral-sh/uv/assets/7703701/ca722e3d-ed80-440a-ace2-480290d32b9c)


---

_Comment by @baskervilski on 2024-02-22 13:12_

Ok, found a workaround(?), using the `--override` option:

I created the `overrides.txt` file with the following content
```
# overrides.txt
torch==2.1.1+cpu
torchvision==0.16.1+cpu
```

Then called:
`uv pip compile pyproject.toml -o requirements.txt --override overrides.txt`
and
`uv pip install -e . --override overrides.txt`

For now I can live with this.

---

_Comment by @charliermarsh on 2024-02-22 13:25_

Thanks @baskervilski - that makes sense to me actually (that it works), I believe the bug here relates to our handling of local versions which we’ll need to fix.

---

_Referenced in [astral-sh/uv#1855](../../astral-sh/uv/issues/1855.md) on 2024-02-22 13:47_

---

_Comment by @BurntSushi on 2024-02-22 14:52_

OK, so I'm looking into this (and still catching up conceptually with how #1855 ties into this) and thought it might be helpful to just start sharing what I'm seeing. At least initially, I suspect I'm not sharing anything that @charliermarsh doesn't already know. :)

So what I did here was to try and isolate where the problem is coming from. I was able to get one of the commands shared above failing for me (I'm on Linux `x86-64`):

```
$ uv pip install torch==2.1.1 --index-url https://download.pytorch.org/whl/cpu
  x No solution found when resolving dependencies:
  `-> Because torch==2.1.1 is unusable because no wheels are available with a matching platform and you require torch==2.1.1, we can conclude that the requirements are unsatisfiable.
```

And specifically, `pip install` with the same command works fine:

```
$ pip install torch==2.1.1 --index-url https://download.pytorch.org/whl/cpu
Looking in indexes: https://download.pytorch.org/whl/cpu
Collecting torch==2.1.1
  Using cached https://download.pytorch.org/whl/cpu/torch-2.1.1%2Bcpu-cp311-cp311-linux_x86_64.whl (184.9 MB)
Collecting filelock (from torch==2.1.1)
  Using cached https://download.pytorch.org/whl/filelock-3.9.0-py3-none-any.whl (9.7 kB)
Collecting typing-extensions (from torch==2.1.1)
  Using cached https://download.pytorch.org/whl/typing_extensions-4.8.0-py3-none-any.whl (31 kB)
Collecting sympy (from torch==2.1.1)
  Using cached https://download.pytorch.org/whl/sympy-1.12-py3-none-any.whl (5.7 MB)
Collecting networkx (from torch==2.1.1)
  Using cached https://download.pytorch.org/whl/networkx-3.2.1-py3-none-any.whl (1.6 MB)
Collecting jinja2 (from torch==2.1.1)
  Using cached https://download.pytorch.org/whl/Jinja2-3.1.2-py3-none-any.whl (133 kB)
Collecting fsspec (from torch==2.1.1)
  Using cached https://download.pytorch.org/whl/fsspec-2023.4.0-py3-none-any.whl (153 kB)
Collecting MarkupSafe>=2.0 (from jinja2->torch==2.1.1)
  Using cached https://download.pytorch.org/whl/MarkupSafe-2.1.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (28 kB)
Collecting mpmath>=0.19 (from sympy->torch==2.1.1)
  Using cached https://download.pytorch.org/whl/mpmath-1.3.0-py3-none-any.whl (536 kB)
Installing collected packages: mpmath, typing-extensions, sympy, networkx, MarkupSafe, fsspec, filelock, jinja2, torch
Successfully installed MarkupSafe-2.1.3 filelock-3.9.0 fsspec-2023.4.0 jinja2-3.1.2 mpmath-1.3.0 networkx-3.2.1 sympy-1.12 torch-2.1.1+cpu typing-extensions-4.8.0

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: pip install --upgrade pip
```

So I started taking a look at why `uv` doesn't see `torch 2.1.1`. To start with, I wanted to see with what our `VersionMap` was being populated with. So I printed out debug info for wheels with versions that contain `2.1.1` in them. I got this:

```
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp310, abi tags: cp310, platform tags: manylinux_2_17_aarch64, manylinux2014_aarch64
    filename: "torch-2.1.1-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl#sha256=84fefd63356416c0cd20578637ccdbb82164993400ed17b57c951dd6376dcee8
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp310, abi tags: none, platform tags: macosx_10_9_x86_64
    filename: "torch-2.1.1-cp310-none-macosx_10_9_x86_64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp310-none-macosx_10_9_x86_64.whl#sha256=1e1e5faddd43a8f2c0e0e22beacd1e235a2e447794d807483c94a9e31b54a758
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp310, abi tags: none, platform tags: macosx_11_0_arm64
    filename: "torch-2.1.1-cp310-none-macosx_11_0_arm64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp310-none-macosx_11_0_arm64.whl#sha256=e76bf3c5c354874f1da465c852a2fb60ee6cbce306e935337885760f080f9baa
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp311, abi tags: cp311, platform tags: manylinux_2_17_aarch64, manylinux2014_aarch64
    filename: "torch-2.1.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl#sha256=61b51b33c61737c287058b0c3061e6a9d3c363863e4a094f804bc486888a188a
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp311, abi tags: none, platform tags: macosx_10_9_x86_64
    filename: "torch-2.1.1-cp311-none-macosx_10_9_x86_64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp311-none-macosx_10_9_x86_64.whl#sha256=a70593806f1d7e6b53657d96810518da0f88ef2608c98a402955765b8c79d52c
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp311, abi tags: none, platform tags: macosx_11_0_arm64
    filename: "torch-2.1.1-cp311-none-macosx_11_0_arm64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp311-none-macosx_11_0_arm64.whl#sha256=e312f7e82e49565f7667b0bbf9559ab0c597063d93044740781c02acd5a87978
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp38, abi tags: cp38, platform tags: manylinux_2_17_aarch64, manylinux2014_aarch64
    filename: "torch-2.1.1-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl#sha256=9ca0fcbf3d5ba644d6a8572c83a9abbdf5f7ff575bc38529ef6c185a3a71bde9
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp38, abi tags: none, platform tags: macosx_10_9_x86_64
    filename: "torch-2.1.1-cp38-none-macosx_10_9_x86_64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp38-none-macosx_10_9_x86_64.whl#sha256=d56b032176458e2af4709627bbd2c20fe2917eff8cd087a7fe313acccf5ce2f1
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp38, abi tags: none, platform tags: macosx_11_0_arm64
    filename: "torch-2.1.1-cp38-none-macosx_11_0_arm64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp38-none-macosx_11_0_arm64.whl#sha256=29e3b90a8c281f6660804a939d1f4218604c80162e521e1e6d8c8557325902a0
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp39, abi tags: cp39, platform tags: manylinux_2_17_aarch64, manylinux2014_aarch64
    filename: "torch-2.1.1-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl#sha256=b31230bd058424e56dba7f899280dbc6ac8b9948e43902e0c84a44666b1ec151
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp39, abi tags: none, platform tags: macosx_10_9_x86_64
    filename: "torch-2.1.1-cp39-none-macosx_10_9_x86_64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp39-none-macosx_10_9_x86_64.whl#sha256=715b50d8c1de5da5524a68287eb000f73e026e74d5f6b12bc450ef6995fcf5f9
"2.1.1": PackageName("torch"), wheel version: "2.1.1" python tags: cp39, abi tags: none, platform tags: macosx_11_0_arm64
    filename: "torch-2.1.1-cp39-none-macosx_11_0_arm64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1-cp39-none-macosx_11_0_arm64.whl#sha256=db67e8725c76f4c7f4f02e7551bb16e81ba1a1912867bc35d7bb96d2be8c78b4



"2.1.1+cpu": PackageName("torch"), wheel version: "2.1.1+cpu" python tags: cp310, abi tags: cp310, platform tags: linux_x86_64
    filename: "torch-2.1.1+cpu-cp310-cp310-linux_x86_64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1%2Bcpu-cp310-cp310-linux_x86_64.whl#sha256=16e6cdb1991ff1f15f469b12fffb5b20f00b1ecff8c1073c23c7760fba90fcbc
"2.1.1+cpu": PackageName("torch"), wheel version: "2.1.1+cpu" python tags: cp310, abi tags: cp310, platform tags: win_amd64
    filename: "torch-2.1.1+cpu-cp310-cp310-win_amd64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1%2Bcpu-cp310-cp310-win_amd64.whl#sha256=8e4e47aa4a004d2f5edd516b296da6fba07981ae9d1fc0da862abf651dc07d96
"2.1.1+cpu": PackageName("torch"), wheel version: "2.1.1+cpu" python tags: cp311, abi tags: cp311, platform tags: linux_x86_64
    filename: "torch-2.1.1+cpu-cp311-cp311-linux_x86_64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1%2Bcpu-cp311-cp311-linux_x86_64.whl#sha256=d83b13cb17544f9851cc31fed197865eae0c0f5d32df9d8d6d8535df7d2e5109
"2.1.1+cpu": PackageName("torch"), wheel version: "2.1.1+cpu" python tags: cp311, abi tags: cp311, platform tags: win_amd64
    filename: "torch-2.1.1+cpu-cp311-cp311-win_amd64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1%2Bcpu-cp311-cp311-win_amd64.whl#sha256=23be0cb945970443c97d4f9ea61ed03b27f924d835de689dd4134f30966c13f7
"2.1.1+cpu": PackageName("torch"), wheel version: "2.1.1+cpu" python tags: cp38, abi tags: cp38, platform tags: linux_x86_64
    filename: "torch-2.1.1+cpu-cp38-cp38-linux_x86_64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1%2Bcpu-cp38-cp38-linux_x86_64.whl#sha256=9399a8dfe4833bb544e7a0c332c47db1e389a06b4ae5ac9b3b167d863adc95d9
"2.1.1+cpu": PackageName("torch"), wheel version: "2.1.1+cpu" python tags: cp38, abi tags: cp38, platform tags: win_amd64
    filename: "torch-2.1.1+cpu-cp38-cp38-win_amd64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1%2Bcpu-cp38-cp38-win_amd64.whl#sha256=d8e19bb465e1fa15f3231ff57bbf0a673e5dcaca39eee4f58a4be7832b80c9da
"2.1.1+cpu": PackageName("torch"), wheel version: "2.1.1+cpu" python tags: cp39, abi tags: cp39, platform tags: linux_x86_64
    filename: "torch-2.1.1+cpu-cp39-cp39-linux_x86_64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1%2Bcpu-cp39-cp39-linux_x86_64.whl#sha256=b2cc98815251f8a2d102c2f8f4afe8304c2df61ce9c237198032c7903d97fdbb
"2.1.1+cpu": PackageName("torch"), wheel version: "2.1.1+cpu" python tags: cp39, abi tags: cp39, platform tags: win_amd64
    filename: "torch-2.1.1+cpu-cp39-cp39-win_amd64.whl"
    requires_python: None
    url: /whl/cpu/torch-2.1.1%2Bcpu-cp39-cp39-win_amd64.whl#sha256=b09d431c8e53511b6c3624f1d79cce9cd28f4c27ca20c5a49e4dcc1f9e7377e5
```

In the `2.1.1` case, notice that none of them match my particular platform. They're all either macOS (either `x86-64` or `aarch64`) or Linux (only `aarch64`). But in the `2.1.1+cpu` case, there are wheels that match my platform.

So the next thing I wanted to do was check to see whether the `2.1.1+cpu` version was ever being requested, and if so, whether any of its wheels were found to be compatible:

```
lazy init for: "2.1.1"
/whl/cpu/torch-2.1.1-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl#sha256=84fefd63356416c0cd20578637ccdbb82164993400ed17b57c951dd6376dcee8 :: Incompatible(Tag(Abi))
/whl/cpu/torch-2.1.1-cp310-none-macosx_10_9_x86_64.whl#sha256=1e1e5faddd43a8f2c0e0e22beacd1e235a2e447794d807483c94a9e31b54a758 :: Incompatible(Tag(Abi))
/whl/cpu/torch-2.1.1-cp310-none-macosx_11_0_arm64.whl#sha256=e76bf3c5c354874f1da465c852a2fb60ee6cbce306e935337885760f080f9baa :: Incompatible(Tag(Abi))
/whl/cpu/torch-2.1.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl#sha256=61b51b33c61737c287058b0c3061e6a9d3c363863e4a094f804bc486888a188a :: Incompatible(Tag(Platform))
/whl/cpu/torch-2.1.1-cp311-none-macosx_10_9_x86_64.whl#sha256=a70593806f1d7e6b53657d96810518da0f88ef2608c98a402955765b8c79d52c :: Incompatible(Tag(Platform))
/whl/cpu/torch-2.1.1-cp311-none-macosx_11_0_arm64.whl#sha256=e312f7e82e49565f7667b0bbf9559ab0c597063d93044740781c02acd5a87978 :: Incompatible(Tag(Platform))
/whl/cpu/torch-2.1.1-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl#sha256=9ca0fcbf3d5ba644d6a8572c83a9abbdf5f7ff575bc38529ef6c185a3a71bde9 :: Incompatible(Tag(Abi))
/whl/cpu/torch-2.1.1-cp38-none-macosx_10_9_x86_64.whl#sha256=d56b032176458e2af4709627bbd2c20fe2917eff8cd087a7fe313acccf5ce2f1 :: Incompatible(Tag(Abi))
/whl/cpu/torch-2.1.1-cp38-none-macosx_11_0_arm64.whl#sha256=29e3b90a8c281f6660804a939d1f4218604c80162e521e1e6d8c8557325902a0 :: Incompatible(Tag(Abi))
/whl/cpu/torch-2.1.1-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl#sha256=b31230bd058424e56dba7f899280dbc6ac8b9948e43902e0c84a44666b1ec151 :: Incompatible(Tag(Abi))
/whl/cpu/torch-2.1.1-cp39-none-macosx_10_9_x86_64.whl#sha256=715b50d8c1de5da5524a68287eb000f73e026e74d5f6b12bc450ef6995fcf5f9 :: Incompatible(Tag(Abi))
/whl/cpu/torch-2.1.1-cp39-none-macosx_11_0_arm64.whl#sha256=db67e8725c76f4c7f4f02e7551bb16e81ba1a1912867bc35d7bb96d2be8c78b4 :: Incompatible(Tag(Abi))
```

So `2.1.1` is requested (by the resolver), and as expected, none of its wheels are considered compatible. But crucially, `2.1.1+cpu` is never even requested in the first place. My next step is to understand why that is.

---

_Comment by @BurntSushi on 2024-02-22 15:02_

Candidate selection believes `2.1.1+cpu` does not satisfy `==2.1.1`:

```
[andrew@duff i1497]$ RUST_LOG=trace uv pip install torch==2.1.1 --index-url https://download.pytorch.org/whl/cpu
⠙ Resolving dependencies...                                                                                                                                                                                                             SELECTING CANDIDATE FOR PackageName("torch") and range Range { segments: [(Included("2.1.1"), Included("2.1.1"))] }
select candidate: "2.2.1+cpu"
    version not in range
select candidate: "2.2.1"
    version not in range
select candidate: "2.2.0+cpu"
    version not in range
select candidate: "2.2.0"
    version not in range
select candidate: "2.1.2+cpu"
    version not in range
select candidate: "2.1.2"
    version not in range
select candidate: "2.1.1+cpu"
    version not in range
select candidate: "2.1.1"
lazy init for: "2.1.1"
SELECTING CANDIDATE FOR PackageName("torch") and range Range { segments: [(Included("2.1.1"), Included("2.1.1"))] }
select candidate: "2.2.1+cpu"
    version not in range
select candidate: "2.2.1"
    version not in range
select candidate: "2.2.0+cpu"
    version not in range
select candidate: "2.2.0"
    version not in range
select candidate: "2.1.2+cpu"
    version not in range
select candidate: "2.1.2"
    version not in range
select candidate: "2.1.1+cpu"
    version not in range
select candidate: "2.1.1"
```

(Again, I think @charliermarsh already figured this out. But I'm confirming.)

---

_Comment by @BurntSushi on 2024-02-27 14:22_

@Nishikoh I've noticed that while `uv-main pip install 'torch==2.1.1' --index-url https://download.pytorch.org/whl/cpu` fails for me, `uv-main pip install 'torch==2.1.1+cpu' --index-url https://download.pytorch.org/whl/cpu` succeeds. Can you confirm for me that the latter still fails for you?

Does it fail for anyone else?

---

_Comment by @Nishikoh on 2024-02-27 15:39_

@BurntSushi 
I ran the two commands and confirmed that the latter was successful.
```
# uv v0.1.11
root@bad8609641ae:/# uv pip install 'torch==2.1.1' --index-url https://download.pytorch.org/whl/cpu
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.1.1 is unusable because no wheels are available with a matching platform and you require torch==2.1.1, we can
      conclude that the requirements are unsatisfiable.


root@bad8609641ae:/# uv pip install 'torch==2.1.1+cpu' --index-url https://download.pytorch.org/whl/cpu
Resolved 9 packages in 3.59s
Downloaded 9 packages in 9.92s
Installed 9 packages in 211ms
 + filelock==3.9.0
 + fsspec==2023.4.0
 + jinja2==3.1.2
 + markupsafe==2.1.3
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.1.1+cpu
 + typing-extensions==4.8.0
```

My past comment may have been wrong and confused you.
https://github.com/astral-sh/uv/issues/1497#issuecomment-1951600402

---

_Referenced in [astral-sh/uv#2080](../../astral-sh/uv/issues/2080.md) on 2024-02-29 13:55_

---

_Unassigned @BurntSushi by @BurntSushi on 2024-02-29 15:29_

---

_Referenced in [kornia/workflows#15](../../kornia/workflows/pulls/15.md) on 2024-03-11 21:45_

---

_Referenced in [astral-sh/uv#2430](../../astral-sh/uv/pulls/2430.md) on 2024-03-15 14:48_

---

_Referenced in [astral-sh/uv#2478](../../astral-sh/uv/issues/2478.md) on 2024-03-15 19:26_

---

_Comment by @charliermarsh on 2024-03-20 04:18_

This should work as of the latest release:

```
uv pip install torch==2.1.0+cpu --find-links https://download.pytorch.org/whl/torch_stable.html
```

---

_Closed by @charliermarsh on 2024-03-20 04:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-20 04:18_

---

_Comment by @johnnv1 on 2024-03-20 11:00_

> This should work as of the latest release:

@charliermarsh do you have any plans to add support for the most common case? 
`uv pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu`


---

_Referenced in [astral-sh/uv#2655](../../astral-sh/uv/issues/2655.md) on 2024-03-25 19:12_

---

_Comment by @baggiponte on 2024-05-07 08:16_

> uv pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu

This works for me! py3.10 and latest `uv` 0.1.35.

> This should work as of the latest release:
> 
> ```
> uv pip install torch==2.1.0+cpu --find-links https://download.pytorch.org/whl/torch_stable.html
> ```

On the other hand, this raises:

```
uv pip install torch==2.1.0+cpu --find-links https://download.pytorch.org/whl/torch_stable.html
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.1.0+cpu is unusable because no wheels are available with a matching Python implementation and you require torch==2.1.0+cpu, we can conclude that the requirements are unsatisfiable.
```

EDIT: for this last command I also tried with the latest stable `uv`, 0.1.39.

---

_Comment by @charliermarsh on 2024-05-07 13:05_

Can you confirm that the environment you're in is using Python 3.10, and that you're on x86 Linux or Windows?

---

_Comment by @yashgorana on 2024-05-07 16:52_

The problem here is the behavior for installing CPU-only torch is different on arm64/aarch64 and x86_64/amd64 systems.
- amd64 requires us to explicitly provide `torch==x.x.x+cpu`
- meanwhile for arm64 we don't need the moniker.

On amd64, 
```shell
$ arch
x86_64


$ uv pip install torch==2.3.0 --index-url https://download.pytorch.org/whl/cpu
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.3.0 is unusable because no wheels are available with a matching Python ABI and you require torch==2.3.0, we can conclude that the requirements are unsatisfiable.


# with +cpu moniker
$ uv pip install torch==2.3.0+cpu --index-url https://download.pytorch.org/whl/cpu
Resolved 9 packages in 4.33s
Downloaded 9 packages in 2.90s
Installed 9 packages in 114ms
 + filelock==3.13.1
 + fsspec==2024.2.0
 + jinja2==3.1.3
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.3.0+cpu
 + typing-extensions==4.9.0
```


On arm64, adding the `+cpu` fails & without it installs.
```shell
~ arch
arm64


# with +cpu moniker
~ uv pip install torch==2.3.0+cpu --index-url https://download.pytorch.org/whl/cpu
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.3.0+cpu is unusable because no wheels are available with a matching Python ABI and you require torch==2.3.0+cpu, we can conclude that the requirements are
      unsatisfiable.


# without the moniker
~ uv pip install torch==2.3.0 --index-url https://download.pytorch.org/whl/cpu
Resolved 9 packages in 3.58s
Installed 9 packages in 210ms
 + filelock==3.13.1
 + fsspec==2024.2.0
 + jinja2==3.1.3
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.3.0
 + typing-extensions==4.9.0
```

uv has known limitations around [local version identifiers](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers) but unsure why pip is able to make this work the way pytorch has documented https://pytorch.org/get-started/previous-versions/#v222 but uv can't

---

_Referenced in [astral-sh/uv#3437](../../astral-sh/uv/issues/3437.md) on 2024-05-08 02:59_

---

_Referenced in [OpenMined/PySyft#8605](../../OpenMined/PySyft/pulls/8605.md) on 2024-05-08 08:18_

---

_Comment by @baggiponte on 2024-05-08 11:06_

> Can you confirm that the environment you're in is using Python 3.10, and that you're on x86 Linux or Windows?

Ops, sorry, I am on ARM macOS. You are right.

---

_Comment by @polarathene on 2024-05-09 08:50_

Perhaps it's due to a lack of supported variants that they omitted the local identifier there? (_[here upstream PyTorch states they don't intend to publish `aarch64` variants with GPU accel](https://github.com/pytorch/pytorch/issues/110791#issuecomment-1753315240)_)

```bash
# Torch 2.3.0 + Python 3.12 Linux x86_64/aarch64 wheels

# +cpu
# https://download.pytorch.org/whl/cpu/torch/
torch-2.3.0+cpu-cp312-cp312-linux_x86_64.whl
# No local identifier for arm64:
torch-2.3.0-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl

# +cu121
# https://download.pytorch.org/whl/cu121/torch/
torch-2.3.0+cu121-cp312-cp312-linux_x86_64.whl
```

## Reference

Using Docker from an AMD64 machine to test ARM64 environment:

```console
# Base environment:
$ arch
x86_64
$ docker run --rm -it --platform linux/arm64 --workdir /tmp fedora:40 bash
$ arch
aarch64

# Install uv:
$ curl -LsSf https://astral.sh/uv/install.sh | sh && source $HOME/.cargo/env
# Create and enter venv:
$ uv venv && source .venv/bin/activate

# Install:
$ uv pip install \
    --extra-index-url https://download.pytorch.org/whl/cpu \
    torch torchvision torchaudio

Resolved 13 packages in 20.18s
Downloaded 13 packages in 12.26s
Installed 13 packages in 486ms
 + filelock==3.13.1
 + fsspec==2024.2.0
 + jinja2==3.1.3
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.2.1
 + numpy==1.26.3
 + pillow==10.2.0
 + sympy==1.12
 + torch==2.3.0
 + torchaudio==2.3.0
 + torchvision==0.18.0
 + typing-extensions==4.9.0
```

---

You can kind of see why PyTorch has local identifier variants when comparing to PyPi?:

```console
# ARM64 (PyPi instead of PyTorch)
# Slightly newer versions of the packages but otherwise equivalent
$ uv pip install torch torchvision torchaudio

Resolved 13 packages in 1.08s
Downloaded 13 packages in 23.73s
Installed 13 packages in 425ms
 + filelock==3.14.0
 + fsspec==2024.3.1
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.3
 + numpy==1.26.4
 + pillow==10.3.0
 + sympy==1.12
 + torch==2.3.0
 + torchaudio==2.3.0
 + torchvision==0.18.0
 + typing-extensions==4.11.0
```

```console
# AMD64 (PyPi instead of PyTorch)
# Defaults to latest cuda variant (+cu121)
# Same nvidia specific deps as PyTorch +cu121, except for nvidia-nvjitlink-cu12 (Cuda 12.4 here instead of 12.1)
$ uv pip install torch torchvision torchaudio

Resolved 25 packages in 558ms
Downloaded 25 packages in 53.62s
Installed 25 packages in 189ms
 + filelock==3.14.0
 + fsspec==2024.3.1
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.3
 + numpy==1.26.4
 + nvidia-cublas-cu12==12.1.3.1
 + nvidia-cuda-cupti-cu12==12.1.105
 + nvidia-cuda-nvrtc-cu12==12.1.105
 + nvidia-cuda-runtime-cu12==12.1.105
 + nvidia-cudnn-cu12==8.9.2.26
 + nvidia-cufft-cu12==11.0.2.54
 + nvidia-curand-cu12==10.3.2.106
 + nvidia-cusolver-cu12==11.4.5.107
 + nvidia-cusparse-cu12==12.1.0.106
 + nvidia-nccl-cu12==2.20.5
 + nvidia-nvjitlink-cu12==12.4.127
 + nvidia-nvtx-cu12==12.1.105
 + pillow==10.3.0
 + sympy==1.12
 + torch==2.3.0
 + torchaudio==2.3.0
 + torchvision==0.18.0
 + typing-extensions==4.11.0
```

---

## Resolve platform packaging inconsistency upstream

Maybe open an issue about it upstream since it seems to be more of a packaging concern unrelated to `uv`?:

https://github.com/pytorch/pytorch/issues/110791#issuecomment-1753334468

> _please do not hesitate to open a new issue (or a PR) if you want to propose dropping `+cpu` suffix for Linux and Windows wheels, as nobody seems to be relying on the old installation method anyway._

Which makes sense that CPU should be the default, with local identifiers only used for actual variants (_eg: different cuda version support_). Might introduce some friction to any existing CI perhaps, but you can see similar with nvidia's own Docker images they publish where the naming convention isn't always consistent 😅 (_at least it broke a PyTorch docker build CI task recently_)

---

_Comment by @njzjz on 2024-06-22 07:36_

> do you have any plans to add support for the most common case?
> `uv pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu`

It seems to me there is a workaround by appending `.*` to the public version:

```sh
uv pip install torch==2.3.1.* --extra-index-url https://download.pytorch.org/whl/cpu
Resolved 9 packages in 295ms
Installed 9 packages in 336ms
 + filelock==3.13.1
 + fsspec==2024.2.0
 + jinja2==3.1.3
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.3.1+cpu
 + typing-extensions==4.9.0
```

According to [PEP 440](https://peps.python.org/pep-0440/#version-matching), `.*` means the prefix matching.


---

_Comment by @polarathene on 2024-06-23 05:14_

> It seems to me there is a workaround by appending `.*` to the public version:

That's not very reliable. Try to bring in other torch packages for example:

```console
$ uv pip install 'torch==2.3.1.*' torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.3.1 has no wheels are available with a matching Python ABI and torchaudio==2.3.1+cpu depends on torch==2.3.1, we can conclude that torchaudio==2.3.1+cpu cannot be used. (1)

...

      And because torchaudio==2.2.2+cpu depends on torch==2.2.2 and torchaudio==2.3.0 has no wheels are available with a matching Python ABI, we can conclude that torchaudio<2.3.0+cpu depends on one of:
          torch==2.2.0
          torch==2.2.1
          torch==2.2.2

      And because torchaudio==2.3.0+cpu depends on torch==2.3.0 and torchaudio==2.3.1 has no wheels are available with a matching Python ABI, we can conclude that torchaudio<2.3.1+cpu depends on one of:
          torch==2.2.0
          torch==2.2.1
          torch==2.2.2
          torch==2.3.0

      And because we know from (1) that torchaudio==2.3.1+cpu cannot be used, we can conclude that all versions of torchaudio depend on one of:
          torch==2.2.0
          torch==2.2.1
          torch==2.2.2
          torch==2.3.0

      And because you require torch>=2.3.1.dev0 and torchaudio, we can conclude that the requirements are unsatisfiable.
```

```console
$ uv pip install 'torch==2.3.1.*' 'torchvision==2.3.1.*' 'torchaudio==0.18.1.*' --index-url https://download.pytorch.org/whl/cpu

  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of torchvision are available:
          torchvision<2.3.1.dev0
          torchvision>=2.3.2.dev0
      and you require torchvision>=2.3.1.dev0,<2.3.2.dev0, we can conclude that the requirements are unsatisfiable.

      hint: torchvision was requested with a pre-release marker (e.g., torchvision>=2.3.1.dev0,<2.3.2.dev0), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

While separately, it resolves the desired package instead of `2.3.1.dev0`:

```console
$ uv pip install 'torch==2.3.1.*' --index-url https://download.pytorch.org/whl/cpu

Resolved 9 packages in 3.64s
Downloaded 9 packages in 7.97s
Installed 9 packages in 307ms
 + filelock==3.13.1
 + fsspec==2024.2.0
 + jinja2==3.1.3
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.3.1+cpu
 + typing-extensions==4.9.0
```

---

_Referenced in [astral-sh/uv#7144](../../astral-sh/uv/issues/7144.md) on 2024-09-06 22:46_

---

_Comment by @mgrachten on 2024-11-05 10:54_

I encountered a version of this issue today, installing torchaudio cpuonly as per pytorch instructions.  Pip without uv succeeds:
```
pip install torch torchaudio --index-url https://download.pytorch.org/whl/cpu
```
but 

```
uv pip install torch torchaudio --index-url https://download.pytorch.org/whl/cpu
```
Fails (error log attached).

Versions:
* `Python 3.11.9 (main, Jul 12 2024, 14:48:03) [GCC 11.4.0] on linux`
* `uv-0.4.30-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`

[out.log](https://github.com/user-attachments/files/17631184/out.log)


---

_Comment by @petteriTeikari on 2025-02-09 03:46_

Hi @charliermarsh _et al._

I was facing similar issues here now after following [your instructions ](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies) and trying to have the CPU and CUDA flags both working for laptop debugging and CUDA for actual running of the code.

* `uv sync --extra cu124` works
* `uv pip install torch==2.6.0+cpu --index-url https://download.pytorch.org/whl/cpu` works 
* `uv sync --extra cpu` fails with the following message:

```
error: Distribution `torch==2.6.0 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_35_x86_64`), but `torch` (v2.6.0) only has wheels for the following platform: `macosx_11_0_arm64`
```

I would say that [`torch-2.6.0+cpu.cxx11.abi-cp311-cp311-linux_x86_64.whl`]( https://download.pytorch.org/whl/torch/) seems like the correct wheel? Can I somehow force `uv` to use this wheel?


## My environment:

```
uv version: 0.5.29
Python 3.11.10
Ubuntu 22.4
X86_64 Architecture
```

## My `pyproject.toml`

```
[project]
requires-python = ">=3.11"
dependencies = [
   ...packages...
]

[project.optional-dependencies]
# e.g. torch-2.6.0+cpu.cxx11.abi-cp311-cp311-linux_x86_64.whl
cpu = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]
cu124 = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

## `uv.lock`

```
[[package]]
name = "torch"
version = "2.6.0"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "sys_platform == 'darwin'",
    "platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(platform_machine != 'aarch64' and sys_platform == 'linux') or (sys_platform != 'darwin' and sys_platform != 'linux')",
]
dependencies = [
    { name = "filelock", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "fsspec", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "jinja2", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "networkx", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cublas-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cuda-cupti-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cuda-nvrtc-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cuda-runtime-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cudnn-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cufft-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-curand-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cusolver-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cusparse-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cusparselt-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-nccl-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-nvjitlink-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-nvtx-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "sympy", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "triton", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "typing-extensions", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/78/a9/97cbbc97002fff0de394a2da2cdfa859481fdca36996d7bd845d50aa9d8d/torch-2.6.0-cp311-cp311-manylinux1_x86_64.whl", hash = "sha256:7979834102cd5b7a43cc64e87f2f3b14bd0e1458f06e9f88ffa386d07c7446e1", size = 766715424 },
    { url = "https://files.pythonhosted.org/packages/6d/fa/134ce8f8a7ea07f09588c9cc2cea0d69249efab977707cf67669431dcf5c/torch-2.6.0-cp311-cp311-manylinux_2_28_aarch64.whl", hash = "sha256:ccbd0320411fe1a3b3fec7b4d3185aa7d0c52adac94480ab024b5c8f74a0bf1d", size = 95759416 },
    { url = "https://files.pythonhosted.org/packages/11/c5/2370d96b31eb1841c3a0883a492c15278a6718ccad61bb6a649c80d1d9eb/torch-2.6.0-cp311-cp311-win_amd64.whl", hash = "sha256:46763dcb051180ce1ed23d1891d9b1598e07d051ce4c9d14307029809c4d64f7", size = 204164970 },
    { url = "https://files.pythonhosted.org/packages/0b/fa/f33a4148c6fb46ca2a3f8de39c24d473822d5774d652b66ed9b1214da5f7/torch-2.6.0-cp311-none-macosx_11_0_arm64.whl", hash = "sha256:94fc63b3b4bedd327af588696559f68c264440e2503cc9e6954019473d74ae21", size = 66530713 },
]
```

---

_Comment by @charliermarsh on 2025-02-09 14:16_

Do you mind creating a new issue so that I remember to follow-up?

---

_Referenced in [astral-sh/uv#11406](../../astral-sh/uv/issues/11406.md) on 2025-02-10 21:33_

---
