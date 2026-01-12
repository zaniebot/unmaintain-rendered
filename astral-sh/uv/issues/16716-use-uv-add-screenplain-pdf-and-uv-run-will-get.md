```yaml
number: 16716
title: "use uv add screenplain[pdf] and uv run will get ModuleNotFoundError: No module named 'screenplain'"
type: issue
state: closed
author: zhiquanchi
labels:
  - needs-mre
assignees: []
created_at: 2025-11-13T03:56:12Z
updated_at: 2025-11-14T08:45:40Z
url: https://github.com/astral-sh/uv/issues/16716
synced_at: 2026-01-12T16:02:37Z
```

# use uv add screenplain[pdf] and uv run will get ModuleNotFoundError: No module named 'screenplain'

---

_@zhiquanchi_

### Summary

I'm use uv add screenplain[pdf]  ,then i in code import this model .but  i use uv run xx.py ,i get ModuleNotFoundError: No module named 'screenplain'

### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.9.7

### Python version

Python 3.12.3

---

_Label `bug` added by @zhiquanchi on 2025-11-13 03:56_

---

_Comment by @zhiquanchi on 2025-11-13 03:56_

Repository address https://github.com/vilcans/screenplain

---

_Comment by @nooscraft on 2025-11-13 05:43_

Hey @zhiquanchi — I gave this a spin locally to see what was happening. A couple things stood out:

* `uv add screenplain[pdf]` works as expected for me. After running it, `screenplain` ends up in `.venv/lib/python3.12/site-packages/screenplain/`, so the dependency is resolving.
* I believe you have an idea about how screenplain works, so, If you want to sanity-check that the library is actually installed, you can poke at something it exposes, for example:

  ```bash
  uv run -- python - <<'PY'
  import screenplain.main
  print(screenplain.main.output_formats)
  print(callable(screenplain.main.main))
  PY
  ```

  That prints out the supported formats and verifies the entry point is callable, which shows the module is there.

If you’re still seeing `ModuleNotFoundError`, it usually means `uv run` is being executed from outside the project directory (the one that has `pyproject.toml` and `uv.lock`). `uv run` looks for the project files in the current directory or its parents; if you’re in some other folder, it won’t pick up the dependencies and you’ll get the import error.

Could you try the commands above and confirm the module shows up? If the import still fails, let us know:

1. The output of `uv pip list | grep screenplain`.
2. The output of `pwd` right before you run `uv run`.
3. The `[project]` section of your `pyproject.toml`.

That should help us see what’s different in your setup.

---

_Comment by @nooscraft on 2025-11-13 05:46_

One other thing to double-check: if your shell expands brackets, make sure you add the dependency with `uv add "screenplain[pdf]"` so the PDF extra is actually requested.
@zhiquanchi 

---

_Comment by @zhiquanchi on 2025-11-13 06:29_

```
uv pip list | grep screenplain
screenplain               0.11.1
```
```
[project]
name = "jujijubenshengcheng"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "fastapi>=0.117.1",
    "google-auth>=2.23.0",
    "google-cloud-storage>=2.10.0",
    "google-genai>=0.3.0",
    "loguru>=0.7.3",
    "moviepy>=2.2.1",
    "opencv-python>=4.12.0.88",
    "prefect>=3.5.0",
    "pypdf2>=3.0.1",
    "python-docx>=1.2.0",
    "python-dotenv>=1.1.1",
    "pyyaml>=6.0",
    "questionary>=2.1.1",
    "reportlab>=4.4.4",
    "requests>=2.32.5",
    "rich>=14.2.0",
    "scenedetect>=0.6.7.1",
    "screenplain[pdf]>=0.11.1",
    "tenacity>=9.1.2",
    "tqdm>=4.65.0",
    "uvicorn>=0.37.0",
    "weasyprint>=66.0",
]
```
---
My call is similar to 
```
uv run run_0_1_to_0_8.py
Traceback (most recent call last):
  File "/home/chasezhi/jubenshengcheng/run_0_1_to_0_8.py", line 55, in <module>
    from new_pipeline.steps.step0_9_2_export_pdf_lib import Step0_9_2ExportPDFLib
  File "/home/chasezhi/jubenshengcheng/new_pipeline/steps/step0_9_2_export_pdf_lib.py", line 9, in <module>
    from screenplain.export.pdf import to_pdf
ModuleNotFoundError: No module named 'screenplain'
```


---

_Comment by @nooscraft on 2025-11-13 06:50_

Thanks for the traceback, that helps. A couple of things to check on your side:

1. Make sure the dependency actually landed in this project. From the root of `/home/chasezhi/jubenshengcheng`, run:

   ```bash
   uv pip list | grep screenplain
   ```

   If nothing prints, please run `uv add "screenplain[pdf]"` (quoting makes sure the shell doesn’t eat the brackets) and then `uv sync`.

2. Confirm you’re invoking `uv run` from the same folder that contains `pyproject.toml`. `uv run` looks upward from the current directory to find the project’s `uv.lock`; if you call it from somewhere else, it won’t see the dependency you just added.

3. Once you’ve synced the dependency, a quick sanity check is:

   ```bash
   uv run -- python - <<'PY'
   import screenplain.main
   print(screenplain.main.output_formats)
   PY
   ```

   That should print `('fdx', 'html', 'pdf')`. If it still fails, could you share the `[project]` section of your `pyproject.toml` and the output of `pwd` right before you run `uv run run_0_1_to_0_8.py`? That’ll show whether the dependency is recorded and where the script is being run from.

---

_Comment by @zhiquanchi on 2025-11-13 07:06_

> Thanks for the traceback, that helps. A couple of things to check on your side:感谢提供错误跟踪信息，这很有帮助。有几件事需要您检查：
> 
> 1. Make sure the dependency actually landed in this project. From the root of `/home/chasezhi/jubenshengcheng`, run:确保依赖项确实已添加到该项目中。从 `/home/chasezhi/jubenshengcheng` 的根目录运行：
>    uv pip list | grep screenplain
>        
>          
>        
>    
>          
>        
>    
>        
>      
>    If nothing prints, please run `uv add "screenplain[pdf]"` (quoting makes sure the shell doesn’t eat the brackets) and then `uv sync`.如果没有打印任何内容，请运行 `uv add "screenplain[pdf]"` （使用引号确保 Shell 不会吃掉括号）然后运行 `uv sync` 。
> 2. Confirm you’re invoking `uv run` from the same folder that contains `pyproject.toml`. `uv run` looks upward from the current directory to find the project’s `uv.lock`; if you call it from somewhere else, it won’t see the dependency you just added.确认您是从包含 `pyproject.toml` 的同一文件夹中调用 `uv run` 的。 `uv run` 从当前目录向上查找项目的 `uv.lock` ；如果您从其他地方调用它，它将看不到您刚刚添加的依赖项。
> 3. Once you’ve synced the dependency, a quick sanity check is:一旦同步了依赖，一个快速的检查方法是：
>    uv run -- python - <<'PY'
>    import screenplain.main
>    print(screenplain.main.output_formats)
>    PY
>        
>          
>        
>    
>          
>        
>    
>        
>      
>    That should print `('fdx', 'html', 'pdf')`. If it still fails, could you share the `[project]` section of your `pyproject.toml` and the output of `pwd` right before you run `uv run run_0_1_to_0_8.py`? That’ll show whether the dependency is recorded and where the script is being run from.这应该会打印出 `('fdx', 'html', 'pdf')` 。如果仍然失败，您能否分享您的 `pyproject.toml` 中的 `[project]` 部分，以及您运行 `uv run run_0_1_to_0_8.py` 之前的 `pwd` 的输出？这将显示依赖项是否已记录以及脚本是从哪里运行的。

