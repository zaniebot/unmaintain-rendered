---
number: 17144
title: "401 when trying to `uv sync` on an Azure DevOps Server private index using `keyring`"
type: issue
state: open
author: ubalklen
labels:
  - documentation
assignees: []
created_at: 2025-12-16T13:09:07Z
updated_at: 2026-01-07T13:37:31Z
url: https://github.com/astral-sh/uv/issues/17144
synced_at: 2026-01-10T01:26:14Z
---

# 401 when trying to `uv sync` on an Azure DevOps Server private index using `keyring`

---

_Issue opened by @ubalklen on 2025-12-16 13:09_

### Summary

```powershell
PS C:\Users\username\OneDrive\Code\my-package> uv tool install keyring --with artifacts-keyring
`keyring` is already installed
PS C:\Users\username\OneDrive\Code\my-package> uv sync --native-tls --verbose --no-cache 
```

Relevant logs:

```
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-fastapi\opentelemetry_instrumentation_fastapi-0.60b0-py3-none-any.lock`
  × Failed to download `debugpy==1.8.19`
  ├─▶ Failed to fetch:
  │   `https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/debugpy/1.8.19/debugpy-1.8.19-cp313-cp313-win_amd64.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url
      (https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/debugpy/1.8.19/debugpy-1.8.19-cp313-cp313-win_amd64.whl)
  help: `debugpy` (v1.8.19) was included because `my-package:dev` (v0.1.0.dev0) depends on `ipykernel` (v7.1.0) which depends on `debugpy`
```

<details>

<summary>Full log (redacted)</summary>

