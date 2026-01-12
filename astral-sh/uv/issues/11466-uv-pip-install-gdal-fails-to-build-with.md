```yaml
number: 11466
title: "`uv pip install gdal` fails to build, with workaround (Windows)"
type: issue
state: open
author: maphew
labels:
  - question
  - external
assignees: []
created_at: 2025-02-12T23:44:02Z
updated_at: 2025-07-13T23:18:59Z
url: https://github.com/astral-sh/uv/issues/11466
synced_at: 2026-01-12T16:00:37Z
```

# `uv pip install gdal` fails to build, with workaround (Windows)

---

_@maphew_

### Summary

I'm not sure this is actually a uv problem, but I didn't find it documented in one place anywhere so I'm doing that here. Close if there isn't anything uv should do about this scenario; the workaround will still be here for future searchers to build on.

Problem:
```
D:\test> uv venv --python 3.11
D:\test> uv pip install gdal

Resolved 1 package in 230ms
  x Failed to build `gdal==3.10.1`
  |-> The build backend returned an error
  `-> Call to `setuptools.build_meta.build_wheel` failed (exit code: 1)

      [stdout]
      Using numpy 2.2.2
      running bdist_wheel
      running build
      running build_py
      creating build\lib.win-amd64-cpython-311\osgeo
      copying osgeo\gdal.py -> build\lib.win-amd64-cpython-311\osgeo
[...]
      building 'osgeo._gnm' extension
      building 'osgeo._osr' extension

      [stderr]
      C:\Users\mhwilkie\AppData\Local\uv\cache\builds-v0\.tmpuXEvrM\Lib\site-packages\setuptools\config\_apply_pyprojecttoml.py:81: SetuptoolsWarning:
      `extras_require` overwritten in `pyproject.toml` (optional-dependencies)
        corresp(dist, value, root_dir)
      error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools":
      https://visualstudio.microsoft.com/visual-cpp-build-tools/

      hint: This usually indicates a problem with the package or the build environment.
```


**Workaround solution** - install a binary prebuilt wheel instead of pypi package:

1. Open **geospatial-wheels** binary release page https://github.com/cgohlke/geospatial-wheels/releases/latest and copy url for:
2. Find the binary matching your python version, `cp311` is python 3.11, `cp313` is py 3.13, etc.
3. ...and matching hardware, `win32`, `arm64`, `amd64`
4. feed the matching url to uv pip install

```
uv run python -m pip install https://github.com/cgohlke/geospatial-wheels/releases/download/v2025.1.20/GDAL-3.10.1-cp311-cp311-win_amd64.whl

Collecting GDAL==3.10.1
  Downloading https://github.com/cgohlke/geospatial-wheels/releases/download/v2025.1.20/GDAL-3.10.1-cp311-cp311-win_amd64.whl (43.9 MB)
     ---------------------------------------- 43.9/43.9 MB 17.2 MB/s eta 0:00:00
