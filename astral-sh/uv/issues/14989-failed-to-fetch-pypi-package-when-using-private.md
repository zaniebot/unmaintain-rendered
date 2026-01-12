```yaml
number: 14989
title: "Failed to fetch pypi package when using private indexes when the package isn't found on the private indexer (404)"
type: issue
state: closed
author: WilliamStam
labels:
  - bug
assignees: []
created_at: 2025-07-31T09:42:42Z
updated_at: 2025-07-31T21:00:03Z
url: https://github.com/astral-sh/uv/issues/14989
synced_at: 2026-01-12T16:02:01Z
```

# Failed to fetch pypi package when using private indexes when the package isn't found on the private indexer (404)

---

_@WilliamStam_

### Summary

when using custom indexers the last while uv has been getting stuck on failed to fetch with a 404 

it feels like the 0.6 branch didn't have this issue but in 0.7 it got worse and now in 0.8 i cant uv sync at all

the pypi package repo is a gitea repo that throws a 404 if the package cant be found

the docs say 

> uv will always continue searching across indexes when it encounters a 404 Not Found. This cannot be overridden.

but this isnt the case. its actually failing and stopping when it finds a 404

tried deleting the uv.lock and the .venv and uv sync --upgrade errors out imediately with the first package and a 404

-----------

pyproject.toml
```
[project]
name = "leproject"
version = "0.2.16"
description = "API"
authors = [
    
]
requires-python = ">=3.12"
readme = "README.md"
license = {text = "MIT"}
dependencies = [
    # these use a custom indexer - removed some for brevity
    "Custodian-Database==1.0.*",
    "Hydra-Database==1.0.*",
    

    # these use pypi - removed some for brevity
    "aiofiles==24.1.*",
    "asyncmy==0.2.*",
    "coloredlogs==15.0.*",
    "cryptography==44.0.*",
    "email-validator==2.2.*",
  ...
]

[tool.uv]
package = false


[[tool.uv.index]]
name = "database"
url = "https://pypi@example.com:password@git.example.com/api/packages/Database/pypi/simple"
[[tool.uv.index]]
name = "utilities"
url = "https://pypi@example.com:password@git.example.com/api/packages/Utilities/pypi/simple"
[[tool.uv.index]]
name = "reports"
url = "https://pypi@example.com:password@git.example.com/api/packages/Reports/pypi/simple"

```