```
DEBUG uv 0.9.17 (2b5d65e61 2025-12-09)
DEBUG Disabling the uv cache due to `--no-cache`
DEBUG Acquired shared lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD`
DEBUG Found project root: `C:\Users\username\OneDrive\Code\my-package`
DEBUG No workspace root found, using project root
DEBUG Acquired exclusive lock for `C:\Users\username\OneDrive\Code\my-package`
DEBUG No Python version file found in workspace: C:\Users\username\OneDrive\Code\my-package
DEBUG Using Python request `>=3.10, <3.14` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.10, <3.14`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\uv-06a696fc6b698a38.lock`
DEBUG Acquired exclusive lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: my-package @ file:///C:/Users/username/OneDrive/Code/my-package
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 133 packages in 13ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: my-package @ file:///C:/Users/username/OneDrive/Code/my-package
DEBUG Identified uncached distribution: azure-devops==7.1.0b4
DEBUG Identified uncached distribution: ipykernel==7.1.0
DEBUG Identified uncached distribution: pytest==9.0.2
DEBUG Identified uncached distribution: pytest-cov==7.0.0
DEBUG Identified uncached distribution: pytest-recording==0.13.4
DEBUG Identified uncached distribution: python-dotenv==1.2.1
DEBUG Identified uncached distribution: ruff==0.14.9
DEBUG Identified uncached distribution: artifacts-keyring==1.0.0
DEBUG Identified uncached distribution: azure-monitor-opentelemetry==1.8.3
DEBUG Identified uncached distribution: beautifulsoup4==4.14.3
DEBUG Identified uncached distribution: bradocs4py==1.3.1.0
DEBUG Identified uncached distribution: cryptography==46.0.3
DEBUG Identified uncached distribution: deprecated==1.3.1
DEBUG Identified uncached distribution: pypdf==6.4.2
DEBUG Identified uncached distribution: pythonnet==3.0.5
DEBUG Identified uncached distribution: pywin32==311
DEBUG Identified uncached distribution: requests==2.32.5
DEBUG Identified uncached distribution: requests-kerberos==0.15.0
DEBUG Identified uncached distribution: requests-ntlm==1.3.0
DEBUG Identified uncached distribution: selectorlib==0.16.0
DEBUG Identified uncached distribution: selenium==4.39.0
DEBUG Identified uncached distribution: setuptools==80.9.0
DEBUG Identified uncached distribution: tqdm==4.67.1
DEBUG Identified uncached distribution: truststore==0.10.4
DEBUG Identified uncached distribution: undetected-chromedriver==3.5.5
DEBUG Identified uncached distribution: unidecode==1.4.0
DEBUG Identified uncached distribution: msrest==0.7.1
DEBUG Identified uncached distribution: comm==0.2.3
DEBUG Identified uncached distribution: debugpy==1.8.19
DEBUG Identified uncached distribution: ipython==9.8.0
DEBUG Identified uncached distribution: jupyter-client==8.7.0
DEBUG Identified uncached distribution: jupyter-core==5.9.1
DEBUG Identified uncached distribution: matplotlib-inline==0.2.1
DEBUG Identified uncached distribution: nest-asyncio==1.6.0
DEBUG Identified uncached distribution: packaging==25.0
DEBUG Identified uncached distribution: psutil==7.1.3
DEBUG Identified uncached distribution: pyzmq==27.1.0
DEBUG Identified uncached distribution: tornado==6.5.4
DEBUG Identified uncached distribution: traitlets==5.14.3
DEBUG Identified uncached distribution: colorama==0.4.6
DEBUG Identified uncached distribution: iniconfig==2.3.0
DEBUG Identified uncached distribution: pluggy==1.6.0
DEBUG Identified uncached distribution: pygments==2.19.2
DEBUG Identified uncached distribution: coverage==7.13.0
DEBUG Identified uncached distribution: vcrpy==8.1.0
DEBUG Identified uncached distribution: keyring==25.7.0
DEBUG Identified uncached distribution: azure-core==1.37.0
DEBUG Identified uncached distribution: azure-core-tracing-opentelemetry==1.0.0b12
DEBUG Identified uncached distribution: azure-monitor-opentelemetry-exporter==1.0.0b46
DEBUG Identified uncached distribution: opentelemetry-instrumentation-django==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-fastapi==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-flask==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-psycopg2==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-requests==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-urllib==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-urllib3==0.60b0
DEBUG Identified uncached distribution: opentelemetry-resource-detector-azure==0.1.5
DEBUG Identified uncached distribution: opentelemetry-sdk==1.39.0
DEBUG Identified uncached distribution: soupsieve==2.8
DEBUG Identified uncached distribution: typing-extensions==4.15.0
DEBUG Identified uncached distribution: cffi==2.0.0
DEBUG Identified uncached distribution: wrapt==1.17.3
DEBUG Identified uncached distribution: clr-loader==0.2.9
DEBUG Identified uncached distribution: certifi==2025.11.12
DEBUG Identified uncached distribution: charset-normalizer==3.4.4
DEBUG Identified uncached distribution: idna==3.11
DEBUG Identified uncached distribution: urllib3==2.6.2
DEBUG Identified uncached distribution: pyspnego==0.12.0
DEBUG Identified uncached distribution: click==8.3.1
DEBUG Identified uncached distribution: parsel==1.10.0
DEBUG Identified uncached distribution: pyyaml==6.0.3
DEBUG Identified uncached distribution: trio==0.32.0
DEBUG Identified uncached distribution: trio-websocket==0.12.2
DEBUG Identified uncached distribution: websocket-client==1.9.0
DEBUG Identified uncached distribution: websockets==15.0.1
DEBUG Identified uncached distribution: isodate==0.7.2
DEBUG Identified uncached distribution: requests-oauthlib==2.0.0
DEBUG Identified uncached distribution: decorator==5.2.1
DEBUG Identified uncached distribution: ipython-pygments-lexers==1.1.1
DEBUG Identified uncached distribution: jedi==0.19.2
DEBUG Identified uncached distribution: prompt-toolkit==3.0.52
DEBUG Identified uncached distribution: stack-data==0.6.3
DEBUG Identified uncached distribution: python-dateutil==2.9.0.post0
DEBUG Identified uncached distribution: platformdirs==4.5.1
DEBUG Identified uncached distribution: jaraco-classes==3.4.0
DEBUG Identified uncached distribution: jaraco-context==6.0.1
DEBUG Identified uncached distribution: jaraco-functools==4.3.0
DEBUG Identified uncached distribution: pywin32-ctypes==0.2.3
DEBUG Identified uncached distribution: opentelemetry-api==1.39.0
DEBUG Identified uncached distribution: azure-identity==1.25.1
DEBUG Identified uncached distribution: opentelemetry-instrumentation==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-wsgi==0.60b0
DEBUG Identified uncached distribution: opentelemetry-semantic-conventions==0.60b0
DEBUG Identified uncached distribution: opentelemetry-util-http==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-asgi==0.60b0
DEBUG Identified uncached distribution: opentelemetry-instrumentation-dbapi==0.60b0
DEBUG Identified uncached distribution: pycparser==2.23
DEBUG Identified uncached distribution: sspilib==0.5.0
DEBUG Identified uncached distribution: cssselect==1.3.0
DEBUG Identified uncached distribution: jmespath==1.0.1
DEBUG Identified uncached distribution: lxml==6.0.2
DEBUG Identified uncached distribution: w3lib==2.3.1
DEBUG Identified uncached distribution: attrs==25.4.0
DEBUG Identified uncached distribution: outcome==1.3.0.post0
DEBUG Identified uncached distribution: sniffio==1.3.1
DEBUG Identified uncached distribution: sortedcontainers==2.4.0
DEBUG Identified uncached distribution: wsproto==1.3.2
DEBUG Identified uncached distribution: pysocks==1.7.1
DEBUG Identified uncached distribution: oauthlib==3.3.1
DEBUG Identified uncached distribution: parso==0.8.5
DEBUG Identified uncached distribution: wcwidth==0.2.14
DEBUG Identified uncached distribution: asttokens==3.0.1
DEBUG Identified uncached distribution: executing==2.2.1
DEBUG Identified uncached distribution: pure-eval==0.2.3
DEBUG Identified uncached distribution: six==1.17.0
DEBUG Identified uncached distribution: more-itertools==10.8.0
DEBUG Identified uncached distribution: importlib-metadata==8.7.0
DEBUG Identified uncached distribution: msal==1.34.0
DEBUG Identified uncached distribution: msal-extensions==1.3.1
DEBUG Identified uncached distribution: asgiref==3.11.0
DEBUG Identified uncached distribution: h11==0.16.0
DEBUG Identified uncached distribution: zipp==3.23.0
DEBUG Identified uncached distribution: pyjwt==2.10.1
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\sdists-v9\editable\d4688698bdefab1f`
DEBUG Computed cache info: Timestamp(SystemTime { intervals: 134103613270379379 }), None, None, {}, {"src": None}. Most recently modified: pyproject.toml
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-devops\azure_devops-7.1.0b4-py3-none-any.lock` 
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\ipykernel\ipykernel-7.1.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pytest\pytest-9.0.2-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pytest-cov\pytest_cov-7.0.0-py3-none-any.lock`       
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pytest-recording\pytest_recording-0.13.4-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\python-dotenv\python_dotenv-1.2.1-py3-none-any.lock` 
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\ruff\ruff-0.14.9-py3-none-win_amd64.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\artifacts-keyring\artifacts_keyring-1.0.0-cp313-cp313-win_amd64.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-monitor-opentelemetry\azure_monitor_opentelemetry-1.8.3-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\beautifulsoup4\beautifulsoup4-4.14.3-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\bradocs4py\bradocs4py-1.3.1.0-py3-none-any.lock`     
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\cryptography\cryptography-46.0.3-cp311-abi3-win_amd64.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\deprecated\deprecated-1.3.1-py2.py3-none-any.lock`   
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pypdf\pypdf-6.4.2-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pythonnet\pythonnet-3.0.5-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pywin32\pywin32-311-cp313-cp313-win_amd64.lock`      
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\requests\requests-2.32.5-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\requests-kerberos\requests_kerberos-0.15.0-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\requests-ntlm\requests_ntlm-1.3.0-py3-none-any.lock` 
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\selectorlib\selectorlib-0.16.0-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\selenium\selenium-4.39.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\setuptools\setuptools-80.9.0-py3-none-any.lock`      
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\tqdm\tqdm-4.67.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\truststore\truststore-0.10.4-py3-none-any.lock`      
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\sdists-v9\index\0a0b26c57806d6e9\undetected-chromedriver\3.5.5`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\unidecode\unidecode-1.4.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\msrest\msrest-0.7.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\comm\comm-0.2.3-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\debugpy\debugpy-1.8.19-cp313-cp313-win_amd64.lock`   
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\ipython\ipython-9.8.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jupyter-client\jupyter_client-8.7.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jupyter-core\jupyter_core-5.9.1-py3-none-any.lock`   
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\matplotlib-inline\matplotlib_inline-0.2.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\nest-asyncio\nest_asyncio-1.6.0-py3-none-any.lock`   
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\packaging\packaging-25.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\psutil\psutil-7.1.3-cp37-abi3-win_amd64.lock`        
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pyzmq\pyzmq-27.1.0-cp312-abi3-win_amd64.lock`        
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\tornado\tornado-6.5.4-cp39-abi3-win_amd64.lock`      
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\traitlets\traitlets-5.14.3-py3-none-any.lock`        
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\colorama\colorama-0.4.6-py2.py3-none-any.lock`       
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\iniconfig\iniconfig-2.3.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pluggy\pluggy-1.6.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pygments\pygments-2.19.2-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\coverage\coverage-7.13.0-cp313-cp313-win_amd64.lock` 
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\vcrpy\vcrpy-8.1.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\keyring\keyring-25.7.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-core\azure_core-1.37.0-py3-none-any.lock`      
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-core-tracing-opentelemetry\azure_core_tracing_opentelemetry-1.0.0b12-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-monitor-opentelemetry-exporter\azure_monitor_opentelemetry_exporter-1.0.0b46-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-django\opentelemetry_instrumentation_django-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-fastapi\opentelemetry_instrumentation_fastapi-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-flask\opentelemetry_instrumentation_flask-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-psycopg2\opentelemetry_instrumentation_psycopg2-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-requests\opentelemetry_instrumentation_requests-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-urllib\opentelemetry_instrumentation_urllib-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-urllib3\opentelemetry_instrumentation_urllib3-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-resource-detector-azure\opentelemetry_resource_detector_azure-0.1.5-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-sdk\opentelemetry_sdk-1.39.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\soupsieve\soupsieve-2.8-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\typing-extensions\typing_extensions-4.15.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\cffi\cffi-2.0.0-cp313-cp313-win_amd64.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\wrapt\wrapt-1.17.3-cp313-cp313-win_amd64.lock`       
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\clr-loader\clr_loader-0.2.9-py3-none-any.lock`       
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\certifi\certifi-2025.11.12-py3-none-any.lock`        
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\charset-normalizer\charset_normalizer-3.4.4-cp313-cp313-win_amd64.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\idna\idna-3.11-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\urllib3\urllib3-2.6.2-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pyspnego\pyspnego-0.12.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\click\click-8.3.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\parsel\parsel-1.10.0-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pyyaml\pyyaml-6.0.3-cp313-cp313-win_amd64.lock`      
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\trio\trio-0.32.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\trio-websocket\trio_websocket-0.12.2-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\websocket-client\websocket_client-1.9.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\websockets\websockets-15.0.1-cp313-cp313-win_amd64.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\isodate\isodate-0.7.2-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\requests-oauthlib\requests_oauthlib-2.0.0-py2.py3-none-any.lock`
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/azure-devops/7.1b4/azure_devops-7.1.0b4-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/ipykernel/7.1/ipykernel-7.1.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pytest/9.0.2/pytest-9.0.2-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pytest-cov/7/pytest_cov-7.0.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pytest-recording/0.13.4/pytest_recording-0.13.4-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/python-dotenv/1.2.1/python_dotenv-1.2.1-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/ruff/0.14.9/ruff-0.14.9-py3-none-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/artifacts-keyring/1/artifacts_keyring-1.0.0-cp313-cp313-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/azure-monitor-opentelemetry/1.8.3/azure_monitor_opentelemetry-1.8.3-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/beautifulsoup4/4.14.3/beautifulsoup4-4.14.3-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/bradocs4py/1.3.1/bradocs4py-1.3.1.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/cryptography/46.0.3/cryptography-46.0.3-cp311-abi3-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/deprecated/1.3.1/deprecated-1.3.1-py2.py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pypdf/6.4.2/pypdf-6.4.2-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pythonnet/3.0.5/pythonnet-3.0.5-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pywin32/311/pywin32-311-cp313-cp313-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/requests/2.32.5/requests-2.32.5-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/requests-kerberos/0.15/requests_kerberos-0.15.0-py2.py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/requests-ntlm/1.3/requests_ntlm-1.3.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/selectorlib/0.16/selectorlib-0.16.0-py2.py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/selenium/4.39/selenium-4.39.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/setuptools/80.9/setuptools-80.9.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/tqdm/4.67.1/tqdm-4.67.1-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/truststore/0.10.4/truststore-0.10.4-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/undetected-chromedriver/3.5.5/undetected-chromedriver-3.5.5.tar.gz
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/unidecode/1.4/Unidecode-1.4.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/msrest/0.7.1/msrest-0.7.1-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/comm/0.2.3/comm-0.2.3-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/debugpy/1.8.19/debugpy-1.8.19-cp313-cp313-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/ipython/9.8/ipython-9.8.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/jupyter-client/8.7/jupyter_client-8.7.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/jupyter-core/5.9.1/jupyter_core-5.9.1-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/matplotlib-inline/0.2.1/matplotlib_inline-0.2.1-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/nest-asyncio/1.6/nest_asyncio-1.6.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/packaging/25/packaging-25.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/psutil/7.1.3/psutil-7.1.3-cp37-abi3-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pyzmq/27.1/pyzmq-27.1.0-cp312-abi3-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/tornado/6.5.4/tornado-6.5.4-cp39-abi3-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/traitlets/5.14.3/traitlets-5.14.3-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/colorama/0.4.6/colorama-0.4.6-py2.py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/iniconfig/2.3/iniconfig-2.3.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pluggy/1.6/pluggy-1.6.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/pygments/2.19.2/pygments-2.19.2-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/coverage/7.13/coverage-7.13.0-cp313-cp313-win_amd64.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/vcrpy/8.1/vcrpy-8.1.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/keyring/25.7/keyring-25.7.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/azure-core/1.37/azure_core-1.37.0-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/azure-core-tracing-opentelemetry/1b12/azure_core_tracing_opentelemetry-1.0.0b12-py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/azure-monitor-opentelemetry-exporter/1b46/azure_monitor_opentelemetry_exporter-1.0.0b46-py2.py3-none-any.whl
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/opentelemetry-instrumentation-django/0.60b0/opentelemetry_instrumentation_django-0.60b0-py3-none-any.whl
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pyjwt\pyjwt-2.10.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\zipp\zipp-3.23.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\h11\h11-0.16.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\asgiref\asgiref-3.11.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\msal-extensions\msal_extensions-1.3.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\msal\msal-1.34.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\importlib-metadata\importlib_metadata-8.7.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\more-itertools\more_itertools-10.8.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\six\six-1.17.0-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pure-eval\pure_eval-0.2.3-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\executing\executing-2.2.1-py2.py3-none-any.lock`     
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\asttokens\asttokens-3.0.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\wcwidth\wcwidth-0.2.14-py2.py3-none-any.lock`        
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\parso\parso-0.8.5-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\oauthlib\oauthlib-3.3.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pysocks\pysocks-1.7.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\wsproto\wsproto-1.3.2-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\sortedcontainers\sortedcontainers-2.4.0-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\sniffio\sniffio-1.3.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\outcome\outcome-1.3.0.post0-py2.py3-none-any.lock`   
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\attrs\attrs-25.4.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\w3lib\w3lib-2.3.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\lxml\lxml-6.0.2-cp313-cp313-win_amd64.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jmespath\jmespath-1.0.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\cssselect\cssselect-1.3.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\sspilib\sspilib-0.5.0-cp311-abi3-win_amd64.lock`     
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pycparser\pycparser-2.23-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-dbapi\opentelemetry_instrumentation_dbapi-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-asgi\opentelemetry_instrumentation_asgi-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-util-http\opentelemetry_util_http-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-semantic-conventions\opentelemetry_semantic_conventions-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-wsgi\opentelemetry_instrumentation_wsgi-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation\opentelemetry_instrumentation-0.60b0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-identity\azure_identity-1.25.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-api\opentelemetry_api-1.39.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pywin32-ctypes\pywin32_ctypes-0.2.3-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jaraco-functools\jaraco_functools-4.3.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jaraco-context\jaraco_context-6.0.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jaraco-classes\jaraco_classes-3.4.0-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\platformdirs\platformdirs-4.5.1-py3-none-any.lock`   
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\python-dateutil\python_dateutil-2.9.0.post0-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\stack-data\stack_data-0.6.3-py3-none-any.lock`       
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\prompt-toolkit\prompt_toolkit-3.0.52-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jedi\jedi-0.19.2-py2.py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\ipython-pygments-lexers\ipython_pygments_lexers-1.1.1-py3-none-any.lock`
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\decorator\decorator-5.2.1-py3-none-any.lock`
   Building my-package @ file:///C:/Users/username/OneDrive/Code/my-package