Installing collected packages: GDAL
Successfully installed GDAL-3.10.1
```

What could/should uv do for this scenario?

* don't attempt to build on Windows where there isn't a tool chain (?)
* and/or offer an info message about what to do (but maybe the setuptools message with _Get it with "Microsoft C++ Build Tools"_ serves that well enough)
* cache pip installs so that downloads aren't repeated


Sources:
* https://www.reddit.com/r/gis/comments/1hojo4t/whats_the_point_of_pip_install_gdal_eli5/
* https://github.com/astral-sh/uv/issues/1703

### Platform

MSYS_NT-10.0-19045 3.4.10-2e2ef940.x86_64 x86_64 Msy

### Version

uv 0.5.30 (ddbc6e315 2025-02-10)

### Python version

Python 3.11.11

---

_Label `bug` added by @maphew on 2025-02-12 23:44_

---

_Comment by @zanieb on 2025-02-13 01:53_

Can you share verbose logs? e.g., `uv pip install -v ...`?

---

_Comment by @zanieb on 2025-02-13 02:02_

Oh I see, they don't publish wheels to PyPi there are just prebuilt ones elsewhere?

You can add a platform specific source URL source if you want this to work with `uv sync`:

- https://docs.astral.sh/uv/concepts/projects/dependencies/#url
- https://docs.astral.sh/uv/concepts/projects/dependencies/#platform-specific-sources

I think what you're doing for `uv pip install` is sort of "the solution" unless you want to install the prerequisites necessary to build from source. I don't think our error message there could really be any clearer.

Ideally the project would publish wheels to PyPi, or that other project would publish the wheels under a different name or static package index.


---

_Label `bug` removed by @zanieb on 2025-02-13 02:03_

---

_Label `question` added by @zanieb on 2025-02-13 02:03_

---

_Label `upstream` added by @zanieb on 2025-02-13 02:03_

---

_Comment by @maphew on 2025-02-13 17:56_

Thanks for the feedback! Can the dependency by url be used in a pep723 style script header? I love the `uv run` workflow, not having to build a .toml, resulting in single file tool scripts.

---

_Comment by @JacobJeppesen on 2025-02-16 11:41_

Edit: Realized `https://girder.github.io/large_image_wheels` only hosts wheels for Linux, so it doesn't work on Windows. 

---
Generally, I think it's easier to install `gdal` from `https://girder.github.io/large_image_wheels`. I got it working with `uv add --find-links https://girder.github.io/large_image_wheels gdal`. However, it doesn't add the `--find-links https://girder.github.io/large_image_wheels` part to `pyproject.toml`, so it also has to be manually added when running `uv sync` (i.e., `uv sync --find-links https://girder.github.io/large_image_wheels`). 

