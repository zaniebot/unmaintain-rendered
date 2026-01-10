---
number: 2810
title: Random errors on accessing cache with simultaneous installs on Windows
type: issue
state: open
author: gozdal
labels:
  - windows
assignees: []
created_at: 2024-04-03T15:17:03Z
updated_at: 2025-05-23T08:50:18Z
url: https://github.com/astral-sh/uv/issues/2810
synced_at: 2026-01-10T01:23:22Z
---

# Random errors on accessing cache with simultaneous installs on Windows

---

_Issue opened by @gozdal on 2024-04-03 15:17_

Quite often `uv venv` or `uv pip install` fails in our CI environment. There are multiple jobs creating virtual envs and/or installing requirements with `uv pip install`. Each is using a different venv but they all share user and thus the cache.

Example (synthetic reproduction):

```bash
host="jenkins@sf-win19-vm4"
python="C:/Python3.12/python.exe"

venv=$WORKSPACE/venv

ssh "$host" "uv venv --seed --python $python $venv"
```

Running this snippet as a Jenkins job from 9 hosts to the same Windows hosts sometimes works, but more often than not it fails on some of the hosts. Example:

```
11:09:38 + ssh jenkins@sf-win19-vm4 'uv venv --seed --python C:/Python3.12/python.exe /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-precision9/venv'
11:09:38 Using Python 3.12.2 interpreter at: C:\Python3.12\python.exe
11:09:38 Creating virtualenv at: /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-precision9/venv
11:09:42 uv::venv::seed
11:09:42 
11:09:42   × Failed to install seed packages
11:09:42   ├─▶ No solution found when resolving: pip
11:09:42   ├─▶ Failed to write to the client cache
11:09:42   ╰─▶ Failed to persist temporary file to
11:09:42       AppData\Local\uv\cache\simple-v6\pypi\pip.rkyv: Access is denied. (os
11:09:42       error 5)
```

```
11:09:38 + ssh jenkins@sf-win19-vm4 'uv venv --seed --python C:/Python3.12/python.exe /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-precision4/venv'
11:09:38 Using Python 3.12.2 interpreter at: C:\Python3.12\python.exe
11:09:38 Creating virtualenv at: /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-precision4/venv
11:09:42 uv::venv::seed
11:09:42 
11:09:42   × Failed to install seed packages
11:09:42   ├─▶ No solution found when resolving: pip
11:09:42   ├─▶ Failed to write to the client cache
11:09:42   ╰─▶ Failed to persist temporary file to
11:09:42       AppData\Local\uv\cache\simple-v6\pypi\pip.rkyv: Access is denied. (os
11:09:42       error 5)
```

```
11:09:38 + ssh jenkins@sf-win19-vm4 'uv venv --seed --python C:/Python3.12/python.exe /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-worker3/venv'
11:09:38   × Failed to persist temporary file to
11:09:38   │ AppData\Local\uv\cache\interpreter-v0\55968f1457792c80.msgpack: Access is
11:09:38   │ denied. (os error 5)
```

```
11:09:38 + ssh jenkins@sf-win19-vm4 'uv venv --seed --python C:/Python3.12/python.exe /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-worker2/venv'
11:09:38 Using Python 3.12.2 interpreter at: C:\Python3.12\python.exe
11:09:38 Creating virtualenv at: /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-worker2/venv
11:09:42 uv::venv::seed
11:09:42 
11:09:42   × Failed to install seed packages
11:09:42   ├─▶ No solution found when resolving: pip
11:09:42   ├─▶ Failed to write to the client cache
11:09:42   ╰─▶ Failed to persist temporary file to
11:09:42       AppData\Local\uv\cache\simple-v6\pypi\pip.rkyv: Access is denied. (os
11:09:42       error 5)
```

```
1:09:38 + ssh jenkins@sf-win19-vm4 'uv venv --seed --python C:/Python3.12/python.exe /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-precision2/venv'
11:09:38 Using Python 3.12.2 interpreter at: C:\Python3.12\python.exe
11:09:38 Creating virtualenv at: /home/jenkins/workspace/gozdal/uv-repro-windows-cache-bug/node/sf-precision2/venv
11:09:42 uv::venv::seed
11:09:42 
11:09:42   × Failed to install seed packages
11:09:42   ├─▶ No solution found when resolving: pip
11:09:42   ├─▶ Failed to write to the client cache
11:09:42   ╰─▶ Failed to persist temporary file to
11:09:42       AppData\Local\uv\cache\simple-v6\pypi\pip.rkyv: Access is denied. (os
11:09:42       error 5)
```