DEBUG Building: my-package @ file:///C:/Users/username/OneDrive/Code/my-package
DEBUG Not using uv build backend direct build of `my-package @ file:///C:/Users/username/OneDrive/Code/my-package`, pyproject.toml does not match: The value for `build_system.build-backend` should be `"uv_build"`, not `"setuptools.build_meta"`
DEBUG Reusing existing build environment for: my-package @ file:///C:/Users/username/OneDrive/Code/my-package
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: C:\Users\username\AppData\Roaming\uv\python\cpython-3.13.11-windows-x86_64-none\python.exe
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.11
DEBUG Solving with target Python version: >=3.13.11
DEBUG Adding direct dependency: setuptools>=61.0
DEBUG Acquired exclusive lock for `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\simple-v18\index\0a0b26c57806d6e9\setuptools.lock`
DEBUG No cache entry for: https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/MyFeed/pypi/simple/setuptools/
DEBUG No netrc file found
DEBUG Acquired exclusive lock for `credentials store`
DEBUG Loaded credential file C:\Users\username\AppData\Roaming\uv\credentials\credentials.toml
DEBUG Released lock at `C:\Users\username\AppData\Roaming\uv\credentials\credentials.toml.lock`
DEBUG Checking text store for credentials for https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/debugpy/1.8.19/debugpy-1.8.19-cp313-cp313-win_amd64.whl
DEBUG Skipping keyring fetch for https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/debugpy/1.8.19/debugpy-1.8.19-cp313-cp313-win_amd64.whl without username; use `authenticate = always` to force
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\debugpy\debugpy-1.8.19-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\simple-v18\index\0a0b26c57806d6e9\setuptools.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\sdists-v9\editable\d4688698bdefab1f\.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\msrest\msrest-0.7.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\unidecode\unidecode-1.4.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\tqdm\tqdm-4.67.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\setuptools\setuptools-80.9.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\selectorlib\selectorlib-0.16.0-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\requests-ntlm\requests_ntlm-1.3.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\requests-kerberos\requests_kerberos-0.15.0-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\requests\requests-2.32.5-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pythonnet\pythonnet-3.0.5-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\deprecated\deprecated-1.3.1-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\cryptography\cryptography-46.0.3-cp311-abi3-win_amd64.lock`     
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\bradocs4py\bradocs4py-1.3.1.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\beautifulsoup4\beautifulsoup4-4.14.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\ruff\ruff-0.14.9-py3-none-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\python-dotenv\python_dotenv-1.2.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pytest-recording\pytest_recording-0.13.4-py3-none-any.lock`     
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pytest\pytest-9.0.2-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\ipykernel\ipykernel-7.1.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-django\opentelemetry_instrumentation_django-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-monitor-opentelemetry-exporter\azure_monitor_opentelemetry_exporter-1.0.0b46-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-core-tracing-opentelemetry\azure_core_tracing_opentelemetry-1.0.0b12-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-core\azure_core-1.37.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\keyring\keyring-25.7.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\vcrpy\vcrpy-8.1.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\coverage\coverage-7.13.0-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pygments\pygments-2.19.2-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pluggy\pluggy-1.6.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\iniconfig\iniconfig-2.3.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\colorama\colorama-0.4.6-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\traitlets\traitlets-5.14.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\tornado\tornado-6.5.4-cp39-abi3-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pyzmq\pyzmq-27.1.0-cp312-abi3-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\psutil\psutil-7.1.3-cp37-abi3-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\packaging\packaging-25.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\nest-asyncio\nest_asyncio-1.6.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\matplotlib-inline\matplotlib_inline-0.2.1-py3-none-any.lock`    
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jupyter-core\jupyter_core-5.9.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jupyter-client\jupyter_client-8.7.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-devops\azure_devops-7.1.0b4-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\ipython\ipython-9.8.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\truststore\truststore-0.10.4-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pytest-cov\pytest_cov-7.0.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pywin32\pywin32-311-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\sdists-v9\index\0a0b26c57806d6e9\undetected-chromedriver\3.5.5\.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\selenium\selenium-4.39.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-monitor-opentelemetry\azure_monitor_opentelemetry-1.8.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pypdf\pypdf-6.4.2-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\comm\comm-0.2.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\artifacts-keyring\artifacts_keyring-1.0.0-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\requests-oauthlib\requests_oauthlib-2.0.0-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\decorator\decorator-5.2.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\ipython-pygments-lexers\ipython_pygments_lexers-1.1.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jedi\jedi-0.19.2-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\prompt-toolkit\prompt_toolkit-3.0.52-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\stack-data\stack_data-0.6.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\python-dateutil\python_dateutil-2.9.0.post0-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\platformdirs\platformdirs-4.5.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jaraco-classes\jaraco_classes-3.4.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jaraco-context\jaraco_context-6.0.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jaraco-functools\jaraco_functools-4.3.0-py3-none-any.lock`      
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pywin32-ctypes\pywin32_ctypes-0.2.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-api\opentelemetry_api-1.39.0-py3-none-any.lock`   
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\azure-identity\azure_identity-1.25.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation\opentelemetry_instrumentation-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-wsgi\opentelemetry_instrumentation_wsgi-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-semantic-conventions\opentelemetry_semantic_conventions-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-util-http\opentelemetry_util_http-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-asgi\opentelemetry_instrumentation_asgi-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-dbapi\opentelemetry_instrumentation_dbapi-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pycparser\pycparser-2.23-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\sspilib\sspilib-0.5.0-cp311-abi3-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\cssselect\cssselect-1.3.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\jmespath\jmespath-1.0.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\lxml\lxml-6.0.2-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\w3lib\w3lib-2.3.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\attrs\attrs-25.4.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\outcome\outcome-1.3.0.post0-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\sniffio\sniffio-1.3.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\sortedcontainers\sortedcontainers-2.4.0-py2.py3-none-any.lock`  
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\wsproto\wsproto-1.3.2-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pysocks\pysocks-1.7.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\oauthlib\oauthlib-3.3.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\parso\parso-0.8.5-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\wcwidth\wcwidth-0.2.14-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\asttokens\asttokens-3.0.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\executing\executing-2.2.1-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pure-eval\pure_eval-0.2.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\six\six-1.17.0-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\more-itertools\more_itertools-10.8.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\importlib-metadata\importlib_metadata-8.7.0-py3-none-any.lock`  
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\msal\msal-1.34.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\msal-extensions\msal_extensions-1.3.1-py3-none-any.lock`        
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\asgiref\asgiref-3.11.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\h11\h11-0.16.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\zipp\zipp-3.23.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pyjwt\pyjwt-2.10.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\isodate\isodate-0.7.2-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\websockets\websockets-15.0.1-cp313-cp313-win_amd64.lock`        
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\websocket-client\websocket_client-1.9.0-py3-none-any.lock`      
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\trio-websocket\trio_websocket-0.12.2-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\trio\trio-0.32.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pyyaml\pyyaml-6.0.3-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\parsel\parsel-1.10.0-py2.py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\click\click-8.3.1-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\pyspnego\pyspnego-0.12.0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\urllib3\urllib3-2.6.2-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\idna\idna-3.11-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\charset-normalizer\charset_normalizer-3.4.4-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\certifi\certifi-2025.11.12-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\clr-loader\clr_loader-0.2.9-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\wrapt\wrapt-1.17.3-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\cffi\cffi-2.0.0-cp313-cp313-win_amd64.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\typing-extensions\typing_extensions-4.15.0-py3-none-any.lock`   
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\soupsieve\soupsieve-2.8-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-sdk\opentelemetry_sdk-1.39.0-py3-none-any.lock`   
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-resource-detector-azure\opentelemetry_resource_detector_azure-0.1.5-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-urllib3\opentelemetry_instrumentation_urllib3-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-urllib\opentelemetry_instrumentation_urllib-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-requests\opentelemetry_instrumentation_requests-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-psycopg2\opentelemetry_instrumentation_psycopg2-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-flask\opentelemetry_instrumentation_flask-0.60b0-py3-none-any.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\wheels-v5\index\0a0b26c57806d6e9\opentelemetry-instrumentation-fastapi\opentelemetry_instrumentation_fastapi-0.60b0-py3-none-any.lock`
  × Failed to download `debugpy==1.8.19`
  ├─▶ Failed to fetch:
  │   `https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/debugpy/1.8.19/debugpy-1.8.19-cp313-cp313-win_amd64.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url
      (https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/debugpy/1.8.19/debugpy-1.8.19-cp313-cp313-win_amd64.whl)
  help: `debugpy` (v1.8.19) was included because `my-package:dev` (v0.1.0.dev0) depends on `ipykernel` (v7.1.0) which depends on `debugpy`
