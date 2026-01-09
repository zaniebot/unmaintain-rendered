---
number: 9226
title: Should reinstall-package run for nested packages?
type: issue
state: open
author: jfun
labels: []
assignees: []
created_at: 2024-11-19T14:22:45Z
updated_at: 2024-11-22T09:48:39Z
url: https://github.com/astral-sh/uv/issues/9226
synced_at: 2026-01-07T13:12:18-06:00
---

# Should reinstall-package run for nested packages?

---

_Issue opened by @jfun on 2024-11-19 14:22_

- uv platform: `Windows`
- uv version: `0.5.2 (195f4b634 2024-11-14)`

This is my current situatation:

- I have a `lib` that depends on [supervisely](https://github.com/supervisely/supervisely), which depends on [python-libmagic](https://github.com/dveselov/python-libmagic)
- I use `python-magic-bin` to provide the libraries that `python-libmagic` expects to find.
  - However both `python-libmagic` and `python-magic-bin` share the module name `magic` which makes this workaround dependent on the order on which the dependencies are installed.
- Current workaround is to use `reinstall-package` to install `python-magic-bin` last.

This workaround needs to be specified for every single consumer of `lib`, it would be nice if this could be specified once in `lib` and all consumers do not have to have knowledge of this hack.

Otherwise, is there a better way to handle this issue locally?

## Reproduction

Repro repository: https://github.com/jfun/uv_libmagic_repro/tree/main

```
cd packages/app
uv run --verbose hello.py
```
### Output 
```
DEBUG uv 0.5.2 (195f4b634 2024-11-14)
DEBUG Found project root: `C:\BV\dev\zz\packages\app`
DEBUG No workspace root found, using project root
DEBUG Discovered project `app` at: C:\BV\dev\zz\packages\app
DEBUG Reading Python requests from version file at `C:\BV\dev\zz\packages\app\.python-version`
DEBUG Using Python request `3.10.11` from version file at `.python-version`
DEBUG Searching for Python 3.10.11 in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\BV\AppData\Roaming\uv\python`
DEBUG Skipping incompatible managed installation `cpython-3.13.0-windows-x86_64-none`
DEBUG Found managed installation `cpython-3.10.11-windows-x86_64-none`
DEBUG Found `cpython-3.10.11-windows-x86_64-none` at `C:\Users\BV\AppData\Roaming\uv\python\cpython-3.10.11-windows-x86_64-none\python.exe` (managed installations)
Using CPython 3.10.11
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: app @ file:///C:/BV/dev/zz/packages/app
DEBUG No workspace root found, using project root
DEBUG Found static `pyproject.toml` for: lib @ file:///C:/BV/dev/zz/packages/lib
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 91 packages in 1ms
DEBUG Using request timeout of 30s
DEBUG Directory source requirement already cached: lib==0.1.0 (from file:///C:/BV/dev/zz/packages/lib)
DEBUG Requirement already cached: setuptools==75.5.0
DEBUG Requirement already cached: python-magic-bin==0.4.14
DEBUG Requirement already cached: supervisely==6.73.233
DEBUG Requirement already cached: aiofiles==24.1.0
DEBUG Requirement already cached: anyio==4.2.0
DEBUG Requirement already cached: arel==0.3.0
DEBUG Requirement already cached: async-asgi-testclient==1.4.11
DEBUG Requirement already cached: beautifulsoup4==4.12.3
DEBUG Requirement already cached: bidict==0.23.1
DEBUG Requirement already cached: cacheout==0.14.1
DEBUG Requirement already cached: cachetools==5.5.0
DEBUG Requirement already cached: click==8.1.7
DEBUG Requirement already cached: distinctipy==1.3.4
DEBUG Requirement already cached: fastapi==0.109.0
DEBUG Requirement already cached: ffmpeg-python==0.2.0
DEBUG Requirement already cached: gitpython==3.1.43
DEBUG Requirement already cached: giturlparse==0.12.0
DEBUG Requirement already cached: httpx==0.27.2
DEBUG Requirement already cached: imutils==0.5.4
DEBUG Requirement already cached: jinja2==3.1.4
DEBUG Requirement already cached: jsonpatch==1.33
DEBUG Requirement already cached: jsonschema==4.20.0
DEBUG Requirement already cached: markupsafe==2.1.5
DEBUG Requirement already cached: numerize==0.12
DEBUG Requirement already cached: numpy==1.26.4
DEBUG Requirement already cached: opencv-python==4.10.0.84
DEBUG Requirement already cached: pandas==2.1.4
DEBUG Requirement already cached: pillow==10.2.0
DEBUG Requirement already cached: protobuf==3.20.3
DEBUG Requirement already cached: psutil==5.9.8
DEBUG Requirement already cached: ptable==0.9.2
DEBUG Requirement already cached: pydantic==2.8.2
DEBUG Requirement already cached: pydicom==2.4.4
DEBUG Requirement already cached: pyjwt==2.10.0
DEBUG Requirement already cached: pynrrd==0.4.3
DEBUG Requirement already cached: python-dotenv==1.0.0
DEBUG Requirement already cached: python-json-logger==2.0.7
DEBUG Requirement already cached: python-magic==0.4.27
DEBUG Requirement already cached: python-multipart==0.0.12
DEBUG Requirement already cached: pyyaml==6.0.2
DEBUG Requirement already cached: requests==2.32.3
DEBUG Requirement already cached: requests-toolbelt==1.0.0
DEBUG Requirement already cached: rich==13.9.4
DEBUG Requirement already cached: shapely==2.0.2
DEBUG Requirement already cached: simpleitk==2.4.0
DEBUG Requirement already cached: stringcase==1.2.0
DEBUG Requirement already cached: tqdm==4.67.0
DEBUG Requirement already cached: trimesh==4.5.0
DEBUG Requirement already cached: urllib3==2.2.2
DEBUG Requirement already cached: uvicorn==0.32.0
DEBUG Requirement already cached: varname==0.13.5
DEBUG Requirement already cached: websockets==13.1
DEBUG Requirement already cached: zstd==1.5.5.1
DEBUG Requirement already cached: exceptiongroup==1.2.2
DEBUG Requirement already cached: idna==3.10
DEBUG Requirement already cached: sniffio==1.3.1
DEBUG Requirement already cached: typing-extensions==4.12.2
DEBUG Requirement already cached: starlette==0.35.1
DEBUG Requirement already cached: watchfiles==0.24.0
DEBUG Requirement already cached: multidict==6.1.0
DEBUG Requirement already cached: soupsieve==2.6
DEBUG Requirement already cached: colorama==0.4.6
DEBUG Requirement already cached: future==1.0.0
DEBUG Requirement already cached: gitdb==4.0.11
DEBUG Requirement already cached: certifi==2024.8.30
DEBUG Requirement already cached: httpcore==1.0.7
DEBUG Requirement already cached: h2==4.1.0
DEBUG Requirement already cached: jsonpointer==3.0.0
DEBUG Requirement already cached: attrs==24.2.0
DEBUG Requirement already cached: jsonschema-specifications==2024.10.1
DEBUG Requirement already cached: referencing==0.35.1
DEBUG Requirement already cached: rpds-py==0.21.0
DEBUG Requirement already cached: python-dateutil==2.9.0.post0
DEBUG Requirement already cached: pytz==2024.2
DEBUG Requirement already cached: tzdata==2024.2
DEBUG Requirement already cached: annotated-types==0.7.0
DEBUG Requirement already cached: pydantic-core==2.20.1
DEBUG Requirement already cached: charset-normalizer==3.4.0
DEBUG Requirement already cached: markdown-it-py==3.0.0
DEBUG Requirement already cached: pygments==2.18.0
DEBUG Requirement already cached: h11==0.14.0
DEBUG Requirement already cached: httptools==0.6.4
DEBUG Requirement already cached: executing==2.1.0
DEBUG Requirement already cached: smmap==5.0.1
DEBUG Requirement already cached: hpack==4.0.0
DEBUG Requirement already cached: hyperframe==6.0.1
DEBUG Requirement already cached: six==1.16.0
DEBUG Requirement already cached: mdurl==0.1.2
DEBUG File already exists (subsequent attempt), overwriting: C:\BV\dev\zz\packages\app\.venv\Lib\site-packages\magic\__init__.py
Installed 89 packages in 1.02s
 + aiofiles==24.1.0
 + annotated-types==0.7.0
 + anyio==4.2.0
 + arel==0.3.0
 + async-asgi-testclient==1.4.11
 + attrs==24.2.0
 + beautifulsoup4==4.12.3
 + bidict==0.23.1
 + cacheout==0.14.1
 + cachetools==5.5.0
 + certifi==2024.8.30
 + charset-normalizer==3.4.0
 + click==8.1.7
 + colorama==0.4.6
 + distinctipy==1.3.4
 + exceptiongroup==1.2.2
 + executing==2.1.0
 + fastapi==0.109.0
 + ffmpeg-python==0.2.0
 + future==1.0.0
 + gitdb==4.0.11
 + gitpython==3.1.43
 + giturlparse==0.12.0
 + h11==0.14.0
 + h2==4.1.0
 + hpack==4.0.0
 + httpcore==1.0.7
 + httptools==0.6.4
 + httpx==0.27.2
 + hyperframe==6.0.1
 + idna==3.10
 + imutils==0.5.4
 + jinja2==3.1.4
 + jsonpatch==1.33
 + jsonpointer==3.0.0
 + jsonschema==4.20.0
 + jsonschema-specifications==2024.10.1
 + lib==0.1.0 (from file:///C:/BV/dev/zz/packages/lib)
 + markdown-it-py==3.0.0
 + markupsafe==2.1.5
 + mdurl==0.1.2
 + multidict==6.1.0
 + numerize==0.12
 + numpy==1.26.4
 + opencv-python==4.10.0.84
 + pandas==2.1.4
 + pillow==10.2.0
 + protobuf==3.20.3
 + psutil==5.9.8
 + ptable==0.9.2
 + pydantic==2.8.2
 + pydantic-core==2.20.1
 + pydicom==2.4.4
 + pygments==2.18.0
 + pyjwt==2.10.0
 + pynrrd==0.4.3
 + python-dateutil==2.9.0.post0
 + python-dotenv==1.0.0
 + python-json-logger==2.0.7
 + python-magic==0.4.27
 + python-magic-bin==0.4.14
 + python-multipart==0.0.12
 + pytz==2024.2
 + pyyaml==6.0.2
 + referencing==0.35.1
 + requests==2.32.3
 + requests-toolbelt==1.0.0
 + rich==13.9.4
 + rpds-py==0.21.0
 + setuptools==75.5.0
 + shapely==2.0.2
 + simpleitk==2.4.0
 + six==1.16.0
 + smmap==5.0.1
 + sniffio==1.3.1
 + soupsieve==2.6
 + starlette==0.35.1
 + stringcase==1.2.0
 + supervisely==6.73.233
 + tqdm==4.67.0
 + trimesh==4.5.0
 + typing-extensions==4.12.2
 + tzdata==2024.2
 + urllib3==2.2.2
 + uvicorn==0.32.0
 + varname==0.13.5
 + watchfiles==0.24.0
 + websockets==13.1
 + zstd==1.5.5.1
DEBUG Using Python 3.10.11 interpreter at: C:\BV\dev\zz\packages\app\.venv\Scripts\python.exe
DEBUG Running `python hello.py`
C:\BV\dev\zz\packages\app\.venv\lib\site-packages\prettytable\prettytable.py:421: SyntaxWarning: "is" with a literal. Did you mean "=="?
  elif val is None or (isinstance(val, dict) and len(val) is 0):
C:\BV\dev\zz\packages\app\.venv\lib\site-packages\prettytable\prettytable.py:441: SyntaxWarning: "is" with a literal. Did you mean "=="?
  elif val is None or (isinstance(val, dict) and len(val) is 0):
C:\BV\dev\zz\packages\app\.venv\lib\site-packages\prettytable\prettytable.py:459: SyntaxWarning: "is" with a literal. Did you mean "=="?
  if val is None or (isinstance(val, dict) and len(val) is 0):
C:\BV\dev\zz\packages\app\.venv\lib\site-packages\prettytable\prettytable.py:476: SyntaxWarning: "is" with a literal. Did you mean "=="?
  if val is None or (isinstance(val, dict) and len(val) is 0):
C:\BV\dev\zz\packages\app\.venv\lib\site-packages\prettytable\prettytable.py:674: SyntaxWarning: "is" with a literal. Did you mean "=="?
  if val is None or (isinstance(val, dict) and len(val) is 0):
C:\BV\dev\zz\packages\app\.venv\lib\site-packages\prettytable\prettytable.py:691: SyntaxWarning: "is" with a literal. Did you mean "=="?
  if val is None or (isinstance(val, dict) and len(val) is 0):
Traceback (most recent call last):
  File "C:\BV\dev\zz\packages\app\hello.py", line 1, in <module>
    import supervisely  # noqa: F401
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\__init__.py", line 221, in <module>
    from supervisely.io import docker_utils
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\io\docker_utils.py", line 8, in <module>
    from supervisely.app import DialogWindowError
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\app\__init__.py", line 6, in <module>
    import supervisely.app.widgets as widgets
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\app\widgets\__init__.py", line 69, in <module>
    from supervisely.app.widgets.model_info.model_info import ModelInfo
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\app\widgets\model_info\model_info.py", line 11, in <module>
    from supervisely.nn.inference import Session
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\nn\__init__.py", line 2, in <module>
    import supervisely.nn.inference as inference
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\nn\inference\__init__.py", line 23, in <module>
    from supervisely.nn.inference.session import Session, SessionJSON
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\nn\inference\session.py", line 17, in <module>
    from supervisely.convert.image.sly.sly_image_helper import get_meta_from_annotation
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\convert\__init__.py", line 9, in <module>
    from supervisely.convert.converter import ImportManager
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\convert\converter.py", line 10, in <module>
    from supervisely.convert.image.csv.csv_converter import CSVConverter
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\convert\image\csv\csv_converter.py", line 20, in <module>
    from supervisely.convert.image.image_converter import ImageConverter
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\supervisely\convert\image\image_converter.py", line 6, in <module>
    import magic
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\magic\__init__.py", line 209, in <module>
    libmagic = loader.load_lib()
  File "C:\BV\dev\zz\packages\app\.venv\lib\site-packages\magic\loader.py", line 49, in load_lib
    raise ImportError('failed to find libmagic.  Check your installation')
ImportError: failed to find libmagic.  Check your installation
DEBUG Command exited with code: 1
```



<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @ReinforcedKnowledge on 2024-11-22 02:19_

I'd like to help you but there is no distribution for `python-magic-bin` on macos.
```
error: Distribution `python-magic-bin==0.4.14 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

But I was wondering, who are the consumers of your `lib`? I think the only way they can install your package is by building it from source using `uv` (to understand the reinstall part, or use another tool but apply the reinstall part manually). But I can't seem to even install the dependencies so I think that will be hard.

Unfortunately the reinstall don't go into the wheel, so you can't distribute that. And it'd amount to the same anyways since I don't think you'll be able to build a wheel for platforms where `python-magic-bin` isn't present.

Locally, my understanding of the issue you're encountering is:

`uv run --verbose hello.py` will install `lib` in your virtual environment. Since it's an editable dependency, what will happen is, `uv` will run the `hatchling` backend in `lib` to build an "editable" wheel (see [PEP 660](https://peps.python.org/pep-0660/)), this wheel is exactly like a regular wheel, just with some extra stuff that will make the install of the wheel in your virtual environment effectively editable. Now as we said, the wheel doesn't contain the information from the reinstall. So when it comes to installing `lib`, even as editable, its dependencies will be installed as recorded in `METADATA` in `.dist-info` of the wheel, which will be `"python-magic-bin>=0.4.14"` then `"supervisely>=6.73.233"`. And that's all, no reinstall. Then there will be a reference to `lib` because it's an editable install (instead of having the code itself in your venv). 

My guess is confirmed by the correct installation of your `app` dependencies, `uv` tries to run `hello.py` at "DEBUG Running `python hello.py`" but then you have that issue with the `magic` module right.

I think you should add a reinstall in your `app`'s `pyproject.toml`.

---

_Comment by @jfun on 2024-11-22 09:48_

Thanks for looking into it!

> But I was wondering, who are the consumers of your lib?

Basically I have a monorepo with multiple non-workspace packages (i.e `uv init --no-workspace ...etc`) that consume `lib`. There's no intention of distributing any of this, all code is staying inhouse.

> Locally, my understanding of the issue you're encountering is:
> uv run --verbose hello.py will install lib in your virtual environment. Since it's an editable dependency, what will happen is, uv will run the hatchling backend in lib to build an "editable" wheel (see [PEP 660](https://peps.python.org/pep-0660/)), this wheel is exactly like a regular wheel, just with some extra stuff that will make the install of the wheel in your virtual environment effectively editable. Now as we said, the wheel doesn't contain the information from the reinstall. So when it comes to installing lib, even as editable, its dependencies will be installed as recorded in METADATA in .dist-info of the wheel, which will be "python-magic-bin>=0.4.14" then "supervisely>=6.73.233". And that's all, no reinstall. Then there will be a reference to lib because it's an editable install (instead of having the code itself in your venv).

That makes sense, thank you for the write up.

> I think you should add a reinstall in your app's pyproject.toml.

I have a script that adds the `reinstall-package` to all non-workspace packages.

I was just wondering if there was a better solution to this situation, the script just feels like a giant hack.

---