Would be great if there was a way to add the `find-links` part to script headers or `pyproject.toml`. I tried following the [guide for using uv with PyTorch](https://docs.astral.sh/uv/guides/integration/pytorch/), but that required me to hardcode the specific `.whl` url. I used to use `pip-tools`, where `find-links` could be added to `requirements.in` like this:
```
--find-links=https://girder.github.io/large_image_wheels
GDAL
```

Would be great to see something similar with `uv`. Could of course also be that it's already added, and it's just me who didn't manage to get it to work 🙂 

---

_Comment by @zanieb on 2025-02-16 18:37_

> Can the dependency by url be used in a pep723 style script header? I love the uv run workflow, not having to build a .toml, resulting in single file tool scripts.

Yes we'll read `[tool.uv]` sections from those scripts too.

> Would be great if there was a way to add the find-links part to script headers or pyproject.toml

https://docs.astral.sh/uv/reference/settings/#find-links

---

_Comment by @jsmariegaard on 2025-02-17 07:29_

We would like to move all our Python users to uv instead of conda-forge. The only thing missing on Windows is GDAL which is a requirement for a lot of our users. Would be great to find a solution!   

---

_Comment by @jsmariegaard on 2025-02-17 12:21_

> We would like to move all our Python users to uv instead of conda-forge. The only thing missing on Windows is GDAL which is a requirement for a lot of our users. Would be great to find a solution!

GDAL binary wheels can be found on https://github.com/cgohlke/geospatial-wheels and you can use this low-level solution

    $ uv pip install https://github.com/cgohlke/geospatial-wheels/releases/download/v2025.1.20/GDAL-3.10.1-cp313-cp313-win_amd64.whl

But how do you get a better uv-native solution using uv add/sync etc? 

---

_Comment by @zanieb on 2025-02-17 14:51_

As mentioned above, it sounds like

```
[tool.uv]
find-links = "https://girder.github.io/large_image_wheels"
```

should work?

---

_Comment by @JacobJeppesen on 2025-02-17 15:13_

Sorry, that was my fault. They only host wheels for Linux on `https://girder.github.io/large_image_wheels`. I thought they hosted for more platforms. It does work really well on Linux when adding `find-links` to `pyproject.toml` though 👍 

---

_Comment by @zanieb on 2025-02-17 15:18_

I see. I'm sorry to say this is a problem with the package — there's not much we can do here.

See https://github.com/astral-sh/uv/issues/11466#issuecomment-2655264552 for adding the link to the wheel to your `pyproject.toml`

---

_Comment by @jsmariegaard on 2025-02-17 15:34_

I wonder if [@cgohike  ](https://github.com/cgohlke) could create a flat list of links to package files as requires by https://docs.astral.sh/uv/reference/settings/#find-links 🤔 Then I guess we would be good to go. @zanieb, the regular release assets in https://github.com/cgohlke/geospatial-wheels/releases cannot be used in find-links, right? (I could not get it to work) 

---

_Comment by @maphew on 2025-02-17 19:50_

The 2nd script below will build the flat list of links from a Github assets list, `uv run list_github_assets.py > asset-links.txt`, but I haven't figured out how to feed that to `uv run`. 

re: https://github.com/astral-sh/uv/issues/11466#issuecomment-2661562869, what is the syntax using `[uv.tool]` in the script header? This is closest I've gotten is below, but it emits _"expected struct ToolUv"_. I get zero internet search results on that phrase and don't know what to do with it.


**test-gdal.py**:
```
# /// script
# requires-python = ">=3.8"
# dependencies = [
#   "gdal",
# ]
# tool.uv = [
#    "find-links = file://asset-links.txt"
#    ]
# ///
print('hi!')
```

yields:
```
 uv run test-gdal.py
error: TOML parse error at line 5, column 11
  |
5 | tool.uv = "find-links = file://asset-links.txt"
  |           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
invalid type: string "find-links = file://asset-links.txt", expected struct ToolUv
```


**list_github_assets.py**:
```python
# /// script
# requires-python = ">=3.8"
# dependencies = [
#     "httpx",
# ]
# ///
import sys
import httpx

def fetch_assets(repo: str):
    """Fetch asset URLs from the latest GitHub release."""
    url = f"https://api.github.com/repos/{repo}/releases/latest"

    response = httpx.get(url, timeout=10)
    response.raise_for_status()
    assets = response.json().get("assets", [])

    for asset in assets:
        print(asset["browser_download_url"])

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage:  uv run list_github_assets.py <PROJECT/REPO>\n\t", file=sys.stderr)
        print("uv run list_github_assets.py cgohlke/geospatial-wheels", file=sys.stderr)
        sys.exit(1)

    fetch_assets(sys.argv[1])
```

---

_Comment by @Huyston on 2025-02-20 18:21_

> [tool.uv]
> find-links = "https://girder.github.io/large_image_wheels"

Actually, it should be a list

#pyproject.toml
[tool.uv]
find-links = ["https://girder.github.io/large_image_wheels"]

---

_Comment by @maphew on 2025-02-24 19:38_

@Huyston that's not working for me. Can you show a full example? Here's my latest attempt: https://gist.github.com/maphew/0cb4b65777732451895b490e61d7e236

---

_Comment by @trungleduc on 2025-02-26 07:48_

> We would like to move all our Python users to uv instead of conda-forge.

@jsmariegaard i'm curious, you are moving away of conda-forge because of conda slowness?

---

_Comment by @jsmariegaard on 2025-02-26 07:59_

> > We would like to move all our Python users to uv instead of conda-forge.
> 
> [@jsmariegaard](https://github.com/jsmariegaard) i'm curious, you are moving away of conda-forge because of conda slowness?

yes, and uv-niceness! uv offers a lot more in terms of package and project management and reproducibility  

---

_Comment by @trungleduc on 2025-02-26 08:05_

sorry for being off-topic, but did you take a look at [pixi](https://pixi.sh/latest/), it's basically the `uv` version of `conda`

---

_Comment by @jsmariegaard on 2025-02-26 08:11_

> sorry for being off-topic, but did you take a look at [pixi](https://pixi.sh/latest/), it's basically the `uv` version of `conda`

Yes, it is definitely and interesting project and some of my colleagues tested it out. uv just works, the experiences with pixi were a bit more mixed. Our recommendation in our company (300ish Python users) is to use uv as the primary solution, if that does not work for you and you need some conda packages, then use Miniforge3. In the future we may change that to pixi.   

---

_Comment by @nathanjmcdougall on 2025-02-28 00:42_

I've created a proof-of-concept PEP503 index that just points to the released wheels at <https://github.com/cgohlke/geospatial-wheels>, and can be used together with the Linux wheels at <https://gitlab.com/mentaljam/gdal-wheels>.

You can find it here:
<https://github.com/nathanjmcdougall/geospatial-wheels-index>.

You can use it with this `pyproject.toml` config:

```TOML
[tool.uv.sources]
gdal = [
  { index = "gdal-wheels", marker = "sys_platform == 'linux'" },
  { index = "geospatial_wheels", marker = "sys_platform == 'win32'" },
]

[[tool.uv.index]]
name = "geospatial_wheels"
url = "https://nathanjmcdougall.github.io/geospatial-wheels-index/"
explicit = true

[[tool.uv.index]]
name = "gdal-wheels"
url = "https://gitlab.com/api/v4/projects/61637378/packages/pypi/simple"
explicit = true
```

And then run `uv add gdal`.


---

_Comment by @hectorpatino on 2025-03-14 15:35_

This worked for me with python 3.10.11 

---

_Comment by @giswqs on 2025-07-13 22:46_

I tested it on Linux. It seems no longer working.

```bash
uv venv
uv init
# Update the project.toml config
uv add gdal
```

> Resolved 5 packages in 2.96s
  × Failed to build `gdal==3.11.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stdout]
      Using numpy 2.3.1
      running egg_info
      writing gdal-utils/GDAL.egg-info/PKG-INFO
      writing dependency_links to gdal-utils/GDAL.egg-info/dependency_links.txt
      writing entry points to gdal-utils/GDAL.egg-info/entry_points.txt
      writing requirements to gdal-utils/GDAL.egg-info/requires.txt
      writing top-level names to gdal-utils/GDAL.egg-info/top_level.txt
      gdal_config_error: Traceback (most recent call last):
        File "<string>", line 92, in fetch_config
        File "/usr/lib/python3.13/subprocess.py", line 1039, in __init__
          self._execute_child(args, executable, preexec_fn, close_fds,
          ~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                              pass_fds, cwd, env,
                              ^^^^^^^^^^^^^^^^^^^
          ...<5 lines>...
                              gid, gids, uid, umask,
                              ^^^^^^^^^^^^^^^^^^^^^^
                              start_new_session, process_group)
                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/usr/lib/python3.13/subprocess.py", line 1969, in _execute_child
          raise child_exception_type(errno_num, err_msg, err_filename)
      FileNotFoundError: [Errno 2] No such file or directory: 'gdal-config'

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "<string>", line 244, in get_gdal_config
        File "<string>", line 95, in fetch_config
      gdal_config_error: [Errno 2] No such file or directory: 'gdal-config'

      Could not find gdal-config. Make sure you have installed the GDAL native library and development headers.

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking




---

_Comment by @nathanjmcdougall on 2025-07-13 23:12_

@giswqs the Linux wheels are maintained in this project: https://gitlab.com/mentaljam/gdal-wheels
I think you should open an issue there. I agree they seem to be broken for v3.11.1.

I would recommend using `uv add gdal!=3.11.1` until the issue is resolved.

I also note, that project is now building both Linux and Windows wheels, so potentially you could use it exclusively if you wished to do so (I haven't tested this thoroughly)

```TOML
[tool.uv.sources]
gdal = [
  { index = "gdal-wheels" }
]

[[tool.uv.index]]
name = "gdal-wheels"
url = "https://gitlab.com/api/v4/projects/61637378/packages/pypi/simple"
explicit = true
```

---
