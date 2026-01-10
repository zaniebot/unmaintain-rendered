```yaml
number: 10683
title: "`uv lock` fails to update outdated lockfile if project is in cache"
type: issue
state: open
author: rg936672
labels:
  - needs-decision
assignees: []
created_at: 2025-01-16T15:12:21Z
updated_at: 2025-01-20T03:55:50Z
url: https://github.com/astral-sh/uv/issues/10683
synced_at: 2026-01-10T04:27:58Z
```

# `uv lock` fails to update outdated lockfile if project is in cache

---

_Issue opened by @rg936672 on 2025-01-16 15:12_

We've just ran into an issue with uv on our repo [coreax](https://github.com/gchq/coreax) where uv was complaining the lockfile needed to be updated, but running `uv lock` did nothing.

Reproduction (sorry, this isn't a minimal example, but it should work):

```shell
> uv -V
uv 0.5.20 (1c17662b3 2025-01-15)
> git clone https://github.com/gchq/coreax.git
> cd coreax
> git checkout dd1332db204153334b846a140b208c2990a8dc42
> uv sync --frozen
> uv lock --check  # fails
Resolved 246 packages in 371ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
> uv lock
Resolved 246 packages in 374ms
> uv lock --check  # fails
Resolved 246 packages in 371ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
> cp uv.lock uv.lock.bad  # to compare with the second trial, which works properly
```

The diff here doesn't seem to be much of interest:

<details>
<summary>git diff uv.lock</summary>

```diff
diff --git a/uv.lock b/uv.lock
index 498c0c4..4c9ceff 100644
--- a/uv.lock
+++ b/uv.lock
@@ -2825,7 +2825,6 @@ name = "nvidia-cublas-cu12"
 version = "12.4.5.8"
 source = { registry = "https://pypi.org/simple" }
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/7f/7f/7fbae15a3982dc9595e49ce0f19332423b260045d0a6afe93cdbe2f1f624/nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_aarch64.whl", hash = "sha256:0f8aa1706812e00b9f19dfe0cdb3999b092ccb8ca168c0db5b8ea712456fd9b3", size = 363333771 },
     { url = "https://files.pythonhosted.org/packages/ae/71/1c91302526c45ab494c23f61c7a84aa568b8c1f9d196efa5993957faf906/nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_x86_64.whl", hash = "sha256:2fc8da60df463fdefa81e323eef2e36489e1c94335b5358bcb38360adf75ac9b", size = 363438805 },
 ]

@@ -2834,7 +2833,6 @@ name = "nvidia-cuda-cupti-cu12"
 version = "12.4.127"
 source = { registry = "https://pypi.org/simple" }
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/93/b5/9fb3d00386d3361b03874246190dfec7b206fd74e6e287b26a8fcb359d95/nvidia_cuda_cupti_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl", hash = "sha256:79279b35cf6f91da114182a5ce1864997fd52294a87a16179ce275773799458a", size = 12354556 },
     { url = "https://files.pythonhosted.org/packages/67/42/f4f60238e8194a3106d06a058d494b18e006c10bb2b915655bd9f6ea4cb1/nvidia_cuda_cupti_cu12-12.4.127-py3-none-manylinux2014_x86_64.whl", hash = "sha256:9dec60f5ac126f7bb551c055072b69d85392b13311fcc1bcda2202d172df30fb", size = 13813957 },
 ]

@@ -2843,7 +2841,6 @@ name = "nvidia-cuda-nvrtc-cu12"
 version = "12.4.127"
 source = { registry = "https://pypi.org/simple" }
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/77/aa/083b01c427e963ad0b314040565ea396f914349914c298556484f799e61b/nvidia_cuda_nvrtc_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl", hash = "sha256:0eedf14185e04b76aa05b1fea04133e59f465b6f960c0cbf4e37c3cb6b0ea198", size = 24133372 },
     { url = "https://files.pythonhosted.org/packages/2c/14/91ae57cd4db3f9ef7aa99f4019cfa8d54cb4caa7e00975df6467e9725a9f/nvidia_cuda_nvrtc_cu12-12.4.127-py3-none-manylinux2014_x86_64.whl", hash = "sha256:a178759ebb095827bd30ef56598ec182b85547f1508941a3d560eb7ea1fbf338", size = 24640306 },
 ]

@@ -2852,7 +2849,6 @@ name = "nvidia-cuda-runtime-cu12"
 version = "12.4.127"
 source = { registry = "https://pypi.org/simple" }
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/a1/aa/b656d755f474e2084971e9a297def515938d56b466ab39624012070cb773/nvidia_cuda_runtime_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl", hash = "sha256:961fe0e2e716a2a1d967aab7caee97512f71767f852f67432d572e36cb3a11f3", size = 894177 },
     { url = "https://files.pythonhosted.org/packages/ea/27/1795d86fe88ef397885f2e580ac37628ed058a92ed2c39dc8eac3adf0619/nvidia_cuda_runtime_cu12-12.4.127-py3-none-manylinux2014_x86_64.whl", hash = "sha256:64403288fa2136ee8e467cdc9c9427e0434110899d07c779f25b5c068934faa5", size = 883737 },
 ]

@@ -2875,7 +2871,6 @@ dependencies = [
     { name = "nvidia-nvjitlink-cu12", marker = "(python_full_version < '3.10' and platform_machine != 'arm64' and sys_platform == 'darwin') or (platform_machine != 'aarch64' and sys_platform == 'linux') or (sys_platform != 'darwin' and sys_platform != 'linux')" },
 ]
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/7a/8a/0e728f749baca3fbeffad762738276e5df60851958be7783af121a7221e7/nvidia_cufft_cu12-11.2.1.3-py3-none-manylinux2014_aarch64.whl", hash = "sha256:5dad8008fc7f92f5ddfa2101430917ce2ffacd86824914c82e28990ad7f00399", size = 211422548 },
     { url = "https://files.pythonhosted.org/packages/27/94/3266821f65b92b3138631e9c8e7fe1fb513804ac934485a8d05776e1dd43/nvidia_cufft_cu12-11.2.1.3-py3-none-manylinux2014_x86_64.whl", hash = "sha256:f083fc24912aa410be21fa16d157fed2055dab1cc4b6934a0e03cba69eb242b9", size = 211459117 },
 ]

@@ -2884,7 +2879,6 @@ name = "nvidia-curand-cu12"
 version = "10.3.5.147"
 source = { registry = "https://pypi.org/simple" }
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/80/9c/a79180e4d70995fdf030c6946991d0171555c6edf95c265c6b2bf7011112/nvidia_curand_cu12-10.3.5.147-py3-none-manylinux2014_aarch64.whl", hash = "sha256:1f173f09e3e3c76ab084aba0de819c49e56614feae5c12f69883f4ae9bb5fad9", size = 56314811 },
     { url = "https://files.pythonhosted.org/packages/8a/6d/44ad094874c6f1b9c654f8ed939590bdc408349f137f9b98a3a23ccec411/nvidia_curand_cu12-10.3.5.147-py3-none-manylinux2014_x86_64.whl", hash = "sha256:a88f583d4e0bb643c49743469964103aa59f7f708d862c3ddb0fc07f851e3b8b", size = 56305206 },
 ]

@@ -2898,7 +2892,6 @@ dependencies = [
     { name = "nvidia-nvjitlink-cu12", marker = "(python_full_version < '3.10' and platform_machine != 'arm64' and sys_platform == 'darwin') or (platform_machine != 'aarch64' and sys_platform == 'linux') or (sys_platform != 'darwin' and sys_platform != 'linux')" },
 ]
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/46/6b/a5c33cf16af09166845345275c34ad2190944bcc6026797a39f8e0a282e0/nvidia_cusolver_cu12-11.6.1.9-py3-none-manylinux2014_aarch64.whl", hash = "sha256:d338f155f174f90724bbde3758b7ac375a70ce8e706d70b018dd3375545fc84e", size = 127634111 },
     { url = "https://files.pythonhosted.org/packages/3a/e1/5b9089a4b2a4790dfdea8b3a006052cfecff58139d5a4e34cb1a51df8d6f/nvidia_cusolver_cu12-11.6.1.9-py3-none-manylinux2014_x86_64.whl", hash = "sha256:19e33fa442bcfd085b3086c4ebf7e8debc07cfe01e11513cc6d332fd918ac260", size = 127936057 },
 ]

@@ -2910,7 +2903,6 @@ dependencies = [
     { name = "nvidia-nvjitlink-cu12", marker = "(python_full_version < '3.10' and platform_machine != 'arm64' and sys_platform == 'darwin') or (platform_machine != 'aarch64' and sys_platform == 'linux') or (sys_platform != 'darwin' and sys_platform != 'linux')" },
 ]
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/96/a9/c0d2f83a53d40a4a41be14cea6a0bf9e668ffcf8b004bd65633f433050c0/nvidia_cusparse_cu12-12.3.1.170-py3-none-manylinux2014_aarch64.whl", hash = "sha256:9d32f62896231ebe0480efd8a7f702e143c98cfaa0e8a76df3386c1ba2b54df3", size = 207381987 },
     { url = "https://files.pythonhosted.org/packages/db/f7/97a9ea26ed4bbbfc2d470994b8b4f338ef663be97b8f677519ac195e113d/nvidia_cusparse_cu12-12.3.1.170-py3-none-manylinux2014_x86_64.whl", hash = "sha256:ea4f11a2904e2a8dc4b1833cc1b5181cde564edd0d5cd33e3c168eff2d1863f1", size = 207454763 },
 ]

@@ -2927,7 +2919,6 @@ name = "nvidia-nvjitlink-cu12"
 version = "12.4.127"
 source = { registry = "https://pypi.org/simple" }
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/02/45/239d52c05074898a80a900f49b1615d81c07fceadd5ad6c4f86a987c0bc4/nvidia_nvjitlink_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl", hash = "sha256:4abe7fef64914ccfa909bc2ba39739670ecc9e820c83ccc7a6ed414122599b83", size = 20552510 },
     { url = "https://files.pythonhosted.org/packages/ff/ff/847841bacfbefc97a00036e0fce5a0f086b640756dc38caea5e1bb002655/nvidia_nvjitlink_cu12-12.4.127-py3-none-manylinux2014_x86_64.whl", hash = "sha256:06b3b9b25bf3f8af351d664978c9739670ecc9e820c83ccc7a6ed414122599b83", size = 20552510 },
     { url = "https://files.pythonhosted.org/packages/ff/ff/847841bacfbefc97a00036e0fce5a0f086b640756dc38caea5e1bb002655/nvidia_nvjitlink_cu12-12.4.127-py3-none-manylinux2014_x86_64.whl", hash = "sha256:06b3b9b25bf3f8af351d664978ca26a16d2c5127dbd53c0497e28d1fb9611d57", size = 21066810 },
 ]

@@ -2936,7 +2927,6 @@ name = "nvidia-nvtx-cu12"
 version = "12.4.127"
 source = { registry = "https://pypi.org/simple" }
 wheels = [
-    { url = "https://files.pythonhosted.org/packages/06/39/471f581edbb7804b39e8063d92fc8305bdc7a80ae5c07dbe6ea5c50d14a5/nvidia_nvtx_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl", hash = "sha256:7959ad635db13edf4fc65c06a6e9f9e55fc2f92596db928d169c0bb031e88ef3", size = 100417 },
     { url = "https://files.pythonhosted.org/packages/87/20/199b8713428322a2f22b722c62b8cc278cc53dffa9705d744484b5035ee9/nvidia_nvtx_cu12-12.4.127-py3-none-manylinux2014_x86_64.whl", hash = "sha256:781e950d9b9f60d8241ccea575b32f5105a5baf4c2351cab5256a24869f12a1a", size = 99144 },
 ]
```
</details>

However, if the project is not in the cache, we get a different result:

```shell
> git reset --hard
> uv cache clean
> uv lock --check
Resolved 246 packages in 3.98s
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
> uv lock 
Resolved 246 packages in 378ms
Removed coreax v0.3.1
> git diff --no-index uv.lock.bad uv.lock
diff --git a/uv.lock.bad b/uv.lock
index 4c9ceff..b8fa2bc 100644
--- a/uv.lock.bad
+++ b/uv.lock
@@ -667,7 +667,6 @@ wheels = [

 [[package]]
 name = "coreax"
-version = "0.3.1"
 source = { editable = "." }
 dependencies = [
     { name = "equinox", version = "0.11.10", source = { registry = "https://pypi.org/simple" }, marker = "python_full_version < '3.10'" },
```

Inspecting the repo history, the offending `uv.lock` was generated using uv 0.5.18 (in [this CI job](https://github.com/gchq/coreax/actions/runs/12729366534/job/35481066298), creating [this commit](https://github.com/gchq/coreax/tree/d65eae5eec3f3ea18830d78383a242cfd30a3d34)).

We were fortunately able to work around the issue using `uv lock --upgrade` with uv 0.5.20; it was only in creating a working example that I discovered the dependency on the cache.

At a guess, this is maybe something to do with `coreax`'s version being set dynamically in `pyproject.toml`?


---

_Comment by @charliermarsh on 2025-01-16 15:14_

Can you include the verbose logging from `uv lock --check  # fails`?

---

_Comment by @rg936672 on 2025-01-16 15:25_

https://gist.github.com/rg936672/bb6705cce6bc1755e80d473d8835dc59

---

_Comment by @charliermarsh on 2025-01-16 15:28_

Ahhh I see. This is basically a temporary effect of my PR to remove dynamic versions (#10622)... In short, we now reject your existing lockfile because it "looks like" the package has a static version in the lockfile but a dynamic version in practice. But in the cache, we're returning metadata that doesn't indicate a dynamic version (since the metadata is already cached and is missing that "dynamic" field).

Arguably, I should've bumped the cache version there to avoid this. I need to think on whether that's worth doing.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-16 15:28_

---

_Label `needs-decision` added by @charliermarsh on 2025-01-16 15:28_

---

_Comment by @charliermarsh on 2025-01-16 15:28_

(I wouldn't expect this to be an ongoing problem.)

---

_Comment by @flipdazed on 2025-01-16 17:30_

when will it be sorted? ty!

---

_Comment by @zanieb on 2025-01-16 17:35_

I imagine this would be resolved with `uv cache clean` or `uv lock --refresh`?

---

_Comment by @charliermarsh on 2025-01-16 17:37_

@flipdazed -- I don't think this should be an ongoing problem? `uv cache clean {package_name}` should solve it for you.

---

_Comment by @flipdazed on 2025-01-16 17:43_

Maybe my problem is unrelated - it appeared in a runner out of nowhere though around the same time

```
#15 [build 10/10] RUN --mount="type=secret,id=*" uv sync --no-editable
#15 0.332 error: Failed to parse `uv.lock`
#15 0.332   Caused by: TOML parse error at line 284, column 1
#15 0.332     |
#15 0.332 284 | [[package]]
#15 0.332     | ^^^^^^^^^^^
#15 0.332 missing field `version`
#15 ERROR: process "/bin/sh -c uv sync --no-editable" did not complete successfully: exit code: 2
```

because it's in a build stage of a docker image, I don't know if we can just clear it?

---

_Comment by @charliermarsh on 2025-01-16 17:45_

I think you must be using two different versions of uv -- is that possible? It looks like the lockfile was generated by a more recent version than whatever your runner is using.

---

_Comment by @flipdazed on 2025-01-16 17:46_

Is this a sep issue? Shall I create a new thread?

---

_Comment by @charliermarsh on 2025-01-16 17:49_

There isn't anything we can do about that, now that it's released. We arguably should've bumped the lockfile version or something similar, but we can't change the behavior for already-released uv versions. You need to bump the uv version in your runner.

---

_Comment by @flipdazed on 2025-01-16 17:50_

We have a base python image that does indeed have an older lock-file - we then run `uv sync` on that base image. Are you saying that I need to go through all may base images and update them all to the new uv version ?

---

_Comment by @charliermarsh on 2025-01-16 17:54_

You need to avoid generating a lockfile on newer versions of uv, and then attempting to sync that lockfile on older versions of uv. So if you generated your lockfile with a newer version of uv than is what's on your base image, I suggest either (1) upgrading the base image or (2) downgrading your local version to match whatever is being used in the base image.

---

_Comment by @flipdazed on 2025-01-16 18:13_

Is there any workaround to basically hack this to get it running in the meantime like so?

```
RUN --mount="type=secret,id=***" uv lock --refresh
RUN --mount="type=secret,id=***" uv sync --no-editable
```

---

_Comment by @moonbox3 on 2025-01-17 06:33_

I am still running into the "missing field `version`" issue even after upgrading to 0.5.20. I re-generated the uv lock file locally (first had to run `uv lock --refresh` and then `uv lock`) with 0.5.20 as I had previously modified it with 0.5.18. I saw that it removed the static package version from the uv lock, which seemed fine. However, while running the `uv-pre-commit` in GH Actions, I see:

```
error: Failed to parse `uv.lock`
  Caused by: TOML parse error at line 4711, column 1
     |
4711 | [[package]]
     | ^^^^^^^^^^^
missing field `version`
```

Failed run is [here](https://github.com/microsoft/semantic-kernel/actions/runs/12823813719/job/35759054100?pr=10191#step:5:204) with raw logs [here](https://productionresultssa4.blob.core.windows.net/actions-results/09111564-ca97-47e3-ba90-eb8c77ee9762/workflow-job-run-c64f4189-7663-57d2-81db-f67c8e63a324/logs/job/job-logs.txt?rsct=text%2Fplain&se=2025-01-17T06%3A41%3A37Z&sig=0XangXIWM%2BIrdwfJ%2FzyTH2HcKLUwN%2BZT0zEg1LdR6tw%3D&ske=2025-01-17T18%3A18%3A26Z&skoid=ca7593d4-ee42-46cd-af88-8b886a2f84eb&sks=b&skt=2025-01-17T06%3A18%3A26Z&sktid=398a6654-997b-47e9-b12b-9515b896b4de&skv=2024-11-04&sp=r&spr=https&sr=b&st=2025-01-17T06%3A31%3A32Z&sv=2024-11-04).

I do see our GH Actions installing 0.5.20:

```
Downloading uv from "https://github.com/astral-sh/uv/releases/download/0.5.20/uv-x86_64-unknown-linux-gnu.tar.gz" ...
/usr/bin/tar xz --warning=no-unknown-keyword --overwrite -C /home/runner/work/_temp/017df719-ca42-44fc-900d-acbb6fc05a63 -f /home/runner/work/_temp/3fdd0ed4-532d-468e-b048-e59a7da8c6c8
Added /opt/hostedtoolcache/uv/0.5.20/x86_64 to the path
Added /home/runner/.local/bin to the path
Set UV_CACHE_DIR to /home/runner/work/_temp/setup-uv-cache
Successfully installed uv version 0.5.20
```

I removed cached files to make sure everything started fresh, just in case.

Am I missing something? Thanks for your help.

---

_Comment by @charliermarsh on 2025-01-17 15:20_

@moonbox3 -- I think you must be running an older uv version in pre-commit (which is versioned separately from whatever you install with GH Actions).

---

_Comment by @moonbox3 on 2025-01-20 03:55_

> [@moonbox3](https://github.com/moonbox3) -- I think you must be running an older uv version in pre-commit (which is versioned separately from whatever you install with GH Actions).

Thanks for the tip. As I was debugging the last pre-commit showed as `0.5.20`. I now see `0.5.21` and that works. Appreciate your help.

---
