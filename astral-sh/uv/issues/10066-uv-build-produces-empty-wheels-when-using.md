```yaml
number: 10066
title: "`uv build` produces empty wheels when using hatchling (in some cases)"
type: issue
state: closed
author: jsnel-ct
labels:
  - question
  - external
assignees: []
created_at: 2024-12-20T20:52:17Z
updated_at: 2025-04-05T11:43:38Z
url: https://github.com/astral-sh/uv/issues/10066
synced_at: 2026-01-12T16:00:06Z
```

# `uv build` produces empty wheels when using hatchling (in some cases)

---

_@jsnel-ct_

When using `uv build` to produce distributable wheels I get an empty wheel, as opposed to when using hatch directly `uvx hatch build`. 

The issue reproduces (for me) on Windows, as well as WSL2, with `uv` versions up until `0.5.11`. 

This issue might be related to  this earlier reported [issue](https://github.com/astral-sh/uv/issues/8200).

I pushed a 'minimal' example repository along with reproduction instructions in the readme here:

- https://github.com/jsnel-ct/uv_build_pmt

In brief:

## Setup

```shell
git clone https://github.com/jsnel-ct/uv_build_pmt.git; cd uv_build_pmt
uv sync
# activate your venv (e.g. `.\.venv\Scripts\activate`)
# and verify this does not produce an error:
python -c 'import uv_build_pmt'
```

## Issue - observed behavior
After running the step in setup above, run:

```shell
uv build
uv pip install --force-reinstall .\dist\uv_build_pmt-0.1.0-py3-none-any.whl
python -c 'import uv_build_pmt'
```
And observe the error:
> ModuleNotFoundError: No module named 'uv_build_pmt'


## Expected behavior

When building directly with hatch(ling) 

```shell
uvx hatch build
uv pip install --force-reinstall .\dist\uv_build_pmt-0.1.0-py3-none-any.whl
python -c 'import uv_build_pmt'
```

This does not result in an import error, as I expect.


## Update

User @cofin provided a helpful hint in the right direction, which largely resolves this issue:

> [!TIP] 
> Make sure that your `pyproject.toml` does not have a section `[tool.hatch.build]` which includes the line `sources = ["src"]`. Check the [hatch documentation](https://hatch.pypa.io/1.13/config/build/) for up to date instructions. 


---

_Comment by @charliermarsh on 2024-12-20 20:53_

What does `uvx --from build pyproject-build` produce? If it also produces empty wheels, then it's likely a Hatch issue, not a uv issue.

---

_Label `question` added by @charliermarsh on 2024-12-20 20:53_

---

_Comment by @jsnel-ct on 2024-12-20 21:21_

This also produces an empty wheel (starting with the same reproduction environment described above).

```log
 uvx --from build pyproject-build
Installed 4 packages in 55ms
* Creating isolated environment: venv+pip...                                                                                                                                       
* Installing packages in isolated environment:                                                                                                                                     
  - hatchling
* Getting build dependencies for sdist...
* Building sdist...
* Building wheel from sdist
* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:
  - hatchling
* Getting build dependencies for wheel...
* Building wheel...
Successfully built uv_build_pmt-0.1.0.tar.gz and uv_build_pmt-0.1.0-py3-none-any.whl
```

Not sure if it was clear, but by 'empty' I mean that `uv_build_pmt-0.1.0-py3-none-any.whl` only contains:

```txt
└───uv_build_pmt-0.1.0.dist-info
    │   METADATA
    │   RECORD
    │   WHEEL
    │
    └───licenses
            LICENSE
```

The content of WHEEL being:
```
Wheel-Version: 1.0
Generator: hatchling 1.27.0
Root-Is-Purelib: true
Tag: py3-none-any
```


---

_Comment by @zanieb on 2024-12-20 21:25_

I'd recommend opening an issue with `hatch` then — both `uv` and `build` are operating as standards-compliant build frontends in this situation.

---

_Comment by @jsnel-ct on 2024-12-20 21:30_

Thank you for the quick feedback, I will file it with hatch.

Is there a reproducing scenario I can use that doesn't involve `uv`? If I install and use `hatch build` it works correctly, and also `uv run --isolated --with hatch hatch build` works correctly. How exactly does `uv` invoke hatch to build normally and can I emulate that directly on the CLI?

---

_Comment by @zanieb on 2024-12-20 21:39_

You can reproduce using a build frontend without uv

```
python -m pip install build
python -m build
```

@ofek will be familiar with that reproduction.

We invoke `hatchling` which is the `hatch` build backend, it's roughly `python -c "import hatchling.build; hatchling.build.build_wheel('.')"`

---

_Comment by @cofin on 2024-12-20 22:49_

I believe [this is related](https://github.com/astral-sh/uv/issues/8833#issuecomment-2466423582). 

Moving away from:
```toml
[tool.hatch.build]
only-include = ["src"]
sources = ["src"]
```
Was the solution in my case.

---

_Comment by @jsnel-ct on 2024-12-20 23:29_

Thank you! That **indeed** is the controlling variable!

Removing **either** one of those lines resulted in `uv build` building the right wheel. 

Probably the configuration that comes closest to what was expected (function wheel, clean sdist) is:
```toml
[tool.hatch.build]
only-include = ["src"]
```

But it does appear that hatch in [its documentation](https://hatch.pypa.io/1.13/config/build/) has moved toward recommending to specify build instruction on a per-target level. So more along these lines:

```toml
[tool.hatch.build.targets.wheel]
sources = ["src"]

[tool.hatch.build.targets.sdist]
only-include = ["src"]
```

For now - to help with the search engine or LLM bots scraping this - I'll just leave the TIP below, and leave it to the maintainers to decide how to close this issue. 

> [!TIP] 
> Make sure that your `pyproject.toml` does **not** have a section `[tool.hatch.build]` which includes the lines `sources = ["src"]` as well as `only-include = ["src"]` _at the same time_. To update your configuration, refer to the [latest hatch documentation](https://hatch.pypa.io/1.13/config/build/). 


---

_Comment by @zanieb on 2024-12-21 00:59_

Thanks for following up!

---

_Closed by @zanieb on 2024-12-21 00:59_

---

_Label `upstream` added by @zanieb on 2024-12-21 00:59_

---

_Comment by @JamesParrott on 2025-04-04 21:41_

Hi everybody,
I don't have a reproduction or a concrete explanation, but I ran into this issue in a Github action from something else, in a simple pure Python project, and thought I'd share my fix.  Basically I had a mix up between a package folder (in src) that was displaying as containing upper case characters on Windows (on which directories are case insensitive, but there is some mechanism to let you display them in whatever case you like), but on Github was mixed case.  So Hatchling's default logic could never find my package, and from Windows I had no clue why.  

The fix was to make some commits straight to my repo on Github (i.e. not via VS Code and Github Desktop): 
 - creating a new folder/package in with the mixed case name I wanted as the package name
 - copying all the files into this (Github lets you do a bulk upload)
 - Deleting the incorrectly named folder.

The empty wheel problem was self-inflicted, presumably for the same reasons as others have described above (how hatch identifies files for inclusion in a build target).  I had added to my pyproject.toml:

```
[tool.hatch.build.targets.wheel]
packages = ["src/foo"]
```

In my defence, I did this (naively) because without it, I was hitting the following error, which suggested I do so:

```
Run uv build
  uv build
  shell: /usr/bin/bash -e {0}
  env:
    UV_CACHE_DIR: /home/runner/work/_temp/setup-uv-cache
Building source distribution...
Building wheel from source distribution...
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/home/runner/work/_temp/setup-uv-cache/builds-v0/.tmp1JcKPt/lib/python3.12/site-packages/hatchling/build.py", line 58, in build_wheel
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['standard'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/work/_temp/setup-uv-cache/builds-v0/.tmp1JcKPt/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 155, in build
    artifact = version_api[version](directory, **build_data)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/work/_temp/setup-uv-cache/builds-v0/.tmp1JcKPt/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 477, in build_standard
    for included_file in self.recurse_included_files():
  File "/home/runner/work/_temp/setup-uv-cache/builds-v0/.tmp1JcKPt/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 176, in recurse_included_files
    yield from self.recurse_selected_project_files()
  File "/home/runner/work/_temp/setup-uv-cache/builds-v0/.tmp1JcKPt/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 180, in recurse_selected_project_files
    if self.config.only_include:
       ^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/functools.py", line 995, in __get__
    val = self.func(instance)
          ^^^^^^^^^^^^^^^^^^^
  File "/home/runner/work/_temp/setup-uv-cache/builds-v0/.tmp1JcKPt/lib/python3.12/site-packages/hatchling/builders/config.py", line 713, in only_include
    only_include = only_include_config.get('only-include', self.default_only_include()) or self.packages
                                                           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/work/_temp/setup-uv-cache/builds-v0/.tmp1JcKPt/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 262, in default_only_include
    return self.default_file_selection_options.only_include
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/functools.py", line 995, in __get__
    val = self.func(instance)
          ^^^^^^^^^^^^^^^^^^^
  File "/home/runner/work/_temp/setup-uv-cache/builds-v0/.tmp1JcKPt/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 250, in default_file_selection_options
    raise ValueError(message)
ValueError: Unable to determine which files to ship inside the wheel using the following heuristics: https://hatch.pypa.io/latest/plugins/builder/wheel/#default-file-selection

The most likely cause of this is that there is no directory that matches the name of your project (Cheetah_GH or cheetah_gh).

At least one file selection option must be defined in the `tool.hatch.build.targets.wheel` table, see: https://hatch.pypa.io/latest/config/build/

As an example, if you intend to ship a directory named `foo` that resides within a `src` directory located at the root of your project, you can define the following:

[tool.hatch.build.targets.wheel]
packages = ["src/foo"]
  × Failed to build `/home/runner/work/Cheetah_GH/Cheetah_GH`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.build_wheel` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build
      environment.
Error: Process completed with exit code 2.
```

---

_Comment by @zanieb on 2025-04-04 23:28_

cc @ofek for visibility

Thanks for sharing @JamesParrott !

---

_Comment by @JamesParrott on 2025-04-05 10:34_

You're welcome Zanie.  I've ruled out my situation being due to anything in uv.  This is not a bug in Hatchling either however, as it involves so many different parts, and specific choices by the user.  But if they choose to do so, the Hatchling team could help avoid this situation by tweaking the suggested fix in the error message, or even by widening the default directory searching logic for wheel targets.

I've made an [MRX repo](https://github.com/JamesParrott/fOO_base/), and tested it both with uv/hatchling and python/build/hatchling alone.  The behaviour is the same with both workflows (on Ubuntu runners).  Full explanation:  https://github.com/pypa/hatch/issues/1953

---