DEBUG Released lock at `C:\Users\username\OneDrive\Code\my-package\.venv\.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmpqhfQdD\.lock`
```

</details>

`pyproject.toml`:

```
[project]
name = "my-package"
version = "0.1.0.dev0" 
requires-python = ">=3.10,<3.14"
dependencies = [
    "artifacts-keyring",
    "azure-monitor-opentelemetry",
    "beautifulsoup4",
    "bradocs4py",
    "cryptography",
    "Deprecated",
    "pypdf",
    "pythonnet",
    "pywin32; sys_platform=='win32'",
    "requests",
    "requests-kerberos; sys_platform=='win32'",
    "requests_ntlm",
    "selenium",
    "selectorlib",
    "setuptools", # undeclared dependency of undetected-chromedriver
    "tqdm",
    "truststore",
    "undetected-chromedriver",
    "Unidecode",
]

[dependency-groups]
dev = [
    "pytest",
    "pytest-cov",
    "pytest-recording",
    "ruff",
    "python-dotenv",
    "ipykernel",
    "azure-devops",
]

[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[tool.setuptools.packages.find]
exclude = ["tests*"]

[tool.setuptools.package-data]
"my-package.sistemas" = ["xml/*.xml", "yaml/*.yaml"]

[tool.uv]
keyring-provider = "subprocess"

[[tool.uv.index]]
name = "my_org_feed"
url = "https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/MyOrgFeed/pypi/simple/"
publish-url = "https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/MyOrgFeed/pypi/upload"
```

There is no `uv.toml`.

It *does* work when setting `UV_INDEX_MY_ORG_FEED_USERNAME=dummy` and `UV_INDEX_MY_ORG_FEED_PASSWORD=MyPersonalAccessToken` env variables..

It *also* works when adding a token with `uv auth login my-org.com --token MyPersonalAccessToken`. I should add I could not find any references to this command in the documentation about alternative indexes.

This is Azure DevOps **Server** (the on-premises deployment, not cloud).

### Platform

Windows 11 x86_64

### Version

uv 0.9.17 (2b5d65e61 2025-12-09)

### Python version

Python 3.13.11

---

_Label `bug` added by @ubalklen on 2025-12-16 13:09_

---

_Comment by @konstin on 2025-12-16 13:47_

The solution is somewhat hidden in the log:

> DEBUG Skipping keyring fetch for https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/1f682db7-3995-4f32-afeb-fa23a36322da/pypi/download/debugpy/1.8.19/debugpy-1.8.19-cp313-cp313-win_amd64.whl without username; use `authenticate = always` to force

You can either set a username or use `authenticate = always`, see https://docs.astral.sh/uv/guides/integration/alternative-indexes/#authenticate-with-keyring-and-artifacts-keyring for more details. We're working on removing the username requirement.

---

_Label `bug` removed by @konstin on 2025-12-16 13:47_

---

_Label `question` added by @konstin on 2025-12-16 13:47_

---

_Comment by @ubalklen on 2025-12-16 16:59_

Thanks @konstin. It was indeed the case. I should have paid more attention to the documentation.

Unfortunately, that was not enough. I can see in the logs the user `VssSessionToken` is correctly attached, but authentication still fails:

```
DEBUG Checking text store for credentials for https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/MyOrgFeed/pypi/simple/pytest-recording/
DEBUG Checking keyring for credentials for index URL VssSessionToken@https://ads.intra.my-org.com/tfs/PRODUCTS/_packaging/MyOrgFeed/pypi/simple/
DEBUG Released lock at `C:\Users\username\AppData\Local\Temp\.tmp9bY31l\simple-v18\index\0a0b26c57806d6e9\pytest-recording.lock`
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
```

It turns out maybe `artifacts-keyring` does not support Azure DevOps *Server* after all. This was recently semi-confirmed [here](https://developercommunity.visualstudio.com/t/Is-artifacts-keyring-supposed-to-work/11009418?space=8&query=provisioning+issues&sort=newest) and [here](https://github.com/microsoft/artifacts-keyring/issues/102), with the suggestion that maybe `keyring` alone is supported. Their sister project's [official repo](https://github.com/microsoft/artifacts-credprovider#azure-devops-server) also has some information regarding Azure DevOps Server support:

> **Azure DevOps Server**
>
> The Azure Artifacts Credential Provider may not be necessary for an on-premises Azure DevOps Server on Windows. If the credential provider is needed, it cannot acquire credentials interactively, therefore, the VSS_NUGET_EXTERNAL_FEED_ENDPOINTS environment variable must be used as an alternative. Supply a [Personal Access Token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops) directly using the VSS_NUGET_EXTERNAL_FEED_ENDPOINTS [environment variable](https://github.com/microsoft/artifacts-credprovider#environment-variables).
>
> From Azure DevOps Server 2020 RC1 forward, the NuGet Authenticate task can be used in Pipelines.

Therefore, *maybe*, one can get away with `keyring` alone, and even so it would demand a PAT in some env variable. This is worse than the alternative methods that `uv` already supports, so I'm not taking that path.

Leaving it open in case maintainers think it's a good idea to add some documentation to warn users about the limitations of authentication in Azure DevOps Server environments.


---

_Label `question` removed by @konstin on 2025-12-16 17:00_

---

_Label `documentation` added by @konstin on 2025-12-16 17:00_

---

_Comment by @angelo-peronio on 2025-12-19 16:54_

My experiences until now with `uv`, `keyring`, and the artifacts feed of a on-premises Azure DevOps Server 2022.2 (AzureDevopsServer_20250527.1):

## `keyring` setup

1) Install just `keyring`, without `artifacts-keyring`:

    ```bash
    uv tool install keyring
    ````

2) In Azure DevOps Server, generate a Personal Access Token with scope Packaging/Read.
3) Store the token. In my experience the username does not need to be `VssSessionToken`, we use here `MYTOKEN`:

    ```bash
    keyring set MYSERVER.COM MYTOKEN
    ```