`uv 0.1.28 (0046a0d59 2024-04-02)`

---

_Comment by @gozdal on 2024-04-03 15:24_

Example of an error in `uv pip install`:
```
[2024-04-03T12:12:32.983Z] Built 8 editables in 5.84s
[2024-04-03T12:12:41.046Z] Resolved 253 packages in 7.26s
[2024-04-03T12:12:41.046Z] error: Failed to download distributions
[2024-04-03T12:12:41.046Z]   Caused by: Failed to fetch wheel: python-swiftclient @ git+https://github.com/StarfishStorage/python-swiftclient.git@fbdac94cf75e2fb5d3cf20207d6883fc6bd280b4
[2024-04-03T12:12:41.046Z]   Caused by: Failed to write to the distribution cache
[2024-04-03T12:12:41.046Z]   Caused by: The process cannot access the file because another process has locked a portion of the file. (os error 33)
```

```
[2024-04-03T12:12:33.275Z] error: Failed to build editables
[2024-04-03T12:12:33.275Z]   Caused by: Failed to build editable: file:///C:/jenkins/pr_integration_windows_multi_pipeline-PR-13659-2/part-1/remote/repo/starfish/agent
[2024-04-03T12:12:33.275Z]   Caused by: Build backend failed to determine extra requires with `build_editable()` with exit code: 1
[2024-04-03T12:12:33.275Z] --- stdout:
[2024-04-03T12:12:33.275Z] 
[2024-04-03T12:12:33.275Z] --- stderr:
[2024-04-03T12:12:33.275Z] Traceback (most recent call last):
[2024-04-03T12:12:33.275Z]   File "<string>", line 14, in <module>
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\build_meta.py", line 448, in get_requires_for_build_editable
[2024-04-03T12:12:33.275Z]     return self.get_requires_for_build_wheel(config_settings)
[2024-04-03T12:12:33.275Z]            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\build_meta.py", line 325, in get_requires_for_build_wheel
[2024-04-03T12:12:33.275Z]     return self._get_build_requires(config_settings, requirements=['wheel'])
[2024-04-03T12:12:33.275Z]            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\build_meta.py", line 295, in _get_build_requires
[2024-04-03T12:12:33.275Z]     self.run_setup()
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\build_meta.py", line 311, in run_setup
[2024-04-03T12:12:33.275Z]     exec(code, locals())
[2024-04-03T12:12:33.275Z]   File "<string>", line 1, in <module>
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\__init__.py", line 104, in setup
[2024-04-03T12:12:33.275Z]     return distutils.core.setup(**attrs)
[2024-04-03T12:12:33.275Z]            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\_distutils\core.py", line 147, in setup
[2024-04-03T12:12:33.275Z]     _setup_distribution = dist = klass(attrs)
[2024-04-03T12:12:33.275Z]                                  ^^^^^^^^^^^^
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\dist.py", line 307, in __init__
[2024-04-03T12:12:33.275Z]     _Distribution.__init__(self, dist_attrs)
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\_distutils\dist.py", line 283, in __init__
[2024-04-03T12:12:33.275Z]     self.finalize_options()
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\dist.py", line 657, in finalize_options
[2024-04-03T12:12:33.275Z]     for ep in sorted(loaded, key=by_order):
[2024-04-03T12:12:33.275Z]               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
[2024-04-03T12:12:33.275Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools\dist.py", line 656, in <lambda>
[2024-04-03T12:12:33.275Z]     loaded = map(lambda e: e.load(), filtered)
[2024-04-03T12:12:33.275Z]                            ^^^^^^^^
[2024-04-03T12:12:33.275Z]   File "C:\Python\python.3.12.2\tools\Lib\importlib\metadata\__init__.py", line 205, in load
[2024-04-03T12:12:33.275Z]     module = import_module(match.group('module'))
[2024-04-03T12:12:33.275Z]              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
[2024-04-03T12:12:33.275Z]   File "C:\Python\python.3.12.2\tools\Lib\importlib\__init__.py", line 90, in import_module
[2024-04-03T12:12:33.275Z]     return _bootstrap._gcd_import(name[level:], package, level)
[2024-04-03T12:12:33.275Z]            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
[2024-04-03T12:12:33.275Z]   File "<frozen importlib._bootstrap>", line 1387, in _gcd_import
[2024-04-03T12:12:33.275Z]   File "<frozen importlib._bootstrap>", line 1360, in _find_and_load
[2024-04-03T12:12:33.275Z]   File "<frozen importlib._bootstrap>", line 1310, in _find_and_load_unlocked
[2024-04-03T12:12:33.275Z]   File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
[2024-04-03T12:12:33.275Z]   File "<frozen importlib._bootstrap>", line 1387, in _gcd_import
[2024-04-03T12:12:33.275Z]   File "<frozen importlib._bootstrap>", line 1360, in _find_and_load
[2024-04-03T12:12:33.275Z]   File "<frozen importlib._bootstrap>", line 1310, in _find_and_load_unlocked
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 1387, in _gcd_import
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 1360, in _find_and_load
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 1331, in _find_and_load_unlocked
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 935, in _load_unlocked
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap_external>", line 995, in exec_module
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
[2024-04-03T12:12:33.276Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools_scm\__init__.py", line 7, in <module>
[2024-04-03T12:12:33.276Z]     from ._config import Configuration
[2024-04-03T12:12:33.276Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools_scm\_config.py", line 15, in <module>
[2024-04-03T12:12:33.276Z]     from ._integration.pyproject_reading import (
[2024-04-03T12:12:33.276Z]   File "C:\Users\jenkins\AppData\Local\uv\cache\.tmp37BXNX\.venv\Lib\site-packages\setuptools_scm\_integration\pyproject_reading.py", line 8, in <module>
[2024-04-03T12:12:33.276Z]     from .setuptools import read_dist_name_from_setup_cfg
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 1360, in _find_and_load
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 1331, in _find_and_load_unlocked
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap>", line 935, in _load_unlocked
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap_external>", line 991, in exec_module
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap_external>", line 1128, in get_code
[2024-04-03T12:12:33.276Z]   File "<frozen importlib._bootstrap_external>", line 1186, in get_data
[2024-04-03T12:12:33.276Z] PermissionError: [Errno 13] Permission denied: 'C:\\Users\\jenkins\\AppData\\Local\\uv\\cache\\.tmp37BXNX\\.venv\\Lib\\site-packages\\setuptools_scm\\_integration\\setuptools.py'
```

