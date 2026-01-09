---
number: 10378
title: "Unable to upload ruff==0.3.2 to Azure Artifacts because it doesn't support Metadata-Version=2.3"
type: issue
state: closed
author: olivierlefloch
labels: []
assignees: []
created_at: 2024-03-13T06:21:18Z
updated_at: 2024-03-29T19:58:22Z
url: https://github.com/astral-sh/ruff/issues/10378
synced_at: 2026-01-07T13:12:15-06:00
---

# Unable to upload ruff==0.3.2 to Azure Artifacts because it doesn't support Metadata-Version=2.3

---

_Issue opened by @olivierlefloch on 2024-03-13 06:21_

When attempting to upload `ruff==0.3.2` to Azure Artifacts, I get the following error:

```
Uploading ruff-0.3.2-py3-none-musllinux_1_2_x86_64.whl
INFO     Response from                                                          
         https://pkgs.dev.azure.com/ORGNAME/_packaging/REPONAME/pypi/upload:    
         400 Bad Request - Metadata version '2.3' is unsupported. Supported     
         versions are: 1.0,1.1,1.2,2.0,2.1. (DevOps Activity ID:                
         27498FFA-3436-49CE-B4F4-27A5F86057FE)                                  
INFO     {"$id":"1","innerException":null,"message":"Metadata version '2.3' is  
         unsupported. Supported versions are:                                   
         1.0,1.1,1.2,2.0,2.1.","typeName":"Microsoft.VisualStudio.Services.Packa
         ging.Shared.WebApi.Exceptions.InvalidPackageException,                 
         Microsoft.VisualStudio.Services.Packaging.Shared.WebApi","typeKey":"Inv
         alidPackageException","errorCode":0,"eventId":3000}                    
ERROR    HTTPError: 400 Bad Request from                                        
         https://pkgs.dev.azure.com/ORGNAME/_packaging/REPONAME/pypi/upload     
         Bad Request - Metadata version '2.3' is unsupported. Supported versions
         are: 1.0,1.1,1.2,2.0,2.1. (DevOps Activity ID:                         
         27498FFA-3436-49CE-B4F4-27A5F86057FE)                                  
```

The error is, in practice, on Azure Artifacts' side, as they are not, it would seem, compliant with the spirit of the Python packaging spec:

https://packaging.python.org/en/latest/specifications/core-metadata/#metadata-version

> Automated tools consuming metadata SHOULD warn if metadata_version is greater than the highest version they support, and MUST fail if metadata_version has a greater major version than the highest version they support (as described in the Version specifier specification, the major version is the value before the first dot).

However, I wonder if it might be possible to resolve the issue on the `ruff` side. An initial exploration of the codebase / recent commits did not clearly indicate what has caused this difference in the built wheels. Based on the diff between the `METADATA` files between versions `0.3.1` and `0.3.2`:

```
--- .../ruff-0.3.1-py3-none-macosx_10_12_x86_64.macosx_11_0_arm64.macosx_10_12_universal2/ruff-0.3.1.dist-info/METADATA	2024-03-06 22:41:54
+++ .../ruff-0.3.2-py3-none-macosx_10_12_x86_64.macosx_11_0_arm64.macosx_10_12_universal2/ruff-0.3.2.dist-info/METADATA	2024-03-09 00:49:26
@@ -1,6 +1,6 @@
-Metadata-Version: 2.1
+Metadata-Version: 2.3
 Name: ruff
-Version: 0.3.1
+Version: 0.3.2
 Classifier: Development Status :: 5 - Production/Stable
 Classifier: Environment :: Console
 Classifier: Intended Audience :: Developers
@@ -179,7 +179,7 @@
 ```yaml
 - repo: https://github.com/astral-sh/ruff-pre-commit
   # Ruff version.
-  rev: v0.3.1
+  rev: v0.3.2
   hooks:
     # Run the linter.
     - id: ruff
```

it seems the set of fields being used hasn't changed, which suggests, following the spec, that it should be possible to downgrade to `Metadata-Version: 2.1`:

> For broader compatibility, build tools MAY choose to produce distribution metadata using the lowest metadata version that includes all of the needed fields.

Based on the absence of other changes, I assume some difference in the build pipeline is the underlying cause of this change.

I've also reported this issue to Azure Artifacts: https://developercommunity.visualstudio.com/t/Uploading-ruff032-to-Azure-Repos-for/10614507

---

_Referenced in [astral-sh/uv#2414](../../astral-sh/uv/issues/2414.md) on 2024-03-13 14:34_

---

_Comment by @charliermarsh on 2024-03-13 14:37_

Interesting. I think this is going to cause them a lot of problems pretty quickly.

---

_Referenced in [PyO3/maturin#1993](../../PyO3/maturin/issues/1993.md) on 2024-03-14 16:34_

---

_Comment by @olivierlefloch on 2024-03-14 16:38_

After further investigation, it seems `maturin` recently updated the value they use for `Metadata-Version` https://github.com/PyO3/maturin/issues/1993

I agree this is likely going to be a significant problem on the Azure Artifacts side of things, and other than perhaps pinning `maturin` in `ruff`'s build pipeline to ensure such changes don't occur unexpectedly, I don't think this is actually an issue that could/should be fixed within `ruff`'s codebase.

---

_Comment by @MichaReiser on 2024-03-18 16:45_

Thanks for reporting this in maturin and on the Microsoft platform. Let's see if this gets resolved upstream.

---

_Comment by @charliermarsh on 2024-03-18 16:46_

I wrote this in uv:

> We could pin back to Maturin 0.14.0 or whichever it was that preceded Metadata 2.2 (and Metadata 2.3), but Metadata 2.2 and 2.3 are actually good, and soon Poetry and Hatch and others will build source distributions with Metadata 2.2 by default.

> I'm gonna say for now that we won't roll this back, but if it continues to be a problem for an extended period of time let me know and we can reconsider.

I'd vote to close here too, since I think it would be suboptimal to roll this back.

---

_Closed by @charliermarsh on 2024-03-29 19:58_

---

_Comment by @charliermarsh on 2024-03-29 19:58_

Closing this for the same reason as in uv.

---