```
uv run -- python - <<'PY'
('fdx', 'html', 'pdf')
```
This is the fix provided by Claude, and it does work. However, I'd like to understand the underlying cause of the error.
📊 问题分析总结
🔍 根本原因
你的脚本 [run_0_1_to_0_8.py](vscode-file://vscode-app/c:/Users/chase.zhi/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 使用了 PEP 723 内联脚本元数据（文件头部的 # /// script 块）。当使用 [uv run](vscode-file://vscode-app/c:/Users/chase.zhi/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 运行这类脚本时：

uv 的行为：创建一个临时隔离环境，只安装脚本头部声明的依赖
python3 的行为：直接使用当前环境（系统或虚拟环境）中的所有已安装包
🎯 为什么会出现 ModuleNotFoundError
脚本头部缺少 [screenplain[pdf]](vscode-file://vscode-app/c:/Users/chase.zhi/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 和其他必要依赖的声明
[uv run](vscode-file://vscode-app/c:/Users/chase.zhi/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 创建的临时环境中没有安装这些缺失的包
而 python3 使用的环境已经通过 [pyproject.toml](vscode-file://vscode-app/c:/Users/chase.zhi/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 安装了完整依赖
✅ 解决方案
已在脚本头部添加所有必要的依赖声明，包括：

[screenplain[pdf]>=0.11.1](vscode-file://vscode-app/c:/Users/chase.zhi/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
moviepy>=2.2.1
opencv-python>=4.12.0.88
scenedetect>=0.6.7.1
pypdf2>=3.0.1
reportlab>=4.4.4
rich>=14.2.0
weasyprint>=66.0
现在脚本可以正常使用 [uv run](vscode-file://vscode-app/c:/Users/chase.zhi/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 运行了！

---

_Comment by @nooscraft on 2025-11-13 08:19_

@zanieb :confused:

---

_Comment by @konstin on 2025-11-13 10:28_

Can you clear the cache with `uv cache clean` and clear the venv with `uv venv -c`, then share verbose logs from `uv run -v`?

---

_Comment by @zhiquanchi on 2025-11-14 03:23_

> Can you clear the cache with `uv cache clean` and clear the venv with `uv venv -c`, then share verbose logs from `uv run -v`?您可以使用 `uv cache clean` 清除缓存，使用 `uv venv -c` 清除 venv，然后从 `uv run -v` 分享详细的日志？

sure .this uv run -v output
```
➜  jubenshengcheng git:(feat/perfect_workflow) uv run -v
DEBUG uv 0.8.4
DEBUG Found project root: `/home/chasezhi/jubenshengcheng`
DEBUG No workspace root found, using project root
DEBUG Discovered project `jujijubenshengcheng` at: /home/chasezhi/jubenshengcheng
DEBUG Acquired lock for `/home/chasezhi/jubenshengcheng`
DEBUG Reading Python requests from version file at `/home/chasezhi/jubenshengcheng/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.12`
DEBUG Released lock at `/tmp/uv-b5d5d703491348bb.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: jujijubenshengcheng @ file:///home/chasezhi/jubenshengcheng
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 142 packages in 16ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: fastapi==0.117.1
DEBUG Identified uncached distribution: google-auth==2.40.3
DEBUG Identified uncached distribution: google-cloud-storage==3.4.0
DEBUG Identified uncached distribution: google-genai==1.38.0
DEBUG Identified uncached distribution: loguru==0.7.3
DEBUG Identified uncached distribution: moviepy==2.2.1
DEBUG Identified uncached distribution: opencv-python==4.12.0.88
DEBUG Identified uncached distribution: prefect==3.5.0
DEBUG Identified uncached distribution: pypdf2==3.0.1
DEBUG Identified uncached distribution: python-docx==1.2.0
DEBUG Identified uncached distribution: python-dotenv==1.1.1
DEBUG Identified uncached distribution: pyyaml==6.0.2
DEBUG Identified uncached distribution: questionary==2.1.1
DEBUG Identified uncached distribution: reportlab==4.4.4
DEBUG Identified uncached distribution: requests==2.32.5
DEBUG Identified uncached distribution: rich==14.2.0
DEBUG Identified uncached distribution: scenedetect==0.6.7.1
DEBUG Identified uncached distribution: screenplain==0.11.1
DEBUG Registry requirement already cached: tenacity==9.1.2
DEBUG Identified uncached distribution: tqdm==4.67.1
DEBUG Identified uncached distribution: uvicorn==0.37.0
DEBUG Identified uncached distribution: weasyprint==66.0
DEBUG Identified uncached distribution: pydantic==2.11.9
DEBUG Identified uncached distribution: starlette==0.48.0
DEBUG Identified uncached distribution: typing-extensions==4.15.0
DEBUG Identified uncached distribution: cachetools==5.5.2
DEBUG Identified uncached distribution: pyasn1-modules==0.4.2
DEBUG Identified uncached distribution: rsa==4.9.1
DEBUG Identified uncached distribution: google-api-core==2.25.1
DEBUG Identified uncached distribution: google-cloud-core==2.4.3
DEBUG Identified uncached distribution: google-crc32c==1.7.1
DEBUG Identified uncached distribution: google-resumable-media==2.7.2
DEBUG Identified uncached distribution: anyio==4.10.0
DEBUG Identified uncached distribution: httpx==0.28.1
DEBUG Registry requirement already cached: websockets==15.0.1
DEBUG Identified uncached distribution: decorator==5.2.1
DEBUG Identified uncached distribution: imageio==2.37.0
DEBUG Identified uncached distribution: imageio-ffmpeg==0.6.0
DEBUG Identified uncached distribution: numpy==2.2.6
DEBUG Registry requirement already cached: pillow==11.3.0
DEBUG Identified uncached distribution: proglog==0.1.12
DEBUG Identified uncached distribution: aiosqlite==0.21.0
DEBUG Identified uncached distribution: alembic==1.17.1
DEBUG Identified uncached distribution: apprise==1.9.5
DEBUG Identified uncached distribution: asgi-lifespan==2.1.0
DEBUG Identified uncached distribution: asyncpg==0.30.0
DEBUG Identified uncached distribution: click==8.2.1
DEBUG Identified uncached distribution: cloudpickle==3.1.2
DEBUG Identified uncached distribution: coolname==2.2.0
DEBUG Identified uncached distribution: cryptography==46.0.3
DEBUG Identified uncached distribution: dateparser==1.2.2
DEBUG Identified uncached distribution: docker==7.1.0
DEBUG Identified uncached distribution: exceptiongroup==1.3.0
DEBUG Identified uncached distribution: fsspec==2025.10.0
DEBUG Identified uncached distribution: graphviz==0.21
DEBUG Identified uncached distribution: griffe==1.14.0
DEBUG Identified uncached distribution: httpcore==1.0.9
DEBUG Identified uncached distribution: humanize==4.14.0
DEBUG Identified uncached distribution: jinja2==3.1.6
DEBUG Identified uncached distribution: jinja2-humanize-extension==0.4.0
DEBUG Identified uncached distribution: jsonpatch==1.33
DEBUG Identified uncached distribution: jsonschema==4.25.1
DEBUG Identified uncached distribution: opentelemetry-api==1.38.0
DEBUG Identified uncached distribution: orjson==3.11.4
DEBUG Identified uncached distribution: packaging==25.0
DEBUG Identified uncached distribution: pathspec==0.12.1
DEBUG Identified uncached distribution: pendulum==3.1.0
DEBUG Identified uncached distribution: pluggy==1.6.0
DEBUG Identified uncached distribution: prometheus-client==0.23.1
DEBUG Identified uncached distribution: pydantic-core==2.33.2
DEBUG Identified uncached distribution: pydantic-extra-types==2.10.6
DEBUG Identified uncached distribution: pydantic-settings==2.11.0
DEBUG Identified uncached distribution: python-dateutil==2.9.0.post0
DEBUG Identified uncached distribution: python-slugify==8.0.4
DEBUG Identified uncached distribution: pytz==2025.2
DEBUG Identified uncached distribution: readchar==4.2.1
DEBUG Identified uncached distribution: rfc3339-validator==0.1.4
DEBUG Identified uncached distribution: ruamel-yaml==0.18.16
DEBUG Identified uncached distribution: semver==3.0.4
DEBUG Identified uncached distribution: sniffio==1.3.1
DEBUG Identified uncached distribution: sqlalchemy==2.0.44
DEBUG Identified uncached distribution: toml==0.10.2
DEBUG Identified uncached distribution: typer==0.19.2
DEBUG Identified uncached distribution: uv==0.9.7
DEBUG Identified uncached distribution: lxml==6.0.2
DEBUG Identified uncached distribution: prompt-toolkit==3.0.52
DEBUG Registry requirement already cached: charset-normalizer==3.4.3
DEBUG Identified uncached distribution: certifi==2025.8.3
DEBUG Identified uncached distribution: idna==3.10
DEBUG Identified uncached distribution: urllib3==2.5.0
DEBUG Identified uncached distribution: markdown-it-py==4.0.0
DEBUG Identified uncached distribution: pygments==2.19.2
DEBUG Identified uncached distribution: platformdirs==4.5.0
DEBUG Identified uncached distribution: h11==0.16.0
DEBUG Identified uncached distribution: cffi==2.0.0
DEBUG Identified uncached distribution: cssselect2==0.8.0
DEBUG Identified uncached distribution: fonttools==4.60.1
DEBUG Identified uncached distribution: pydyf==0.11.0
DEBUG Identified uncached distribution: pyphen==0.17.2
DEBUG Identified uncached distribution: tinycss2==1.4.0
DEBUG Identified uncached distribution: tinyhtml5==2.0.0
DEBUG Identified uncached distribution: annotated-types==0.7.0
DEBUG Identified uncached distribution: typing-inspection==0.4.1
DEBUG Identified uncached distribution: pyasn1==0.6.1
DEBUG Identified uncached distribution: googleapis-common-protos==1.70.0
DEBUG Identified uncached distribution: proto-plus==1.26.1
DEBUG Identified uncached distribution: protobuf==6.32.1
DEBUG Identified uncached distribution: mako==1.3.10
DEBUG Identified uncached distribution: markdown==3.10
DEBUG Identified uncached distribution: requests-oauthlib==2.0.0
DEBUG Identified uncached distribution: regex==2025.11.3
DEBUG Identified uncached distribution: tzlocal==5.3.1
DEBUG Identified uncached distribution: colorama==0.4.6
DEBUG Identified uncached distribution: h2==4.3.0
DEBUG Identified uncached distribution: markupsafe==3.0.3
DEBUG Identified uncached distribution: jsonpointer==3.0.0
DEBUG Identified uncached distribution: attrs==25.4.0
DEBUG Identified uncached distribution: jsonschema-specifications==2025.9.1
DEBUG Identified uncached distribution: referencing==0.37.0
DEBUG Identified uncached distribution: rpds-py==0.28.0
DEBUG Identified uncached distribution: importlib-metadata==8.7.0
DEBUG Identified uncached distribution: tzdata==2025.2
DEBUG Identified uncached distribution: six==1.17.0
DEBUG Identified uncached distribution: text-unidecode==1.3
DEBUG Identified uncached distribution: ruamel-yaml-clib==0.2.14
DEBUG Identified uncached distribution: greenlet==3.2.4
DEBUG Identified uncached distribution: shellingham==1.5.4
DEBUG Identified uncached distribution: wcwidth==0.2.14
DEBUG Identified uncached distribution: mdurl==0.1.2
DEBUG Identified uncached distribution: pycparser==2.23
DEBUG Identified uncached distribution: webencodings==0.5.1
DEBUG Identified uncached distribution: brotli==1.2.0
DEBUG Identified uncached distribution: zopfli==0.4.0
DEBUG Identified uncached distribution: oauthlib==3.3.1
DEBUG Identified uncached distribution: hpack==4.1.0
DEBUG Identified uncached distribution: hyperframe==6.1.0
DEBUG Identified uncached distribution: zipp==3.23.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/df/99/65080c863eb06d4498de3d6c86f3e90595e02e159fd8529f1565f56cfe2c/ruamel.yaml.clib-0.2.14-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a5/32/7df1d81ec2e50fb661944a35183d87e62d3f6c6d9f8aff64a4f245226d55/alembic-1.17.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e5/48/1549795ba7742c948d2ad169c1c8cdbae65bc450d6cd753d124b17c8cd32/certifi-2025.8.3-py3-none-any.whl
DEBUG Acquired lock for `/home/chasezhi/.cache/uv/sdists-v9/pypi/screenplain/0.11.1`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/68/1f/795e7f4aa2eacc59afa4fb61a2e35e510d06414dd5a802b51a012d691b37/opencv_python-4.12.0.88-cp37-abi3-manylinux2014_x86_64.manylinux_2_17_x86_64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/2d/43c8522a2038e9d0e7dbdf3a61195ecc31ca576fb1527a528c877e87d973/imageio_ffmpeg-0.6.0-py3-none-manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c7/59/61d8e9f1734069049abe9e593961de602397c7194712346906c075fec65f/uv-0.9.7-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f4/e6/96494debc7f5fb9730a75abfa600efaeac7fa40683ba52b478f0df294ee8/prefect-3.5.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c0/ca/4bb48a26ed95a1e7eba175535fe5805887682140ee0a0d10a88e1de84208/fonttools-4.60.1-cp312-cp312-manylinux1_x86_64.manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_5_x86_64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8c/3d/1e1db36cfd41f895d266b103df00ca5b3cbe965184df824dec5c08c6b803/numpy-2.2.6-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f4/40/0ae9d061d278b10713ea9021ef6b703ec44698fe32178715a501ac696c6b/asyncpg-0.30.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/45/e5/5aa65852dadc24b7d8ae75b7efb8d19303ed6ac93482e60c44a585930ea5/sqlalchemy-2.0.44-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c6/d1/232b3309a02d60f11e71857778bfcd4acbdb86c07db8260caf7d008b08f8/lxml-6.0.2-cp312-cp312-manylinux_2_26_x86_64.manylinux_2_28_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8f/29/798fc4ec461a1c9e9f735f2fc58741b0daae30688f41b2497dcbc9ed1355/cryptography-46.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f9/41/4b043778cf9c4285d59742281a769eac371b9e47e35f98ad321349cc5d61/pydantic_core-2.33.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/57/66/e040586fe6f9ae7f3a6986186653791fb865947f0b745290ee4ab026b834/reportlab-4.4.4-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/0d/f1/318762320d966e528dfb9e6be5953fe7df2952156f15ba857cbccafb630c/apprise-1.9.5-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7b/1f/c2142d2edf833a90728e5cdeb10bdbdc094dde8dbac078cee0cf33f5e11b/pyphen-0.17.2-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/03/a7/03aa61fbc3c5cbf99b44d158665f9b0dd3d8059be16c460208d9e385c837/brotli-1.2.0-cp312-cp312-manylinux2014_x86_64.manylinux_2_17_x86_64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/9e/8f81e69bd771014a488c4c64476b6e6faab91b2c913d0f81eca7e06401eb/zopfli-0.4.0-cp310-abi3-manylinux2014_x86_64.manylinux_2_17_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/84/bd/9ce9f629fcb714ffc2c3faf62b6766ecb7a585e1e885eb699bcf130a5209/regex-2025.11.3-cp312-cp312-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b9/2b/614b4752f2e127db5cc206abc23a8c19678e92b23c3db30fc86ab731d3bd/PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/19/0d/6660d55f7373b2ff8152401a83e02084956da23ae58cddbfb0b330978fe9/greenlet-3.2.4-cp312-cp312-manylinux_2_24_x86_64.manylinux_2_28_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/81/c4/34e93fe5f5429d7570ec1fa436f1986fb1f00c3e0f43a589fe2bbcd22c3f/pytz-2025.2-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3e/d3/108f2006987c58e76691d5ae5d200dd3e0f532cb4e5fa3560751c3a1feba/pydantic-2.11.9-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/84/03/0d3ce49e2505ae70cf43bc5bb3033955d2fc9f932163e84dc0779cc47f48/prompt_toolkit-3.0.52-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3e/cd/49ce51767b879cde77e7ad9fae164ea15dce3616fe591d9ea1df51152706/rpds_py-0.28.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/87/5d/f7a1d693e5c0f789185117d5c1d5bee104f5b0d9fbf061d715fb61c840a8/pendulum-3.1.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/5c/23/c7abc0ca0a1526a0774eca151daeb8de62ec457e77262b66b359c3c7679e/tzdata-2025.2-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/5c/f6/88d77011b605ef979aace37b7703e4eefad066f7e84d935e5a696515c2dd/protobuf-6.32.1-cp39-abi3-manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/cb/bd/b394387b598ed84d8d0fa90611a90bee0adc2021820ad5729f7ced74a8e2/imageio-2.37.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/87/22/f020c047ae1346613db9322638186468238bcfa8849b4668a22b97faad65/dateparser-1.2.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/86/f1/62a193f0227cf15a920390abe675f386dec35f7ae3ffe6da582d3ade42c7/googleapis_common_protos-1.70.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0f/d1/c5d9b341bf3d556c1e4c6566b3efdda0b1bb175510aa7b09dd3eee246923/weasyprint-66.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/16/12/164a90e4692423ed5532274928b0e19c8cae345ae1aa413d78c6b688231b/google_cloud_storage-3.4.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/00/1e03a4989fa5795da308cd774f05b704ace555a70f9bf9d3be057b680bcf/python_docx-1.2.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/53/6c/1de711bab3c118284904c3bedf870519e8c63a7a8e0905ac3833f1db9cbc/google_genai-1.38.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/25/7a/b0178788f8dc6cafce37a212c99565fa1fe7872c70c6c9c1e1a372d9d88f/rich-14.2.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8e/5e/c86a5643653825d3c913719e788e41386bee415c2b87b4f955432f2de6b2/pypdf2-3.0.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ec/57/56b9bcc3c9c6a792fcbaf139543cee77261f3651ca9da0c93f5c1221264b/python_dateutil-2.9.0.post0-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/78/2d/7fa73dfa841b5ac06c7b8855cfc18622132e365f5b81d02230333ff26e9e/cffi-2.0.0-cp312-cp312-manylinux2014_x86_64.manylinux_2_17_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/17/63/b19553b658a1692443c62bd07e5868adaa0ad746a0751ba62c59568cd45b/google_auth-2.40.3-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/eb/02/a6b21098b1d5d6249b7c5ab69dde30108a71e4e819d4a9778f1de1d5b70d/fsspec-2025.10.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/47/8d/d529b5d697919ba8c11ad626e835d4039be708a35b0d22de83a269a6682c/pyasn1_modules-0.4.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/14/4b/ead00905132820b623732b175d66354e9d3e69fcf2a5dcdab780664e7896/google_api_core-2.25.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/be/9c/92789c596b8df838baa98fa71844d84283302f7604ed565dafe5a6b5041a/oauthlib-3.3.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e3/26/57c6fb270950d476074c087527a558ccb6f4436657314bfb6cdf484114c4/docker-7.1.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2a/b1/9ff6578d789a89812ff21e4e0f80ffae20a65d5dd84e7a17873fe3b365be/griffe-1.14.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/01/7e/62517dddcfce6d53a39543cd74d0dccfcbdf53967017c58af68822100272/orjson-3.11.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/62/a1/3d680cbfd5f4b8f15abc1d571870c5fc3e594bb582bc3b64ea099db13e56/jinja2-3.1.6-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/87/fb/99f81ac72ae23375f22b7afdb7642aba97c00a713c217124420147681a2f/mako-1.3.10-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c3/5b/9512c5fb6c8218332b530f13500c6ff5f3ce3342f35e0dd7be9ac3856fd3/humanize-4.14.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b1/5b/2294ea44b3b2264a50b82a55a3822837a04cb779c873fc168844f36f5b46/scenedetect-0.6.7.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a7/c2/fe1e52489ae3122415c51f387e221dd0773709bad6c6cdaa599e8a2c5185/urllib3-2.5.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6f/12/e5e0282d673bb9746bacfb6e2dba8719989d3660cdb2ea79aee9a9651afb/anyio-4.10.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6d/45/d9d3e8eeefbe93be1c50060a9d9a9f366dba66f288bb518a9566a23a8631/fastapi-0.117.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9a/73/7d3b2010baa0b5eb1e4dfa9e4385e89b6716be76f2fa21a6c0fe34b68e5a/moviepy-2.2.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/bf/9c/8c95d856233c1f82500c2450b8c68576b4cf1c871db3afac5c34ff84e6fd/jsonschema-4.25.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/0f/73/bb1bc2529f852e7bf64a2dec885e89ff9f5cc7bbf6c9340eed30ff2c69c5/ruamel.yaml-0.18.16-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/e3/59cd50310fc9b59512193629e1984c1f95e5c8ae6e5d8c69532ccc65a7fe/pycparser-2.23-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c8/f1/d6a797abb14f6283c0ddff96bbdd46937f64122b8c925cab503dd37f8214/pyasn1-0.6.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/85/32/10bb5764d90a8eee674e9dc6f4db6a0ab47c8c4d0d83c27f7c39ac415a4d/click-8.2.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/82/35/b8d3baf8c46695858cb9d8835a53baa1eeb9906ddaf2f728a5f5b640fd1e/google_resumable_media-2.7.2-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/94/54/e7d793b573f298e1c9013b8c4dade17d481164aa517d1d7148619c2cedbf/markdown_it_py-4.0.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/30/dc54f88dd4a2b5dc8a0279bdd7270e735851848b762aeb1c1184ed1f6b14/tqdm-4.67.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a6/a5/c0b6468d3824fe3fde30dbb5e1f687b291608f9473681bbf7dabbf5a87d7/text_unidecode-1.3-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/be/72/2db2f49247d0a18b4f1bb9a5a39a0162869acf235f3a96418363947b3d46/starlette-0.48.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3a/2a/7cc015f5b9f5db42b7d48157e23356022889fc354a2813c15934b7cb5c0e/attrs-25.4.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ae/a2/d86e01c28300bd41bab8f18afd613676e2bd63515417b77636fc1add426f/opentelemetry_api-1.38.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/85/cd/584a2ceb5532af99dd09e50919e3615ba99aa127e9850eafe5f31ddfdb9a/uvicorn-0.37.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/69/b2/119f6e6dcbd96f9069ce9a2665e0146588dc9f88f29549711853645e736a/h2-4.3.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/29/0348de65b8cc732daa3e33e67806420b2ae89bdce2b04af740289c5c6c8c/loguru-0.7.3-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b8/db/14bafcb4af2139e046d03fd00dea7873e48eafe18b7d2797e73d6681f210/prometheus_client-0.23.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4e/6d/280c4c2ce28b1593a19ad5239c8b826871fc6ec275c21afc8e1820108039/proto_plus-1.26.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/83/d6/887a1ff844e64aa823fb4905978d882a633cfe295c32eacad582b78a7d8b/pydantic_settings-2.11.0-py3-none-any.whl
Downloading cryptography (4.3MiB)
Downloading sqlalchemy (3.2MiB)
Downloading asyncpg (3.4MiB)
Downloading uv (20.4MiB)
Downloading apprise (1.4MiB)
Downloading prefect (5.9MiB)
Downloading pydantic-core (1.9MiB)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/91/4c/e0ce1ef95d4000ebc1c11801f9b944fa5910ecc15b5e351865763d8657f8/graphviz-0.21-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/00/22/35617eee79080a5d071d0f14ad698d325ee6b3bf824fc0467c03b30e7fa8/typer-0.19.2-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/18/67/36e9267722cc04a6b9f15c7f3441c2363321a3ea07da7ae0c0707beb2a9c/typing_extensions-4.15.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/93/04/5c918669096da8d1c9ec7bb716bd72e755526103a61bc5e76a3e4fb23b53/pydantic_extra_types-2.10.6-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5c/de/27c57899297163a4a84104d5cec0af3b1ac5faf62f44667e506373c6b8ce/tinyhtml5-2.0.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1b/b1/5745d7523d8ce53b87779f46ef6cf5c5c342997939c2fe967e607b944e43/coolname-2.2.0-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/af/b5/123f13c975e9f27ab9c0770f514345bd406d0e8d3b7a0723af9d43f710af/wcwidth-0.2.14-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3c/26/1062c7ec1b053db9e499b4d2d5bc231743201b74051c973dadeac80a8f43/questionary-2.1.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/64/8d/0133e4eb4beed9e425d9a98ed6e081a55d195481b7632472be1af08d2f6b/rsa-4.9.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/07/c6/80c95b1b2b94682a72cbdbfb85b81ae2daffa4291fbfa1b1464502ede10d/hpack-4.1.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3b/a5/7279055cf004561894ed3a7bfdf5bf90a53f28fadd01af7cd166e88ddf16/google_crc32c-1.7.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/40/86/bda7241a8da2d28a754aad2ba0f6776e35b67e37c36ae0c45d49370f1014/google_cloud_core-2.4.3-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/20/b0/36bd937216ec521246249be3bf9855081de4c5e06a0c9b4219dbeda50373/importlib_metadata-8.7.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2c/58/ca301544e1fa93ed4f80d724bf5b194f6e4b945841c5bfd555878eea9fcb/referencing-0.37.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e6/34/ebdc18bae6aa14fbee1a08b63c015c72b64868ff7dae68808ab500c492e2/tinycss2-1.4.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3b/5d/63d4ae3b9daea098d5d6f5da83984853c1bbacd5dc826764b249fe119d24/requests_oauthlib-2.0.0-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3c/2e/8d0c2ab90a8c1d9a24f0399058ab8519a3279d1bd4289511d74e909f060e/markupsafe-3.0.3-cp312-cp312-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/39/799be3f2f0f38cc727ee3b4f1445fe6d5e4133064ec2e4115069418a5bb6/cloudpickle-3.1.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/5f/ed/539768cf28c661b5b068d66d96a2f155c4971a5d55684a514c1a0e0dec2f/python_dotenv-1.1.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/54/20/4d324d65cc6d9205fabedc306948156824eb9f0ee1633355a8f7ec5c66bf/pluggy-1.6.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/73/cb/ac7874b3e5d58441674fb70742e6c374b28b0c7cb988d37d991cde47166c/platformdirs-4.5.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/41/45/1a4ed80516f02155c51f51e8cedb3c1902296743db0bbc66608a0db2814f/jsonschema_specifications-2025.9.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c2/14/e2a54fabd4f08cd7af1c07030603c3356b74da07f7cc056e600436edfa17/tzlocal-5.3.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a6/24/4d91e05817e92e3a61c8a21e08fd0f390f5301f1c448b137c57c4bc6e543/semver-3.0.4-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/36/f4/c6e662dade71f56cd2f3735141b265c3c79293c109549c1e6933b0651ffc/exceptiongroup-1.3.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f5/10/6c25ed6de94c49f88a91fa5018cb4c0f3625f31d5be9f771ebe5cc7cd506/aiosqlite-0.21.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0f/e7/aa315e6a749d9b96c2504a1ba0ba031ba2d0517e972ce22682e3fccecb09/cssselect2-0.8.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/17/69/cd203477f944c353c31bade965f880aa1061fd6bf05ded0726ca845b6ff7/typing_inspection-0.4.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/48/30/47d0bf6072f7252e6521f3447ccfa40b421b6824517f82854703d0f5a98b/hyperframe-6.1.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/73/07/02e16ed01e04a374e644b575638ec7987ae846d25ad97bcc9945a3ee4b0e/jsonpatch-1.33-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f4/24/2a3e3df732393fed8b3ebf2ec078f05546de641fe1b667ee316ec1dcf3b7/webencodings-0.5.1-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2f/f5/c36551e93acba41a59939ae6a0fb77ddb3f2e8e8caa716410c65f7341f72/asgi_lifespan-2.1.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2e/54/647ade08bf0db230bfea292f893923872fd20be6ac6f53b2b936ba839d75/zipp-3.23.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/72/76/20fa66124dbe6be5cafeb312ece67de6b61dd91a0247d1ea13db4ebb33c2/cachetools-5.5.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a4/62/02da182e544a51a5c3ccf4b03ab79df279f9c60c5e82d5e8bec7ca26ac11/python_slugify-8.0.4-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a9/10/e4b1e0e5b6b6745c8098c275b69bc9d73e9542d5c7da4f137542b499ed44/readchar-4.2.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4e/8c/f3147f5c4b73e7550fe5f9352eaa956ae838d5c51eb58e7a25b9f3e2643b/decorator-5.2.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c9/ac/d5db977deaf28c6ecbc61bbca269eb3e8f0b3a1f55c8549e5333e606e005/pydyf-0.11.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/71/92/5e77f98553e9e75130c78900d000368476aed74276eb8ae8796f65f00918/jsonpointer-3.0.0-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/1b/f7ea6cde25621cd9236541c66ff018f4268012a534ec31032bcb187dc5e7/proglog-0.1.12-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/b4/08c9d297edd5e1182506edecccbb88a92e1122a057953068cadac420ca5d/jinja2_humanize_extension-0.4.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7b/44/4e421b96b67b2daff264473f7465db72fbdf36a07e05494f50300cc7b0c6/rfc3339_validator-0.1.4-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e9/96/356eb17d39992b525ffb1407d91936c6cb09411f10894a394db4777801c5/screenplain-0.11.1.tar.gz
Downloading opencv-python (63.9MiB)
Downloading imageio-ffmpeg (28.1MiB)
Downloading fonttools (4.7MiB)
Downloading numpy (15.8MiB)
Downloading lxml (5.0MiB)
Downloading reportlab (1.9MiB)
Downloading pyphen (2.0MiB)
Downloading brotli (1.4MiB)
Downloading pygments (1.2MiB)
DEBUG Downloading source distribution: screenplain==0.11.1
   Building screenplain==0.11.1
DEBUG Building: screenplain==0.11.1
DEBUG Not using uv build backend direct build of screenplain==0.11.1, no pyproject.toml: failed to open file `/home/chasezhi/.cache/uv/sdists-v9/pypi/screenplain/0.11.1/ZzhWn6FeaFXzMs2xjEYps/src/pyproject.toml`: No such file or directory (os error 2)
DEBUG Assessing Python executable as base candidate: /usr/bin/python3.12
DEBUG Reusing existing build environment for: screenplain==0.11.1
DEBUG Using base executable for virtual environment: /usr/bin/python3.12
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==80.9.0 [compatible] (setuptools-80.9.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG marker environment resolution took 0.196s
DEBUG Installing in setuptools==80.9.0 in /home/chasezhi/.cache/uv/builds-v0/.tmpvEq8DL
DEBUG Registry requirement already cached: setuptools==80.9.0
DEBUG Installing build requirement: setuptools==80.9.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG /home/chasezhi/.cache/uv/builds-v0/.tmpvEq8DL/lib/python3.12/site-packages/setuptools/dist.py:759: SetuptoolsDeprecationWarning: License classifiers are deprecated.
DEBUG !!
DEBUG
DEBUG         ********************************************************************************
DEBUG         Please consider removing the following classifiers in favor of a SPDX license expression:
DEBUG
DEBUG         License :: OSI Approved :: MIT License
DEBUG
DEBUG         See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
DEBUG         ********************************************************************************
DEBUG
DEBUG !!
DEBUG   self._finalize_license_expression()
DEBUG running egg_info
DEBUG writing screenplain.egg-info/PKG-INFO
DEBUG writing dependency_links to screenplain.egg-info/dependency_links.txt
DEBUG writing entry points to screenplain.egg-info/entry_points.txt
DEBUG writing requirements to screenplain.egg-info/requires.txt
DEBUG writing top-level names to screenplain.egg-info/top_level.txt
DEBUG reading manifest file 'screenplain.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE.txt'
DEBUG writing manifest file 'screenplain.egg-info/SOURCES.txt'
DEBUG Locking the source tree for setuptools
DEBUG Acquired lock for `/home/chasezhi/.cache/uv/sdists-v9/pypi/screenplain/0.11.1/ZzhWn6FeaFXzMs2xjEYps/src`
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("/home/chasezhi/.cache/uv/builds-v0/.tmppF3RWF", {}, None)`
DEBUG /home/chasezhi/.cache/uv/builds-v0/.tmpvEq8DL/lib/python3.12/site-packages/setuptools/dist.py:759: SetuptoolsDeprecationWarning: License classifiers are deprecated.
DEBUG !!
DEBUG
DEBUG         ********************************************************************************
DEBUG         Please consider removing the following classifiers in favor of a SPDX license expression:
DEBUG
DEBUG         License :: OSI Approved :: MIT License
DEBUG
DEBUG         See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
DEBUG         ********************************************************************************
DEBUG
DEBUG !!
DEBUG   self._finalize_license_expression()
DEBUG running bdist_wheel
DEBUG running build
DEBUG running build_py
DEBUG creating build/lib/screenplain
DEBUG copying screenplain/__init__.py -> build/lib/screenplain
DEBUG copying screenplain/richstring.py -> build/lib/screenplain
DEBUG copying screenplain/main.py -> build/lib/screenplain
DEBUG copying screenplain/types.py -> build/lib/screenplain
DEBUG creating build/lib/screenplain/export
DEBUG copying screenplain/export/__init__.py -> build/lib/screenplain/export
DEBUG copying screenplain/export/html.py -> build/lib/screenplain/export
DEBUG copying screenplain/export/fdx.py -> build/lib/screenplain/export
DEBUG copying screenplain/export/pdf.py -> build/lib/screenplain/export
DEBUG creating build/lib/screenplain/parsers
DEBUG copying screenplain/parsers/__init__.py -> build/lib/screenplain/parsers
DEBUG copying screenplain/parsers/fountain.py -> build/lib/screenplain/parsers
DEBUG copying screenplain/export/default.css -> build/lib/screenplain/export
DEBUG installing to build/bdist.linux-x86_64/wheel
DEBUG running install
DEBUG running install_lib
DEBUG creating build/bdist.linux-x86_64/wheel
DEBUG creating build/bdist.linux-x86_64/wheel/screenplain
DEBUG copying build/lib/screenplain/__init__.py -> build/bdist.linux-x86_64/wheel/./screenplain
DEBUG copying build/lib/screenplain/richstring.py -> build/bdist.linux-x86_64/wheel/./screenplain
DEBUG copying build/lib/screenplain/main.py -> build/bdist.linux-x86_64/wheel/./screenplain
DEBUG creating build/bdist.linux-x86_64/wheel/screenplain/parsers
DEBUG copying build/lib/screenplain/parsers/__init__.py -> build/bdist.linux-x86_64/wheel/./screenplain/parsers
DEBUG copying build/lib/screenplain/parsers/fountain.py -> build/bdist.linux-x86_64/wheel/./screenplain/parsers
DEBUG copying build/lib/screenplain/types.py -> build/bdist.linux-x86_64/wheel/./screenplain
DEBUG creating build/bdist.linux-x86_64/wheel/screenplain/export
DEBUG copying build/lib/screenplain/export/__init__.py -> build/bdist.linux-x86_64/wheel/./screenplain/export
DEBUG copying build/lib/screenplain/export/html.py -> build/bdist.linux-x86_64/wheel/./screenplain/export
DEBUG copying build/lib/screenplain/export/default.css -> build/bdist.linux-x86_64/wheel/./screenplain/export
DEBUG copying build/lib/screenplain/export/fdx.py -> build/bdist.linux-x86_64/wheel/./screenplain/export
DEBUG copying build/lib/screenplain/export/pdf.py -> build/bdist.linux-x86_64/wheel/./screenplain/export
DEBUG running install_egg_info
DEBUG running egg_info
DEBUG writing screenplain.egg-info/PKG-INFO
DEBUG writing dependency_links to screenplain.egg-info/dependency_links.txt
DEBUG writing entry points to screenplain.egg-info/entry_points.txt
DEBUG writing requirements to screenplain.egg-info/requires.txt
DEBUG writing top-level names to screenplain.egg-info/top_level.txt
DEBUG reading manifest file 'screenplain.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE.txt'
DEBUG writing manifest file 'screenplain.egg-info/SOURCES.txt'
DEBUG Copying screenplain.egg-info to build/bdist.linux-x86_64/wheel/./screenplain-0.11.1-py3.12.egg-info
DEBUG running install_scripts
DEBUG creating build/bdist.linux-x86_64/wheel/screenplain-0.11.1.dist-info/WHEEL
DEBUG creating '/home/chasezhi/.cache/uv/builds-v0/.tmppF3RWF/.tmp-0465touj/screenplain-0.11.1-py3-none-any.whl' and adding 'build/bdist.linux-x86_64/wheel' to it
DEBUG adding 'screenplain/__init__.py'
DEBUG adding 'screenplain/main.py'
DEBUG adding 'screenplain/richstring.py'
DEBUG adding 'screenplain/types.py'
DEBUG adding 'screenplain/export/__init__.py'
DEBUG adding 'screenplain/export/default.css'
DEBUG adding 'screenplain/export/fdx.py'
DEBUG adding 'screenplain/export/html.py'
DEBUG adding 'screenplain/export/pdf.py'
DEBUG adding 'screenplain/parsers/__init__.py'
DEBUG adding 'screenplain/parsers/fountain.py'
DEBUG adding 'screenplain-0.11.1.dist-info/licenses/LICENSE.txt'
DEBUG adding 'screenplain-0.11.1.dist-info/METADATA'
DEBUG adding 'screenplain-0.11.1.dist-info/WHEEL'
DEBUG adding 'screenplain-0.11.1.dist-info/entry_points.txt'
DEBUG adding 'screenplain-0.11.1.dist-info/top_level.txt'
DEBUG adding 'screenplain-0.11.1.dist-info/RECORD'
DEBUG removing build/bdist.linux-x86_64/wheel
DEBUG Released lock at `/tmp/uv-setuptools-3cf1ea5ac902e632.lock`
DEBUG Finished building: screenplain==0.11.1
      Built screenplain==0.11.1
DEBUG Released lock at `/home/chasezhi/.cache/uv/sdists-v9/pypi/screenplain/0.11.1/.lock`
 Downloading apprise
 Downloading pygments
 Downloading brotli
 Downloading pydantic-core
 Downloading reportlab
 Downloading pyphen
 Downloading sqlalchemy
 Downloading asyncpg
 Downloading cryptography
 Downloading fonttools
 Downloading lxml
 Downloading prefect
 Downloading numpy
 Downloading uv
 Downloading imageio-ffmpeg
 Downloading opencv-python
Prepared 133 packages in 33.34s
Installed 137 packages in 160ms
 + aiosqlite==0.21.0
 + alembic==1.17.1
 + annotated-types==0.7.0
 + anyio==4.10.0
 + apprise==1.9.5
 + asgi-lifespan==2.1.0
 + asyncpg==0.30.0
 + attrs==25.4.0
 + brotli==1.2.0
 + cachetools==5.5.2
 + certifi==2025.8.3
 + cffi==2.0.0
 + charset-normalizer==3.4.3
 + click==8.2.1
 + cloudpickle==3.1.2
 + colorama==0.4.6
 + coolname==2.2.0
 + cryptography==46.0.3
 + cssselect2==0.8.0
 + dateparser==1.2.2
 + decorator==5.2.1
 + docker==7.1.0
 + exceptiongroup==1.3.0
 + fastapi==0.117.1
 + fonttools==4.60.1
 + fsspec==2025.10.0
 + google-api-core==2.25.1
 + google-auth==2.40.3
 + google-cloud-core==2.4.3
 + google-cloud-storage==3.4.0
 + google-crc32c==1.7.1
 + google-genai==1.38.0
 + google-resumable-media==2.7.2
 + googleapis-common-protos==1.70.0
 + graphviz==0.21
 + greenlet==3.2.4
 + griffe==1.14.0
 + h11==0.16.0
 + h2==4.3.0
 + hpack==4.1.0
 + httpcore==1.0.9
 + httpx==0.28.1
 + humanize==4.14.0
 + hyperframe==6.1.0
 + idna==3.10
 + imageio==2.37.0
 + imageio-ffmpeg==0.6.0
 + importlib-metadata==8.7.0
 + jinja2==3.1.6
 + jinja2-humanize-extension==0.4.0
 + jsonpatch==1.33
 + jsonpointer==3.0.0
 + jsonschema==4.25.1
 + jsonschema-specifications==2025.9.1
 + loguru==0.7.3
 + lxml==6.0.2
 + mako==1.3.10
 + markdown==3.10
 + markdown-it-py==4.0.0
 + markupsafe==3.0.3
 + mdurl==0.1.2
 + moviepy==2.2.1
 + numpy==2.2.6
 + oauthlib==3.3.1
 + opencv-python==4.12.0.88
 + opentelemetry-api==1.38.0
 + orjson==3.11.4
 + packaging==25.0
 + pathspec==0.12.1
 + pendulum==3.1.0
 + pillow==11.3.0
 + platformdirs==4.5.0
 + pluggy==1.6.0
 + prefect==3.5.0
 + proglog==0.1.12
 + prometheus-client==0.23.1
 + prompt-toolkit==3.0.52
 + proto-plus==1.26.1
 + protobuf==6.32.1
 + pyasn1==0.6.1
 + pyasn1-modules==0.4.2
 + pycparser==2.23
 + pydantic==2.11.9
 + pydantic-core==2.33.2
 + pydantic-extra-types==2.10.6
 + pydantic-settings==2.11.0
 + pydyf==0.11.0
 + pygments==2.19.2
 + pypdf2==3.0.1
 + pyphen==0.17.2
 + python-dateutil==2.9.0.post0
 + python-docx==1.2.0
 + python-dotenv==1.1.1
 + python-slugify==8.0.4
 + pytz==2025.2
 + pyyaml==6.0.2
 + questionary==2.1.1
 + readchar==4.2.1
 + referencing==0.37.0
 + regex==2025.11.3
 + reportlab==4.4.4
 + requests==2.32.5
 + requests-oauthlib==2.0.0
 + rfc3339-validator==0.1.4
 + rich==14.2.0
 + rpds-py==0.28.0
 + rsa==4.9.1
 + ruamel-yaml==0.18.16
 + ruamel-yaml-clib==0.2.14
 + scenedetect==0.6.7.1
 + screenplain==0.11.1
 + semver==3.0.4
 + shellingham==1.5.4
 + six==1.17.0
 + sniffio==1.3.1
 + sqlalchemy==2.0.44
 + starlette==0.48.0
 + tenacity==9.1.2
 + text-unidecode==1.3
 + tinycss2==1.4.0
 + tinyhtml5==2.0.0
 + toml==0.10.2
 + tqdm==4.67.1
 + typer==0.19.2
 + typing-extensions==4.15.0
 + typing-inspection==0.4.1
 + tzdata==2025.2
 + tzlocal==5.3.1
 + urllib3==2.5.0
 + uv==0.9.7
 + uvicorn==0.37.0
 + wcwidth==0.2.14
 + weasyprint==66.0
 + webencodings==0.5.1
 + websockets==15.0.1
 + zipp==3.23.0
 + zopfli==0.4.0
DEBUG Released lock at `/home/chasezhi/jubenshengcheng/.venv/.lock`
DEBUG Using Python 3.12.3 interpreter at: /home/chasezhi/jubenshengcheng/.venv/bin/python3
Provide a command or script to invoke with `uv run <command>` or `uv run <script>.py`.

The following commands are available in the environment:

- alembic
- apprise
- coolname
- dateparser-download
- dotenv
- f2py
- fastapi
- fonttools
- griffe
- httpx
- imageio_download_bin
- imageio_remove_bin
- jsondiff
- jsonpatch
- jsonpointer
- jsonschema
- mako-render
- markdown-it
- markdown_py
- normalizer
- numpy-config
- prefect
- pyftmerge
- pyftsubset
- pygmentize
- pyrsa-decrypt
- pyrsa-encrypt
- pyrsa-keygen
- pyrsa-priv2pub
- pyrsa-sign
- pyrsa-verify
- pysemver
- python
- python3
- python3.12
- scenedetect
- screenplain
- slugify
- tqdm
- ttx
- typer
- uv
- uvicorn
- uvx
- weasyprint
- websockets

See `uv run --help` for more information.
```

---

_Comment by @konstin on 2025-11-14 08:10_

I cannot reproduce this, if I run `docker run --rm -it ghcr.io/astral-sh/uv:python3.12-trixie bash`, add the `pyproject.toml` you shared inside it and run `uv run python -c "from screenplain.export.pdf import to_pdf"`, it passes.

---

_Label `bug` removed by @konstin on 2025-11-14 08:10_

---

_Label `needs-mre` added by @konstin on 2025-11-14 08:10_

---

_Comment by @zhiquanchi on 2025-11-14 08:45_

I'm testing it now. It's completely working fine. It might be because earlier when I installed it, I installed `screenplain` first instead of `screenplain[pdf]`. It was only later that I installed the version with PDF support.

---

_Closed by @zhiquanchi on 2025-11-14 08:45_

---