```
(venv) W:\Projects\API>uv sync --upgrade
⠼ aiofiles==24.1.0                                                                                                                                                                                                                                                  error: Request failed after 1 retries
  Caused by: Failed to fetch: `https://git.example.com/api/packages/Reports/pypi/simple/coloredlogs/`
  Caused by: HTTP status client error (404 Not Found) for url (https://git.example.com/api/packages/Reports/pypi/simple/coloredlogs/)

(venv) W:\Projects\API>uv sync --upgrade
⠼ app-utilities==1.0.39                                                                                                                                                                                                                                             error: Request failed after 1 retries
  Caused by: Failed to fetch: `https://git.4gl.co.za/api/packages/Reports/pypi/simple/wand/`
  Caused by: HTTP status client error (404 Not Found) for url (https://git.example.com/api/packages/Reports/pypi/simple/wand/)

(venv) W:\Projects\API>uv sync --upgrade                                                                                                                                                                                                      
⠸ asyncmy==0.2.16                                                                                                                                                                                                                                         error: Request failed after 1 retries
  Caused by: Failed to fetch: `https://git.example.com/api/packages/Reports/pypi/simple/sqlalchemy/`
  Caused by: HTTP status client error (404 Not Found) for url (https://git.example.com/api/packages/Reports/pypi/simple/sqlalchemy/)

```



### Platform

windows 11

### Version

uv 0.8.4 (e176e1714 2025-07-30)

### Python version

Python 3.13.3

---

_Label `bug` added by @WilliamStam on 2025-07-31 09:42_

---

_Renamed from "Failed to fetch pypi package when using private indexes as well" to "Failed to fetch pypi package when using private indexes when the package isn't found on the private indexer (404)" by @WilliamStam on 2025-07-31 09:43_

---

_Comment by @WilliamStam on 2025-07-31 09:46_

then just randomly.. it works.. all i did was up arrow enter a few times. 

something is definitely fishy

---

_Comment by @zanieb on 2025-07-31 11:38_

This sounds more like a problem with your index than uv? Can you share verbose logs? What kind of index are you using?

---

_Label `needs-mre` added by @zanieb on 2025-07-31 11:38_

---

_Comment by @WilliamStam on 2025-07-31 14:20_

the indexer returns a 404 when it doesnt find the package. (gitea) im not sure how else its supposed to react? 


```
(api-munsoft-co-za) W:\Projects\Sentinel\API>uv sync --upgrade --verbose                                                                                                                                                                                            
DEBUG uv 0.8.4 (e176e1714 2025-07-30)
DEBUG Found project root: `W:\Projects\Sentinel\API`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `W:\Projects\Sentinel\API`
DEBUG No Python version file found in workspace: W:\Projects\Sentinel\API
DEBUG Using Python request `>=3.12` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.12`
DEBUG Released lock at `C:\Users\William\AppData\Local\Temp\uv-5829bfea898f7e5b.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to `--upgrade`
DEBUG Found static `pyproject.toml` for: api-munsoft-co-za @ file:///W:/Projects/Sentinel/API
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: api-munsoft-co-za*
DEBUG Searching for a compatible version of api-munsoft-co-za @ file:///W:/Projects/Sentinel/API (*)
DEBUG Adding direct dependency: aiofiles>=24.1.dev0, <24.2.dev0
DEBUG Adding direct dependency: app-utilities>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: asyncmy>=0.2.dev0, <0.3.dev0
DEBUG Adding direct dependency: cellcraft>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: coloredlogs>=15.0.dev0, <15.1.dev0
DEBUG Adding direct dependency: cryptography>=45.0.dev0, <45.1.dev0
DEBUG Adding direct dependency: custodian-database>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: dataframe-utilities>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: email-validator>=2.2.dev0, <2.3.dev0
DEBUG Adding direct dependency: fastapi>=0.116.dev0, <0.117.dev0
DEBUG Adding direct dependency: greenlet>=3.2.dev0, <3.3.dev0
DEBUG Adding direct dependency: httpx>=0.27.dev0, <0.28.dev0
DEBUG Adding direct dependency: hydra-database>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: jinja2>=3.1.dev0, <3.2.dev0
DEBUG Adding direct dependency: jsonpickle>=4.0.dev0, <4.1.dev0
DEBUG Adding direct dependency: munsoft-database>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: nest-asyncio>=1.6.dev0, <1.7.dev0
DEBUG Adding direct dependency: oracledb>=3.3.dev0, <3.4.dev0
DEBUG Adding direct dependency: origin-database>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: passlib>=1.7.dev0, <1.8.dev0
DEBUG Adding direct dependency: polars-lts-cpu>=1.31.dev0, <1.32.dev0
DEBUG Adding direct dependency: prometheus-fastapi-instrumentator>=7.1.dev0, <7.2.dev0
DEBUG Adding direct dependency: psutil>=6.1.dev0, <6.2.dev0
DEBUG Adding direct dependency: pydantic>=2.10.dev0, <2.11.dev0
DEBUG Adding direct dependency: python-dotenv>=1.1.dev0, <1.2.dev0
DEBUG Adding direct dependency: python-multipart>=0.0.dev0, <0.1.dev0
DEBUG Adding direct dependency: pyyaml>=6.0.dev0, <6.1.dev0
DEBUG Adding direct dependency: readings-database>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: requests>=2.32.dev0, <2.33.dev0
DEBUG Adding direct dependency: schedules-report>=1.0.dev0, <1.1.dev0
DEBUG Adding direct dependency: sqlalchemy>=2.0.dev0, <2.1.dev0
DEBUG Adding direct dependency: toml>=0.10.dev0, <0.11.dev0
DEBUG Adding direct dependency: uvicorn>=0.19.dev0, <0.20.dev0
DEBUG Adding direct dependency: wand>=0.6.dev0, <0.7.dev0
DEBUG Adding direct dependency: xlsxwriter>=3.2.dev0, <3.3.dev0
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\aiofiles.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\app-utilities.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\asyncmy.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\cellcraft.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\coloredlogs.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\cryptography.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\custodian-database.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\dataframe-utilities.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\email-validator.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\fastapi.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\greenlet.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\httpx.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\hydra-database.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\jinja2.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\jsonpickle.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\munsoft-database.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\nest-asyncio.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\oracledb.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\origin-database.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\passlib.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\polars-lts-cpu.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\prometheus-fastapi-instrumentator.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\psutil.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\pydantic.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\python-dotenv.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\python-multipart.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\pyyaml.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\readings-database.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\requests.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\schedules-report.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\sqlalchemy.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\toml.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\uvicorn.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\wand.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\xlsxwriter.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/aiofiles/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/app-utilities/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/asyncmy/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/cellcraft/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/cryptography/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/coloredlogs/
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Database/pypi/simple/custodian-database/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Database/pypi/simple/custodian-database/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/dataframe-utilities/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/greenlet/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/fastapi/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/email-validator/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/httpx/
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Database/pypi/simple/hydra-database/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Database/pypi/simple/hydra-database/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/jinja2/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/jsonpickle/
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Database/pypi/simple/munsoft-database/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Database/pypi/simple/munsoft-database/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/nest-asyncio/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/oracledb/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/polars-lts-cpu/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/passlib/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/psutil/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/python-dotenv/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/pydantic/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/prometheus-fastapi-instrumentator/
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Database/pypi/simple/origin-database/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Database/pypi/simple/origin-database/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/python-multipart/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/requests/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/schedules-report/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/sqlalchemy/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/pyyaml/
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Database/pypi/simple/readings-database/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Database/pypi/simple/readings-database/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/toml/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/uvicorn/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/wand/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Database/pypi/simple/xlsxwriter/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\jsonpickle.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\jsonpickle.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/jsonpickle/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\prometheus-fastapi-instrumentator.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\prometheus-fastapi-instrumentator.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\coloredlogs.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\coloredlogs.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\passlib.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\passlib.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/prometheus-fastapi-instrumentator/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/coloredlogs/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/passlib/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\greenlet.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\greenlet.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\cellcraft.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\cellcraft.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\oracledb.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\oracledb.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/greenlet/
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/cellcraft/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/cellcraft/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/oracledb/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\polars-lts-cpu.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\polars-lts-cpu.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\aiofiles.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\aiofiles.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\wand.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\wand.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\schedules-report.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/polars-lts-cpu/
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\schedules-report.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/wand/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/aiofiles/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/schedules-report/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\psutil.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\psutil.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\toml.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\toml.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\app-utilities.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\app-utilities.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\uvicorn.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\uvicorn.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/psutil/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/toml/
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/app-utilities/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/app-utilities/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/uvicorn/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\python-multipart.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\python-multipart.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/python-multipart/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\pyyaml.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\pyyaml.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\nest-asyncio.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\nest-asyncio.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\fastapi.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\fastapi.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\sqlalchemy.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\sqlalchemy.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/pyyaml/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/nest-asyncio/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/fastapi/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/sqlalchemy/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\cryptography.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\cryptography.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/cryptography/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\python-dotenv.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\python-dotenv.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\pydantic.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\pydantic.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/python-dotenv/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/pydantic/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\requests.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\requests.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/requests/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\email-validator.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\email-validator.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/email-validator/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\jsonpickle.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\jinja2.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\jsonpickle.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\jinja2.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\passlib.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\passlib.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\oracledb.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\oracledb.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\xlsxwriter.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\xlsxwriter.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\greenlet.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\greenlet.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\httpx.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\httpx.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\asyncmy.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\asyncmy.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\uvicorn.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\uvicorn.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\dataframe-utilities.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\dataframe-utilities.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\prometheus-fastapi-instrumentator.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/jsonpickle/
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\prometheus-fastapi-instrumentator.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/jinja2/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/greenlet/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/httpx/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/passlib/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/xlsxwriter/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/asyncmy/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/uvicorn/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/oracledb/
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/dataframe-utilities/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/dataframe-utilities/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/prometheus-fastapi-instrumentator/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\cryptography.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\cryptography.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/cryptography/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\coloredlogs.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\coloredlogs.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\python-multipart.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\python-multipart.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\fastapi.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\psutil.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\fastapi.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\sqlalchemy.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\psutil.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\pydantic.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\sqlalchemy.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\pydantic.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/coloredlogs/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/fastapi/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/python-multipart/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/psutil/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/pydantic/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/sqlalchemy/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\aiofiles.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\aiofiles.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\polars-lts-cpu.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\polars-lts-cpu.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\wand.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\wand.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\email-validator.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\email-validator.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\toml.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\nest-asyncio.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\toml.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\nest-asyncio.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\pyyaml.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\python-dotenv.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\pyyaml.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/aiofiles/
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\python-dotenv.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/polars-lts-cpu/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/wand/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/email-validator/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/toml/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/pyyaml/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/python-dotenv/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/nest-asyncio/
DEBUG Found modified response for: https://git.4gl.co.za/api/packages/Database/pypi/simple/hydra-database/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\schedules-report.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\schedules-report.lock`
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/schedules-report/
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/schedules-report/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\hydra-database.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\wheels-v5\index\4b8049a9d39d0c25\hydra-database\hydra_database-1.0.3-py3-none-any.lock`
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Database/pypi/files/hydra-database/1.0.3/Hydra_Database-1.0.3-py3-none-any.whl
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Database/pypi/files/hydra-database/1.0.3/Hydra_Database-1.0.3-py3-none-any.whl
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/pydantic/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/sqlalchemy/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/aiofiles/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/python-dotenv/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/polars-lts-cpu/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/wand/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/pyyaml/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/nest-asyncio/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/schedules-report/
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Database/pypi/files/hydra-database/1.0.3/Hydra_Database-1.0.3-py3-none-any.whl
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/email-validator/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\requests.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\requests.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/requests/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\xlsxwriter.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\xlsxwriter.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\jsonpickle.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\jsonpickle.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/xlsxwriter/
DEBUG Found stale response for: https://pypi.org/simple/jsonpickle/
DEBUG Sending revalidation request for: https://pypi.org/simple/jsonpickle/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\uvicorn.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\uvicorn.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\greenlet.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\greenlet.lock`
DEBUG Found stale response for: https://pypi.org/simple/uvicorn/
DEBUG Sending revalidation request for: https://pypi.org/simple/uvicorn/
DEBUG Found stale response for: https://pypi.org/simple/greenlet/
DEBUG Sending revalidation request for: https://pypi.org/simple/greenlet/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\oracledb.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\oracledb.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\httpx.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\httpx.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\jinja2.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\jinja2.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/httpx/
DEBUG Found stale response for: https://pypi.org/simple/oracledb/
DEBUG Sending revalidation request for: https://pypi.org/simple/oracledb/
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/jinja2/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\cryptography.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\cryptography.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\asyncmy.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\asyncmy.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\psutil.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\psutil.lock`
DEBUG No cache entry for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/asyncmy/
DEBUG Found stale response for: https://pypi.org/simple/psutil/
DEBUG Sending revalidation request for: https://pypi.org/simple/psutil/
DEBUG Found stale response for: https://pypi.org/simple/cryptography/
DEBUG Sending revalidation request for: https://pypi.org/simple/cryptography/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\toml.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\toml.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\prometheus-fastapi-instrumentator.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\prometheus-fastapi-instrumentator.lock`
DEBUG Found stale response for: https://pypi.org/simple/toml/
DEBUG Sending revalidation request for: https://pypi.org/simple/toml/
DEBUG Found stale response for: https://pypi.org/simple/prometheus-fastapi-instrumentator/
DEBUG Sending revalidation request for: https://pypi.org/simple/prometheus-fastapi-instrumentator/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\passlib.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\passlib.lock`
DEBUG Found stale response for: https://pypi.org/simple/passlib/
DEBUG Sending revalidation request for: https://pypi.org/simple/passlib/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\python-multipart.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\python-multipart.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\coloredlogs.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\coloredlogs.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\fastapi.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\fastapi.lock`
DEBUG Found stale response for: https://pypi.org/simple/python-multipart/
DEBUG Sending revalidation request for: https://pypi.org/simple/python-multipart/
DEBUG Found stale response for: https://pypi.org/simple/coloredlogs/
DEBUG Sending revalidation request for: https://pypi.org/simple/coloredlogs/
DEBUG Found stale response for: https://pypi.org/simple/fastapi/
DEBUG Sending revalidation request for: https://pypi.org/simple/fastapi/
DEBUG Found modified response for: https://git.4gl.co.za/api/packages/Database/pypi/simple/readings-database/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\readings-database.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\wheels-v5\index\4b8049a9d39d0c25\readings-database\readings_database-1.0.9-py3-none-any.lock`
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Database/pypi/files/readings-database/1.0.9/readings_database-1.0.9-py3-none-any.whl
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Database/pypi/files/readings-database/1.0.9/readings_database-1.0.9-py3-none-any.whl
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\xlsxwriter.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\xlsxwriter.lock`
DEBUG Found stale response for: https://pypi.org/simple/xlsxwriter/
DEBUG Sending revalidation request for: https://pypi.org/simple/xlsxwriter/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\requests.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\requests.lock`
DEBUG Found stale response for: https://pypi.org/simple/requests/
DEBUG Sending revalidation request for: https://pypi.org/simple/requests/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\jinja2.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\jinja2.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\httpx.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\httpx.lock`
DEBUG Found stale response for: https://pypi.org/simple/jinja2/
DEBUG Sending revalidation request for: https://pypi.org/simple/jinja2/
DEBUG Found stale response for: https://pypi.org/simple/httpx/
DEBUG Sending revalidation request for: https://pypi.org/simple/httpx/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\asyncmy.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\asyncmy.lock`
DEBUG Found stale response for: https://pypi.org/simple/asyncmy/
DEBUG Sending revalidation request for: https://pypi.org/simple/asyncmy/
DEBUG Found modified response for: https://git.4gl.co.za/api/packages/Database/pypi/simple/origin-database/
DEBUG Found modified response for: https://git.4gl.co.za/api/packages/Utilities/pypi/simple/cellcraft/
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\origin-database.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\wheels-v5\index\4b8049a9d39d0c25\origin-database\origin_database-1.0.12-py3-none-any.lock`
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Database/pypi/files/origin-database/1.0.12/origin_database-1.0.12-py3-none-any.whl
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Database/pypi/files/origin-database/1.0.12/origin_database-1.0.12-py3-none-any.whl
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\cellcraft.lock`
DEBUG Acquired lock for `C:\Users\William\AppData\Local\uv\cache\wheels-v5\index\7f0971d3bc21ddd2\cellcraft\cellcraft-1.0.19-py3-none-any.lock`
DEBUG Found stale response for: https://git.4gl.co.za/api/packages/Utilities/pypi/files/cellcraft/1.0.19/cellcraft-1.0.19-py3-none-any.whl
DEBUG Sending revalidation request for: https://git.4gl.co.za/api/packages/Utilities/pypi/files/cellcraft/1.0.19/cellcraft-1.0.19-py3-none-any.whl
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\sqlalchemy.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\wheels-v5\index\7f0971d3bc21ddd2\cellcraft\cellcraft-1.0.19-py3-none-any.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\wheels-v5\index\4b8049a9d39d0c25\origin-database\origin_database-1.0.12-py3-none-any.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\asyncmy.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\httpx.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\jinja2.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\requests.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\xlsxwriter.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\wheels-v5\index\4b8049a9d39d0c25\readings-database\readings_database-1.0.9-py3-none-any.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\wheels-v5\index\4b8049a9d39d0c25\hydra-database\hydra_database-1.0.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\fastapi.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\coloredlogs.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\python-multipart.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\passlib.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\prometheus-fastapi-instrumentator.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\toml.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\cryptography.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\psutil.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\oracledb.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\uvicorn.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\greenlet.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\pypi\jsonpickle.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\schedules-report.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\email-validator.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\nest-asyncio.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\pyyaml.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\wand.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\polars-lts-cpu.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\python-dotenv.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\aiofiles.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\a61b33ed6ad27263\pydantic.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\dataframe-utilities.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\7f0971d3bc21ddd2\app-utilities.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\munsoft-database.lock`
DEBUG Released lock at `C:\Users\William\AppData\Local\uv\cache\simple-v16\index\4b8049a9d39d0c25\custodian-database.lock`
DEBUG Released lock at `W:\Projects\Sentinel\API\.venv\.lock`
error: Request failed after 1 retries
  Caused by: Failed to fetch: `https://git.4gl.co.za/api/packages/Reports/pypi/simple/sqlalchemy/`
  Caused by: HTTP status client error (404 Not Found) for url (https://git.4gl.co.za/api/packages/Reports/pypi/simple/sqlalchemy/)
```

what i dont understand is that it "sometimes" works fine like even in this log it says that some 

```
DEBUG Transient request failure for: https://git.4gl.co.za/api/packages/Reports/pypi/simple/sqlalchemy/
```

but then errors out with 
```
DEBUG Released lock at `W:\Projects\Sentinel\API\.venv\.lock`
error: Request failed after 1 retries
  Caused by: Failed to fetch: `https://git.4gl.co.za/api/packages/Reports/pypi/simple/sqlalchemy/`
  Caused by: HTTP status client error (404 Not Found) for url (https://git.4gl.co.za/api/packages/Reports/pypi/simple/sqlalchemy/)
```

i can confirm that visiting that url does indead return a 404 (first it asks for username/password that are captured in the indexer's url)

<img width="558" height="279" alt="Image" src="https://github.com/user-attachments/assets/46f13a8b-3775-4bb3-b71d-2ff643504089" />

```
[[tool.uv.index]]
name = "database"
url = "https://USERNAME:PASSWORD@git.4gl.co.za/api/packages/Database/pypi/simple"
```



---

_Comment by @zanieb on 2025-07-31 14:24_

This sounds like the same bug as https://github.com/astral-sh/uv/issues/14941

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-31 14:29_

---

_Label `needs-mre` removed by @charliermarsh on 2025-07-31 14:29_

---

_Closed by @charliermarsh on 2025-07-31 21:00_

---