## The project to be published

In the `pyproject.toml` of the project I want to publish:

```toml
[tool.uv]
# Use the `keyring` CLI for credential lookup.
keyring-provider = "subprocess"

[[tool.uv.index]]
name = "MYFEED"
url = "https://MYTOKEN@MYSERVER.COM/MYORGANIZATION/MYPACKAGE/_packaging/MYFEED/pypi/simple/"
publish-url = "https://MYSERVER.COM/MYORGANIZATION/MYPACKAGE/_packaging/MYFEED/pypi/upload/"
# Force keyring authentication.
# https://docs.astral.sh/uv/concepts/indexes/#using-credential-providers
authenticate = "always"
# Do not stop if authentication fails.
ignore-error-codes = [401]
```

Azure pipelines use instead environment variables for authentication:

```yaml
variables:
  UV_KEYRING_PROVIDER: disabled # Do not use `keyring` for HTTP authentication.
  UV_NO_PROGRESS: true # Diable spinners and progress bars for CI logs.

[...]

      - pwsh: uv publish --index=MYFEED --verbose
        displayName: Publish
        env:
          UV_INDEX_MYFEED_USERNAME: MYTOKEN # Must be what has been specified in the index URL in pyproject.toml
          UV_INDEX_MYFEED_PASSWORD: $(System.AccessToken)
          UV_PUBLISH_TOKEN: $(System.AccessToken)
```