```
[2024-04-03T12:12:22.667Z] Installing sf-packaging requirements from C:\jenkins\pr_integration_windows_multi_pipeline-PR-13659-2\part-3\remote\repo\sf-packaging\requirements.txt
[2024-04-03T12:12:22.668Z] error: Failed to download and build: argparse-manpager @ git+https://github.com/StarfishStorage/argparse-manpager.git@e5b8176cbea4a48cdcad5390335a8a9464cd3537
[2024-04-03T12:12:22.668Z]   Caused by: Failed to write to the distribution cache
[2024-04-03T12:12:22.668Z]   Caused by: The process cannot access the file because another process has locked a portion of the file. (os error 33)
[2024-04-03T12:12:22.668Z] UpdatePackagingRequirements : uv pip install failed for C:\jenkins\pr_integrati
[2024-04-03T12:12:22.668Z] on_windows_multi_pipeline-PR-13659-2\part-3\remote\repo\sf-packaging\requiremen
[2024-04-03T12:12:22.668Z] ts.txt
```

---

_Comment by @charliermarsh on 2024-04-03 15:27_

The second issue is already fixed on main (#2800). The first issue I'm not really sure but your system seems to be rejecting moving files out of the temporary directory.

---

_Comment by @gozdal on 2024-04-03 15:32_

Thanks for such a quick reply!
The thing is it's not deterministic - sometimes it works, sometimes it doesn't. 
Do you have any tips on how to debug?

---

_Label `windows` added by @charliermarsh on 2024-04-07 00:30_

---

_Comment by @pfmoore on 2024-04-16 11:02_

"The process cannot access the file because another process has locked a portion of the file" sounds like another tool is reading the file while it's being moved. Maybe some sort of virus checker?

---

_Comment by @gozdal on 2024-04-16 11:03_

There is no AV software on those machines.

---

_Referenced in [astral-sh/uv#3280](../../astral-sh/uv/issues/3280.md) on 2024-04-26 16:07_

---

_Comment by @nickhutchinson on 2024-04-27 15:37_

The `ERROR_ACCESS_DENIED` 'Failed to persist temporary file' errors are straightforward to reproduce -- try deleting `%LOCALAPPDATA%\uv\cache` and then running, say, five `uv venv --seed <my-venv-dir>` commands in parallel, where `<my-venv-dir>` is unique across all the invocations.

Looking at a [Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) trace with uv 0.1.39, it looks that these concurrent uv.exe processes are racing to write to `wheel.rykv`/`pip.rykv`/`setuptools.rykv` in `%LOCALAPPDATA%\uv\cache\simple-v7\pypi`. Specifically, the uv processes are writing to a temporary file that is then renamed to `wheel.rykv`/`pip.rykv`/`setuptools.rykv`. However, these rename operations fail with `ERROR_ACCESS_DENIED` if any process (i.e. another of the concurrent uv processes) has an open handle to the file being replaced.

(Replacing an open file _can_ actually be done on recent Windows 10, but is a bit tricky. I believe all opens need to have used the `FILE_SHARE_DELETE` share flag, and the rename operation needs to be done with an API such as `SetFileInformationByHandle` with `SetFileRenameInfoEx` and the `FILE_RENAME_FLAG_POSIX_SEMANTICS` flag. `SetFileRenameInfoEx` is only available for Windows 10 1607 and newer.)

Process Monitor screenshot showing the failed rename op:

<img width="1487" alt="Screenshot 2024-04-27 at 15 24 06" src="https://github.com/astral-sh/uv/assets/755692/969536aa-a432-4614-b786-5a29f6239679">


---

_Comment by @charliermarsh on 2024-04-27 15:50_

Thanks, that's really helpful. Not sure how to resolve that then, other than to make Windows 10 1607 our minimum-supported version and implement POSIX semantics there.

---

_Comment by @nickhutchinson on 2024-04-30 19:21_

> Thanks, that's really helpful. Not sure how to resolve that then, other than to make Windows 10 1607 our minimum-supported version and implement POSIX semantics there.

If you've got a hard requirement on having POSIX-style file renames for uv, then yeah, I guess so. Otherwise, perhaps you could use some form of file-level locking to avoid having multiple uv processes race to write to the same path in the cache.

---

_Referenced in [meltano/meltano#8539](../../meltano/meltano/issues/8539.md) on 2024-05-13 16:42_

---

_Comment by @george-palmsens on 2024-08-28 08:20_

Would love to see this remedied - `uv` speeds up our CI by a huge amount but this issue has forced us to revert. Glad the root cause seems to be understood!

---

_Referenced in [astral-sh/uv#9524](../../astral-sh/uv/pulls/9524.md) on 2024-11-29 15:27_

---

_Comment by @codeLemur on 2024-12-24 10:12_

I had a similar problem with uv , GitLab and a Windows build server.
Parallel builds caused conflicts with the uv cache. The cache was stored for each build in the same directory of the user:
`Failed to retrieve temporary file while trying to persist to C:\Users\GitLabUser\AppData\Local\uv\cache\simple-v14\pypi\docutils.rkyv`

What solved the issue was to set a specific cache directory for each build:
`--cache-dir .cache`

With this solution each parallel build uses its own cache.

---

_Comment by @dinhphieu on 2025-01-19 09:15_

> I had a similar problem with uv , GitLab and a Windows build server. Parallel builds caused conflicts with the uv cache. The cache was stored for each build in the same directory of the user: `Failed to retrieve temporary file while trying to persist to C:\Users\GitLabUser\AppData\Local\uv\cache\simple-v14\pypi\docutils.rkyv`
> 
> What solved the issue was to set a specific cache directory for each build: `--cache-dir .cache`
> 
> With this solution each parallel build uses its own cache.

Thanks! Recently hit this while using uvx with Claude's MCP mcp-server-sqlite

If anyone is in the same boat as me, this is the config

```
{
  "mcpServers": {
    "sqlite": {
      "command": "uvx",
      "args": [
        "--cache-dir",
        ".cache",
        "mcp-server-sqlite",
        "--db-path",
        "/path/to/sqlite.db"
      ]
    }
  }
}
```

---

_Comment by @skaasjager on 2025-05-23 08:00_

Just wanted to let people know that I hit the issue of `Caused by: Failed to persist temporary file to ...` as well during a build using `cmake` and `uv`. I read about AV being the issue and some caching things, but I had another solution that worked for me.

In my case, it seems to have hit the [Maximum Path Length Limitation](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation), because after I moved the repository to a short-named folder in the root of the drive (e.g. C:\Code), I was able to run the build successfully.

Perhaps this helps others as well.

---
