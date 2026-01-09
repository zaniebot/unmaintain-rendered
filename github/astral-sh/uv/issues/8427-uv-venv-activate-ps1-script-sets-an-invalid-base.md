---
number: 8427
title: "uv venv: Activate.ps1 script sets an invalid base directory when using UNC paths."
type: issue
state: closed
author: NucaChance
labels: []
assignees: []
created_at: 2024-10-21T21:15:43Z
updated_at: 2025-10-02T15:24:13Z
url: https://github.com/astral-sh/uv/issues/8427
synced_at: 2026-01-07T13:12:17-06:00
---

# uv venv: Activate.ps1 script sets an invalid base directory when using UNC paths.

---

_Issue opened by @NucaChance on 2024-10-21 21:15_

**PLATFORM:** 

* Windows Server 2019 
* PowerShell 7.4.5
* Python 3.12.4

**UV Version:**  `uv 0.4.25 (97eb6ab4a 2024-10-21)`

This isn't _exactly_ a UV bug, but to handle UNC paths the `activate.ps1` script needs to change how it resolves the path. 

## The Issue

Currently the script is using `Resolve-Path` which returns a `PathInfo` object, and passing that to `Split-Path`, causing it to be cast to a string. For whatever reason the cast contains the ` Microsoft.PowerShell.Core\FileSystem::` provider information when resolving a UNC path, and Split-Path chokes on it. 

### Test script

This is just to show the behavior without running the full activate script.

```test.ps1
$script:THIS_PATH = $myinvocation.mycommand.path
$script:BASE_DIR = Split-Path (Resolve-Path "$THIS_PATH/..").ProviderPath -Parent

$script:ALT_BASE_DIR_1 = Split-Path (Resolve-Path "$THIS_PATH/..").ProviderPath -Parent


$script:CURRENT_FILE = Get-Item $myinvocation.mycommand.path
$script:ALT_BASE_DIR_2 = $CURRENT_FILE.Directory.Parent.FullName

Write-Host "Base Directory: $BASE_DIR"
Write-Host "Alternate Base Directory: $ALT_BASE_DIR_1"
Write-Host "Alternate  Alternate Base Directory: $ALT_BASE_DIR_2"

```

Running the script with sanitized paths:
```run-test.ps1
# Run Local to machine
PS > D:\python\test\.venv\Scripts\test.ps1
Base Directory: D:\python\test\.venv
Alternate Base Directory: D:\python\test\.venv
Alternate Alternate Base Directory: D:\python\test\.venv

# Run on network share
PS > \\storage.my.company.net\network_share\test-venv\.venv\Scripts\test.ps1
Base Directory: Microsoft.PowerShell.Core\FileSystem::\\storage.my.company.net\network_share\test-venv\.venv\Scripts\test.ps1
Alternate Base Directory: \\storage.my.company.net\network_share\test-venv\.venv
Alternate Alternate Base Directory: \\storage.my.company.net\network_share\test-venv\.venv

```

## Fixes

### Option 1:

Just specify you want the `.ProviderPath` like this:

```option-1.ps1
$script:THIS_PATH = $myinvocation.mycommand.path
$script:ALT_BASE_DIR_1 = Split-Path (Resolve-Path "$THIS_PATH/..").ProviderPath -Parent
```

### Option 2:
> My personal pick

Get the `[FileInfo]` object for the script file, and use it's properties like so:

```option-2.ps1
$script:CURRENT_FILE = Get-Item $myinvocation.mycommand.path
$script:ALT_BASE_DIR_2 = $CURRENT_FILE.Directory.Parent.FullName

```






---

_Renamed from "uv venv: Activate.ps1 script can set an invalid base directory when using UNC paths." to "uv venv: Activate.ps1 script sets an invalid base directory when using UNC paths." by @NucaChance on 2024-11-12 14:51_

---

_Comment by @NucaChance on 2025-10-02 15:24_

I just tried this in version `8.2.2` and it appears to be fixed.

---

_Closed by @NucaChance on 2025-10-02 15:24_

---