## A consuming project

In the `pyproject.toml`:

```toml
[project]
dependencies = ["MYPACKAGE"]

[tool.uv]
keyring-provider = "subprocess"

[[tool.uv.index]]
name = "MYFEED"
url = "https://POSSIBLYADIFFERENTTOKEN@MYSERVER.COM/MYORGANIZATION/MYPACKAGE/_packaging/MYFEED/pypi/simple/"
authenticate = "always"
```

## Inconvenience

In the `pyproject.toml` of the project to be published, I have specified `MYTOKEN` in the index URL. This is to select a specific Personal Access Token among the possibly many stored by `keyring` for `MYSERVER.COM`. The drawback is that this forces each developer to store a token under that same username.

---

_Comment by @zanieb on 2025-12-19 20:03_

You should be able to use `uv auth login myserver.com --token` instead? Then omit the username from the index URL?

---

_Comment by @tboddyspargo on 2025-12-30 22:30_

I'm experiencing this same inconvenience. I want to avoid giving my developers more commands steps to run.

> We're working on removing the username requirement.

This would be amazing! 

In my specific case, I would really just appreciate a way to specify `VssSessionToken` as the username in my `pyproject.toml` (either as a `username` field under `[[tool.uv.index]]` or within the existing `url` field) **AND** still be able to override it using `--token` in CI. Currently, I get `error: The username can't be set both in the publish URL and in the CLI`. I'd like the CLI to override the publish URL (or `username` field, if/when supported).

