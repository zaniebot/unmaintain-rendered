```yaml
number: 16894
title: Add a Windows manifest to uv binaries
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: uv-manifest
created_at: 2025-11-30T00:55:48Z
updated_at: 2025-12-01T20:57:50Z
url: https://github.com/astral-sh/uv/pull/16894
synced_at: 2026-01-10T05:49:14Z
```

# Add a Windows manifest to uv binaries

---

_Pull request opened by @samypr100 on 2025-11-30 00:55_

## Summary

Currently we do not include a Windows manifest on the uv binary for windows builds. This can cause problems such as the one in https://github.com/astral-sh/uv/issues/16877 which can limit what uv can do for some Windows operations (e.g. symlinks) that can have restrictions imposed by the OS unbeknownst to us and make it none obvious to isolate the issue.

Given we already do this for the `uv-trampoline`, we should also do it for uv. In the case of uv, I opted for explicit entries in the manifest rather than using the defaults embed_manifest crate provides which are not appropriate in all general cases.

The manifest now includes declarations for:
* Explicit "system" codepage declaration to retain backwards compat with previous uv releases. We should move to utf-8 codepage in the future to align with `uv-trampoline`, but it's arguably a breaking change in rare cases. We shouldn't have issues with using utf-8 as we don't really rely on *A calls to begin with.
* Explicit Windows 10+ support to ensure the executables are not treated as a legacy, preventing application compatibility layers being wrongly applied to it all the way back to NT 6.0 (Windows Vista). Note, other Windows compatibility entries do not imply support, rather they imply awareness as a preventive measure.
* Long Path support to avoid Windows operations assuming [MAX_PATH](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation) applies. This still requires the system to have long paths enabled via ``HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem@LongPathsEnabled`` dword being set to ``1`` (see [ref](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation#registry-setting-to-enable-long-paths)).
* Standard invoker execution levels for CLI applications to disable UAC virtualization after including the manifest.

The resulting manifest is the following

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" 
    xmlns:asmv3="urn:schemas-microsoft-com:asm.v3" manifestVersion="1.0">
    <assemblyIdentity name="uv" type="win32" version="0.9.13.0"></assemblyIdentity>
    <asmv3:trustInfo>
        <asmv3:security>
            <asmv3:requestedPrivileges>
                <asmv3:requestedExecutionLevel level="asInvoker" uiAccess="false"></asmv3:requestedExecutionLevel>
            </asmv3:requestedPrivileges>
        </asmv3:security>
    </asmv3:trustInfo>
    <asmv3:application>
        <asmv3:windowsSettings>
            <longPathAware xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">true</longPathAware>
        </asmv3:windowsSettings>
    </asmv3:application>
    <ms_compatibility:compatibility xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" 
        xmlns="urn:schemas-microsoft-com:compatibility.v1">
        <ms_compatibility:application xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1">
            <ms_compatibility:supportedOS xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" Id="{35138b9a-5d96-4fbd-8e2d-a2440225f93a}"></ms_compatibility:supportedOS>
            <ms_compatibility:supportedOS xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" Id="{4a2f28e3-53b9-4441-ba9c-d69d4a4a6e38}"></ms_compatibility:supportedOS>
            <ms_compatibility:supportedOS xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" Id="{1f676c76-80e1-4239-95bb-83d0f6d0da78}"></ms_compatibility:supportedOS>
            <ms_compatibility:supportedOS xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}"></ms_compatibility:supportedOS>
        </ms_compatibility:application>
    </ms_compatibility:compatibility>
</assembly>
```

For reference, here's `cargo`'s manifest from 1.91

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0" 
    xmlns:asmv3="urn:schemas-microsoft-com:asm.v3">
    <trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
        <security>
            <requestedPrivileges>
                <requestedExecutionLevel level="asInvoker" uiAccess="false"></requestedExecutionLevel>
            </requestedPrivileges>
        </security>
    </trustInfo>
    <asmv3:application>
        <asmv3:windowsSettings xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings" 
            xmlns:ws2="http://schemas.microsoft.com/SMI/2016/WindowsSettings">
            <ws2:longPathAware>true</ws2:longPathAware>
            <activeCodePage>UTF-8</activeCodePage>
        </asmv3:windowsSettings>
    </asmv3:application>
    <ms_compatibility:compatibility xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" 
        xmlns="urn:schemas-microsoft-com:compatibility.v1">
        <ms_compatibility:application xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1">
            <ms_compatibility:supportedOS xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" Id="{35138b9a-5d96-4fbd-8e2d-a2440225f93a}"></ms_compatibility:supportedOS>
            <ms_compatibility:supportedOS xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" Id="{4a2f28e3-53b9-4441-ba9c-d69d4a4a6e38}"></ms_compatibility:supportedOS>
            <ms_compatibility:supportedOS xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" Id="{1f676c76-80e1-4239-95bb-83d0f6d0da78}"></ms_compatibility:supportedOS>
            <ms_compatibility:supportedOS xmlns:ms_compatibility="urn:schemas-microsoft-com:compatibility.v1" Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}"></ms_compatibility:supportedOS>
        </ms_compatibility:application>
    </ms_compatibility:compatibility>
</assembly>
```

Closes #16877

## Test Plan

Before changes on Windows 11 25H2 (without SeCreateSymbolicLinkPrivilege)

```console
$ uv init
$ uv add jupyterlab-widgets==3.0.16 --link-mode=symlink
...
Resolved 2 packages in [TIME]
error: Failed to install: jupyterlab_widgets-3.0.16-py3-none-any.whl (jupyterlab-widgets==3.0.16)
  Caused by: failed to symlink file from [CACHE_DIR]\archive-v0\aQcqEjLJAkVwuSzohqymc\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js to [ROOT]\.venv\Lib\site-packages\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js: A required privilege is not held by the client. (os error 1314)
```

Before changes on Windows 11 25H2 (with SeCreateSymbolicLinkPrivilege)

```console
$ uv init
$ uv add jupyterlab-widgets==3.0.16 --link-mode=symlink
...
Resolved 2 packages in [TIME]
error: Failed to install: jupyterlab_widgets-3.0.16-py3-none-any.whl (jupyterlab-widgets==3.0.16)
  Caused by: failed to symlink file from [CACHE_DIR]\archive-v0\aQcqEjLJAkVwuSzohqymc\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js to [ROOT]\.venv\Lib\site-packages\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js: The parameter is incorrect. (os error 87)
```

After changes on Windows 11 25H2 (with or without SeCreateSymbolicLinkPrivilege)

```console
$ uv init
$ uv add jupyterlab-widgets==3.0.16 --link-mode=symlink
...
Resolved 2 packages in [TIME]
Installed 1 package in [TIME]
 + jupyterlab-widgets==3.0.16
```



---

_Marked ready for review by @samypr100 on 2025-11-30 19:16_

---

_Comment by @zanieb on 2025-12-01 18:20_

Do we need anything for uvx and/or uvw?

---

_Comment by @samypr100 on 2025-12-01 18:24_

> Do we need anything for uvx and/or uvw?

Yes, they should all have the same manifest

---

_@zanieb approved on 2025-12-01 18:24_

---

_Comment by @zanieb on 2025-12-01 18:25_

Does it matter though since they just spawn `uv` immediately? i.e., they don't need long path support right?

---

_Label `enhancement` added by @zanieb on 2025-12-01 18:25_

---

_Label `windows` added by @zanieb on 2025-12-01 18:25_

---

_Comment by @samypr100 on 2025-12-01 18:26_

> Does it matter though since they just spawn `uv` immediately? i.e., they don't need long path support right?

Good point, I guess it doesn't matter in this case

---

_Merged by @zanieb on 2025-12-01 20:02_

---

_Closed by @zanieb on 2025-12-01 20:02_

---

_Branch deleted on 2025-12-01 20:57_

---
