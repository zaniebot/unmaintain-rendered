```yaml
number: 16877
title: "Failed to install `jupyterlab-widgets==3.0.16` in Windows via symlink"
type: issue
state: closed
author: nathanjmcdougall
labels:
  - bug
  - error messages
  - windows
assignees: []
created_at: 2025-11-27T19:40:50Z
updated_at: 2025-12-01T20:02:36Z
url: https://github.com/astral-sh/uv/issues/16877
synced_at: 2026-01-10T03:23:55Z
```

# Failed to install `jupyterlab-widgets==3.0.16` in Windows via symlink

---

_Issue opened by @nathanjmcdougall on 2025-11-27 19:40_

### Summary

Running `uv add jupyterlab-widgets==3.0.16 --link-mode=symlink` fails on a fresh project on Windows when running without admin permissions.

### Reproduction

```
> uv init
> uv add jupyterlab-widgets==3.0.16 --link-mode=symlink
Resolved 2 packages in 32ms
error: Failed to install: jupyterlab_widgets-3.0.16-py3-none-any.whl (jupyterlab-widgets==3.0.16)
  Caused by: failed to symlink file from C:\Users\namc\AppData\Local\uv\cache\archive-v0\EswKn5fxGWjZvCVIQHCBP\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js to C:\Users\namc\Temp\pl\.venv\Lib\site-packages\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js: A required privilege is not held by the client. (os error 1314)
```

- I have Windows developer mode enabled to enable symlinks.
- I've tried `uv cache clean jupyterlab-widgets`.
- I've tried setting a different to the cache directory via `uv add jupyterlab-widgets==3.0.16 --link-mode=symlink --cache-dir="C:/uc"`, with the same issue.

### Other things that do work

- Running with admin permissions is successful.
- Installing the previous version of `jupyterlab-widgets==3.0.15` works fine.
- Using `--no-cache`, `--link-mode=copy` and `--link-mode=hardlink` are all successful.

### Considerations

The long relative path in the package `jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js` is a bit suspicious. Actually, there's an even longer one:  `jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\vendors-node_modules_d3-color_src_color_js-node_modules_d3-format_src_defaultLocale_js-node_m-09b215.2643c43f22ad111f4f82.js.map`.