---

_Comment by @teshanshanuka on 2026-01-06 22:32_

I get a very similar error with a similar setup and would love to know a solution or a workaround. (I do not want to create a duplicate issue so I will post here.)

**Platform: EndeavourOS**
**Version: uv 0.9.22**
**Python version: 3.8.20**

`pyproject.toml`
```
[tool.uv.sources]
pvtpkg = { index = "pvtreg" }

[[tool.uv.index]]
name = "pvtreg"
url = "https://VssSessionToken@pkgs.dev.azure.com/myorg/_packaging/Myorg/pypi/simple/"
explicit = true
```

Output
```
$uv sync
...
Resolved 31 packages in 0.87ms
      Built mypkg @ file:///xxx/mypkg
  × Failed to download `pvtpkg==0.1.1`
  ├─▶ Failed to fetch: `https://pkgs.dev.azure.com/myorg/_packaging/<uuid>/pypi/download/pvtpkg/0.1.1/pvtpkg-0.1.1-py3-none-any.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/myorg/_packaging/<uuid>/pypi/download/pvtpkg/0.1.1/pvtpkg-0.1.1-py3-none-any.whl)
```

It is weird to me that it tries not the given url but a slightly different one with UUIDs. 

The token is set in keyring and works with `--extra-index-url`.

```
uv pip install pvtpkg==0.1.1 --extra-index-url="https://$(keyring get $PVTREG_URL VssSessionToken)@pkgs.dev.azure.com/..."
```


---

_Comment by @ubalklen on 2026-01-07 13:37_

> I get a very similar error with a similar setup and would love to know a solution or a workaround. (I do not want to create a duplicate issue so I will post here.)

Better open a new issue, as this one deals with Azure DevOps **Server** (no cloud).

---