`jupyterlab-widgets` is a required dependency of `ipywidgets`, and shares [the same git repo](https://github.com/jupyter-widgets/ipywidgets).

I am using symlinks as a workaround to #11134. 

### Platform

Windows 11 Enterprise 24H2

### Version

uv 0.9.13 (7ca92dcf6 2025-11-26)

### Python version

Python 3.13.2

---

_Label `bug` added by @nathanjmcdougall on 2025-11-27 19:40_

---

_Comment by @samypr100 on 2025-11-28 05:46_

os error 1314 seems related to missing `SeCreateSymbolicLinkPrivilege` which Admin has by default on windows.

The fact that it work before the "long path length" added in 3.0.16 is a good hint. This makes me think that the non-admin symlink support added starting in [Windows 10 Insiders build 14972](https://blogs.windows.com/windowsdeveloper/2016/12/02/symlinks-windows-10/) doesn't work for path lengths above 260 (e.g. without longpaths being enabled) or simply behaves differently after 260 is reached.

If possible, can you try giving your non-admin user temporary `SeCreateSymbolicLinkPrivilege`?

You can do so via launching `Edit Group Policy` (`gpedit.msc`) then under `Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment\Create symbolic links` --> Add your user and a reboot is likely needed. Then try re-running the uv command once again.

---

_Label `windows` added by @samypr100 on 2025-11-28 05:46_

---

_Comment by @nathanjmcdougall on 2025-11-28 20:35_

I don't have authorization to readily modify the group policy at my organization. However, I was able to reproduce the issue on my personal Windows 10 Home 21H1 installation (build 19043.1165).

I used `ntrights` to give myself the `SeCreateSymbolicLinkPrivilege` permission, restarted, and got past the permissions error. Instead I've got os error 87 - which I'd be tempted to pin down to the fact that this is a fairly old Windows build.

```
C:\Users\Nathan McDougall\Documents\Temp> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== ========
SeShutdownPrivilege           Shut down the system                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled
SeCreateSymbolicLinkPrivilege Create symbolic links                Disabled
```

```
PS C:\Users\Nathan McDougall\Documents\Temp> ~/.local/bin/uv add jupyterlab-widgets==3.0.16 --link-mode=symlink
Resolved 2 packages in 2.06s
error: Failed to install: jupyterlab_widgets-3.0.16-py3-none-any.whl (jupyterlab-widgets==3.0.16)
  Caused by: failed to symlink file from C:\Users\Nathan McDougall\AppData\Local\uv\cache\archive-v0\WE7CiwuqXvBGgXoKsJ0VW\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js to C:\Users\Nathan McDougall\Documents\Temp\.venv\Lib\site-packages\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js: The parameter is incorrect. (os error 87)
```



---

_Comment by @samypr100 on 2025-11-30 01:27_

@nathanjmcdougall Thanks for the details. If possible, could you try the uv build from [this PR](https://github.com/astral-sh/uv/actions/runs/19793348707?pr=16894) to see if it resolves the os error 87 issue (and possibly the os error 1314)?

You can download the binaries for example from [artifacts-x86_64-pc-windows-msvc](https://github.com/astral-sh/uv/actions/runs/19793348707/artifacts/4715445963) and use uv binary instead (assuming your windows system is x86_64).

---

_Comment by @nathanjmcdougall on 2025-11-30 08:04_

Unfortunately the os error 87 persists on the Windows 10 Home machine. I'll try the Windows 11 Enterprise machine next.

```Powershell
 C:\Users\Nathan McDougall\Documents\Temp> ~\Downloads\artifacts-x86_64-pc-windows-msvc\uv-x86_64-pc-windows-msvc\uv self version
uv 0.9.13 (b93a15297 2025-11-29)
```

```
C:\Users\Nathan McDougall\Documents\Temp> ~\Downloads\artifacts-x86_64-pc-windows-msvc\uv-x86_64-pc-windows-msvc\uv add jupyterlab-widgets==3.0.16 --link-mode=symlink
Resolved 2 packages in 103ms
Uninstalled 1 package in 19ms
error: Failed to install: jupyterlab_widgets-3.0.16-py3-none-any.whl (jupyterlab-widgets==3.0.16)
  Caused by: failed to symlink file from C:\Users\Nathan McDougall\AppData\Local\uv\cache\archive-v0\WE7CiwuqXvBGgXoKsJ0VW\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js to C:\Users\Nathan McDougall\Documents\Temp\.venv\Lib\site-packages\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js: The parameter is incorrect. (os error 87)
```

---

_Comment by @nathanjmcdougall on 2025-11-30 08:20_

On the Windows 11 Enterprise machine, getting the same os error 1314.

```
C:\Users\namc\Temp\pl> ~\Downloads\artifacts-x86_64-pc-windows-msvc\uv-x86_64-pc-windows-msvc\uv self version
uv 0.9.13 (17cd72c5d 2025-11-29)
```

(N.B. hash is different - I find that surprising).

```
C:\Users\namc\Temp\pl>  ~\Downloads\artifacts-x86_64-pc-windows-msvc\uv-x86_64-pc-windows-msvc\uv add jupyterlab-widgets==3.0.16 --link-mode=symlink
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 2 packages in 313ms
error: Failed to install: jupyterlab_widgets-3.0.16-py3-none-any.whl (jupyterlab-widgets==3.0.16)
  Caused by: failed to symlink file from C:\Users\namc\AppData\Local\uv\cache\archive-v0\EswKn5fxGWjZvCVIQHCBP\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js to C:\Users\namc\Temp\pl\.venv\Lib\site-packages\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js: A required privilege is not held by the client. (os error 1314)
```


---

_Comment by @samypr100 on 2025-11-30 13:58_

> (N.B. hash is different - I find that surprising).

Running it with `-v` will tell you what version it actually ran with.

`path\to\uv add jupyterlab-widgets==3.0.16 --link-mode=symlink -v` 

Its possible you may need to add the binaries to the PATH in some cases, although not sure of the specifics on the policies.

Commit b93a152979a9aae80ba2df52fdfb01f16fdb6ca5 would be the correct build.

---

_Comment by @samypr100 on 2025-11-30 19:18_

@nathanjmcdougall I was able to reproduce the errors on my windows 11 25H2 system and could confirm #16894 fixed the issues. I don't have an install as old as Windows 10 21H1 to confirm how far back the issue is fixed, but theoretically it should fix it starting from Windows 10 1903 assuming two things are enabled:
1. Developer Mode is enabled
2. ``HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem@LongPathsEnabled`` dword is set to ``1``.

---

_Comment by @nathanjmcdougall on 2025-11-30 20:37_

I can confirm the fix was successful on the Windows 11 24H2 machine, once I set `LongPathsEnabled`.

---

_Label `error messages` added by @konstin on 2025-12-01 08:14_

---

_Comment by @konstin on 2025-12-01 08:20_

The docs suggest that using the `\\?\` prefix works around the MAX_PATH limitation, if that works we should use that, if it doesn't, we should improve the error message to hint at enabling long paths.

---

_Comment by @samypr100 on 2025-12-01 12:45_

> The docs suggest that using the `\\?\` prefix works around the MAX_PATH limitation, if that works we should use that, if it doesn't, we should improve the error message to hint at enabling long paths.

Agreed on improving the error message as well, although enabling long paths doesn't work without the manifest changes.

---

_Closed by @zanieb on 2025-12-01 20:02_

---
