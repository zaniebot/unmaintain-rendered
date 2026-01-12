```yaml
number: 4077
title: Native TLS option fails on corporate laptop.
type: issue
state: closed
author: Syrus-at-Philips
labels:
  - needs-mre
  - external
assignees: []
created_at: 2024-06-05T21:05:12Z
updated_at: 2024-12-12T23:40:17Z
url: https://github.com/astral-sh/uv/issues/4077
synced_at: 2026-01-12T15:58:48Z
```

# Native TLS option fails on corporate laptop.

---

_@Syrus-at-Philips_

Previous versions, prior to 0.2.5 I think, worked on my corporate laptop that includes the Cisco Umbrella proxy. The current version 0.2.7 (and 0.2.6 and 0.2.5) appears to have broken support for native TLS, the `--native-tls` option does nothing. The issue is in the Windows environment. `uv` does still work on linux in WSL2 as long as I'm not connected to the corporate firewall.

```
~\junk on ☁️  (us-east-1)
❯ uv --version
uv 0.2.7 (a241f148d 2024-06-05)

~\junk on ☁️  (us-east-1)
❯ uv venv
Using Python 3.11.9 interpreter at: C:\Users\320159535\mambaforge\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate

~\junk on ☁️  (us-east-1)
❯ .venv\Scripts\activate

~\junk via 🐍 v3.11.9 (junk) on ☁️  (us-east-1)
❯ uv pip install -v numpy
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.11.9 at `C:\Users\320159535\junk\.venv\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.11.9 environment at .venv\Scripts\python.exe
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: numpy
DEBUG Using registry request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: numpy*
DEBUG Found stale response for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/
DEBUG Sending revalidation request for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/
DEBUG Transient request failure for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/, retrying: Request error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/, retrying: Request error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/, retrying: Request error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/, retrying: Request error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer

~\junk via 🐍 v3.11.9 (junk) on ☁️  (us-east-1) took 4s
❯ uv pip install --native-tls -v numpy
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.11.9 at `C:\Users\320159535\junk\.venv\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.11.9 environment at .venv\Scripts\python.exe
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: numpy
DEBUG Using registry request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: numpy*
DEBUG Found stale response for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/
DEBUG Sending revalidation request for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/
DEBUG Transient request failure for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/, retrying: Request error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/, retrying: Request error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/, retrying: Request error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/, retrying: Request error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
error: error sending request for url (https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
```

Note that the environment variable UV_INDEX_URL is set to point to a JFrog pypi mirror that includes proprietary packages. I used to have the variable set for native TLS, but I removed it when it stopped working sometime around version 0.2.5 (it could have been an earlier version).

---

_Comment by @charliermarsh on 2024-06-05 21:08_

As far as I can tell, we didn't change anything here from 0.2.5 to 0.2.7. We did upgrade some dependencies. But `native_tls` is still being respected in the codebase. Can you please determine which version exactly this stopped working for you? There's no way for me to debug it since I don't have access to your environment or your index.

---

_Label `needs-mre` added by @charliermarsh on 2024-06-05 21:08_

---

_Comment by @Syrus-at-Philips on 2024-06-05 23:41_

I will investigate further and report back.

---

_Comment by @charliermarsh on 2024-06-05 23:51_

Thanks.

---

_Comment by @Syrus-at-Philips on 2024-06-06 22:10_

Update: I was trying different versions and noted that `uv self update` is working again for 0.2.9. However, the peer certificate error still occurs when trying to do a pip install.

```
~\junk via 🐍 v3.11.9 (junk) on ☁️  (us-east-1)
❯ uv self update                                                                                                          info: Checking for updates...                                                                                             success: You're on the latest version of `uv` (v0.2.9).                                                                                                                                                                                             ~\junk via �� v3.11.9 (junk) on ☁️  (us-east-1)                                                                           ❯ powershell -c "irm https://github.com/astral-sh/uv/releases/download/0.2.8/uv-installer.ps1 | iex"                      Downloading uv 0.2.8 (x86_64-pc-windows-msvc)
Installing to C:\Users\320159535\.cargo\bin
  uv.exe
Everything's installed!

~\junk via 🐍 v3.11.9 (junk) on ☁️  (us-east-1) took 6s
❯ uv self update
info: Checking for updates...
error: error sending request for url (https://objects.githubusercontent.com/github-production-release-asset-2e65be/699532645/d12c4ef0-c66f-43d6-9cf9-40fc46e8205a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20240606%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240606T220805Z&X-Amz-Expires=300&X-Amz-Signature=ecdeab1b3afe738495483fae0276ce1bb1497d86d47b664fd57776c44467c488&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=699532645&response-content-disposition=attachment%3B%20filename%3Duv-installer.ps1&response-content-type=application%2Foctet-stream)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer

~\junk via 🐍 v3.11.9 (junk) on ☁️  (us-east-1)
❯ powershell -c "irm https://github.com/astral-sh/uv/releases/download/0.2.9/uv-installer.ps1 | iex"
Downloading uv 0.2.9 (x86_64-pc-windows-msvc)
Installing to C:\Users\320159535\.cargo\bin
  uv.exe
Everything's installed!

~\junk via 🐍 v3.11.9 (junk) on ☁️  (us-east-1) took 6s
❯ uv self update
info: Checking for updates...
success: You're on the latest version of `uv` (v0.2.9).

~\junk via 🐍 v3.11.9 (junk) on ☁️  (us-east-1)
❯
```

For the previous several versions, I could not run `uv self update`, but now it runs without an error and without the `--native-tls` option. However, `uv pip install` still fails with the same peer certificate error seen above for `uv self update` for 0.2.8.

What I observe is that for the self-update operation, version 0.2.9 changed behavior and is able to use my system certificate. However, the same is not true for `uv pip install --native-tls numpy`, which still produces the _invalid peer certificate_  error. Does the self update function use the same dependencies and settings to authenticate TLS as the pip install function?

---

_Comment by @Syrus-at-Philips on 2024-06-07 00:57_

So, 0.2.9 also works if my index source is pypi.org rather than our proprietary server. But it's not a real certificate issue on our server because uv is perfectly happy to accept those certificates if I run on the same machine using linux on WSL.

```
~/junk via 🐍 v3.12.3 (junk) on ☁️  (us-east-1) took 9s
❯ uv --version
uv 0.2.9

~/junk via 🐍 v3.12.3 (junk) on ☁️  (us-east-1)
❯ uv clean
Clearing cache at: /home/syrus/.cache/uv
Removed 938 files (65.0MiB)

~/junk via 🐍 v3.12.3 (junk) on ☁️  (us-east-1)
❯ uv venv
Using Python 3.12.3 interpreter at: /home/syrus/mambaforge/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

~/junk via 🐍 v3.12.3 (junk) on ☁️  (us-east-1)
❯ source .venv/bin/activate

~/junk via 🐍 v3.12.3 (junk) on ☁️  (us-east-1)
❯ uv pip list

~/junk via 🐍 v3.12.3 (junk) on ☁️  (us-east-1)
❯ uv pip install -v numpy
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.12.3 at `/home/syrus/junk/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: numpy
DEBUG Using registry request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: numpy*
DEBUG No cache entry for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/
DEBUG Checking netrc for credentials for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/
DEBUG Found credentials in netrc file for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/numpy/
DEBUG Searching for a compatible version of numpy (*)
DEBUG Selecting: numpy==1.26.4 (numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/packages/packages/0f/50/de23fde84e45f5c4fda2488c759b69990fd4512387a8632860f3ac9cd225/numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=675d61ffbfa78604709862923189bad94014bef562cc35cf61d3a07bba02a7ed
DEBUG Tried 2 versions: numpy 1, root 1
Resolved 1 package in 1.62s
DEBUG Identified uncached requirement: numpy==1.26.4
DEBUG No cache entry for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/packages/packages/0f/50/de23fde84e45f5c4fda2488c759b69990fd4512387a8632860f3ac9cd225/numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=675d61ffbfa78604709862923189bad94014bef562cc35cf61d3a07bba02a7ed
Downloaded 1 package in 7.27s
Installed 1 package in 9ms
 + numpy==1.26.4

~/junk via 🐍 v3.12.3 (junk) on ☁️  (us-east-1) took 8s
❯
```

---

_Comment by @zanieb on 2024-06-07 03:00_

I'd recommend taking a look at these similar issues

- https://github.com/astral-sh/uv/issues/1819
- https://github.com/astral-sh/uv/issues/2020

The general theme here is that we don't actually implement certificate loading so we're unlikely to be able to help you. Here's a portion of one of my previous comments:

> It may be worth seeing if there are any upstream [`rustls-native-certs`](https://github.com/rustls/rustls-native-certs) issues that apply to your situation and confirm that your certificates are available through the [`schannel`](https://docs.rs/schannel/0.1.23/schannel/cert_store/struct.CertStore.html) API.

---

_Comment by @zanieb on 2024-06-07 03:01_

You could also bisect the regression to a specific commit on uv, which would be very helpful for investigating the source of the issue.

---

_Comment by @Syrus-at-Philips on 2024-06-07 16:00_

I will try to identify a specific commit. Also, Schannel works fine for git-for-windows and for mamba-forge. I will take a look at the upstream rust-native-certs repo for related issues. Thank you!

---

_Comment by @Syrus-at-Philips on 2024-06-07 16:44_

@zanieb, this issue may be the culprit: https://github.com/rustls/rustls-native-certs/issues/22

Coupled with the upstream bug in loading the SSL certificate file (I also tried that), I seem to be blocked, but it looks like the upstream TLS package will resolve this in the long run. I will also try using the SSL certificate variable again. Because that may be needed.

---

_Comment by @Syrus-at-Philips on 2024-06-07 17:01_

I found more confusion when comparing running in Powershell 7 vs git bash. In git bash with SSL_CERT_FILE defined, `uv` is able to resolve the package list and select a package. It then fails on subsequent steps. I do believe this is an upstream issue, but to ensure `uv` works in a corporate environment, it will need to be resolved in the future. I'm going to close the issue, but I will paste the debug output under git-for-windows bash below.

```
(junk) (base)
320159535@YY291217 MINGW64 ~/junk
$ uv pip install -vv pytest
 uv_requirements::specification::from_source source=pytest
    0.002641s DEBUG uv_interpreter::discovery Searching for Python interpreter in virtual environments
    0.224274s DEBUG uv_interpreter::discovery Found CPython 3.11.9 at `C:/Users/320159535/junk/.venv\Scripts\python.exe` (active virtual environment)
    0.224463s DEBUG uv::commands::pip::install Using Python 3.11.9 environment at .venv\Scripts\python.exe
    0.224776s DEBUG uv_fs Acquired lock for `.venv`
    0.224996s DEBUG uv::commands::pip::install At least one requirement is not satisfied: pytest
 uv_client::linehaul::linehaul
    0.225277s DEBUG uv_client::base_client Using registry request timeout of 30s
 uv_resolver::flat_index::from_entries
 uv_resolver::resolver::solve
   uv_resolver::resolver::solve_tracked
      0.227133s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.11.9
     uv_resolver::resolver::choose_version package=root
     uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
       uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
          0.227574s   0ms DEBUG uv_resolver::resolver Adding direct dependency: pytest*
     uv_resolver::resolver::choose_version package=pytest
 uv_resolver::resolver::process_request request=Versions pytest
   uv_client::registry_client::simple_api package=pytest
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\320159535\AppData\Local\uv\cache\simple-v8\64de79a9d9e0bdeb\pytest.rkyv
 uv_resolver::resolver::process_request request=Prefetch pytest *
 uv_client::cached_client::from_path_sync path="\\\\?\\C:\\Users\\320159535\\AppData\\Local\\uv\\cache\\simple-v8\\64de79a9d9e0bdeb\\pytest.rkyv"
        0.228482s   0ms DEBUG uv_client::cached_client No cache entry for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/pytest/
       uv_client::cached_client::fresh_request url="https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/pytest/"
          0.885437s 656ms DEBUG uv_auth::middleware Checking netrc for credentials for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/pytest/
          0.886072s 657ms DEBUG uv_auth::middleware Found credentials in netrc file for https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/pytest/
       uv_client::cached_client::new_cache file=\\?\C:\Users\320159535\AppData\Local\uv\cache\simple-v8\64de79a9d9e0bdeb\pytest.rkyv
       uv_client::registry_client::parse_simple_api package=pytest
         uv_client::html::parse url=https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/simple/pytest/
   uv_resolver::version_map::from_metadata
        1.074814s 847ms DEBUG uv_resolver::resolver Searching for a compatible version of pytest (*)
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pytest==8.2.2
        1.075067s 847ms DEBUG uv_resolver::resolver Selecting: pytest==8.2.2 (pytest-8.2.2-py3-none-any.whl)
     uv_client::registry_client::wheel_metadata built_dist=pytest==8.2.2
     uv_resolver::resolver::get_dependencies_forking package=pytest, version=8.2.2
       uv_resolver::resolver::get_dependencies package=pytest, version=8.2.2
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\320159535\AppData\Local\uv\cache\wheels-v1\index\64de79a9d9e0bdeb\pytest\pytest-8.2.2-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="\\\\?\\C:\\Users\\320159535\\AppData\\Local\\uv\\cache\\wheels-v1\\index\\64de79a9d9e0bdeb\\pytest\\pytest-8.2.2-py3-none-any.msgpack"
            1.075528s   0ms DEBUG uv_client::cached_client No cache entry for: https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/packages/packages/4e/e7/81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23/pytest-8.2.2-py3-none-any.whl#sha256=c434598117762e2bd304e526244f67bf66bbd7b5d6cf22138be51ff661980343
           uv_client::cached_client::fresh_request url="https://biotelemetry.jfrog.io/artifactory/api/pypi/pypi/packages/packages/4e/e7/81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23/pytest-8.2.2-py3-none-any.whl#sha256=c434598117762e2bd304e526244f67bf66bbd7b5d6cf22138be51ff661980343"
           uv_client::cached_client::new_cache file=\\?\C:\Users\320159535\AppData\Local\uv\cache\wheels-v1\index\64de79a9d9e0bdeb\pytest\pytest-8.2.2-py3-none-any.msgpack
           uv_client::registry_client::read_metadata_range_request wheel=pytest-8.2.2-py3-none-any.whl
    1.760379s DEBUG uv_client::base_client Transient request failure for https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=91fa26a9a173fed4&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165710Z&X-Amz-SignedHeaders=host&X-Amz-Expires=59&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=b40598818fa14b5b9b7ef9ef2360885ae333f5058e0c01d30ce965675aa29fd8, retrying: Request error: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=91fa26a9a173fed4&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165710Z&X-Amz-SignedHeaders=host&X-Amz-Expires=59&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=b40598818fa14b5b9b7ef9ef2360885ae333f5058e0c01d30ce965675aa29fd8)
   Caused by: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=91fa26a9a173fed4&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165710Z&X-Amz-SignedHeaders=host&X-Amz-Expires=59&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=b40598818fa14b5b9b7ef9ef2360885ae333f5058e0c01d30ce965675aa29fd8)
   Caused by: client error (Connect)
   Caused by: invalid peer certificate: UnknownIssuer
    2.947735s DEBUG uv_client::base_client Transient request failure for https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=24d8d95139885844&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165711Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=67743e748a7fad03aff49da031c213a765c097708e6862a98e8439acd9740197, retrying: Request error: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=24d8d95139885844&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165711Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=67743e748a7fad03aff49da031c213a765c097708e6862a98e8439acd9740197)
   Caused by: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=24d8d95139885844&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165711Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=67743e748a7fad03aff49da031c213a765c097708e6862a98e8439acd9740197)
   Caused by: client error (Connect)
   Caused by: invalid peer certificate: UnknownIssuer
    3.930483s DEBUG uv_client::base_client Transient request failure for https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=1e51c819c26796ff&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165712Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=b85261f32aa142fad46e0d94a67bed46f20e96bba756cf55df9d60ef2b5a0554, retrying: Request error: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=1e51c819c26796ff&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165712Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=b85261f32aa142fad46e0d94a67bed46f20e96bba756cf55df9d60ef2b5a0554)
   Caused by: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=1e51c819c26796ff&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165712Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=b85261f32aa142fad46e0d94a67bed46f20e96bba756cf55df9d60ef2b5a0554)
   Caused by: client error (Connect)
   Caused by: invalid peer certificate: UnknownIssuer
    5.538647s DEBUG uv_client::base_client Transient request failure for https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=068038fb40625d69&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165714Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=a312aa448e6acf6e082954a2b783ea66dfa2bd1ec62307a18f7ac511f6feb669, retrying: Request error: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=068038fb40625d69&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165714Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=a312aa448e6acf6e082954a2b783ea66dfa2bd1ec62307a18f7ac511f6feb669)
   Caused by: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=068038fb40625d69&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165714Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=a312aa448e6acf6e082954a2b783ea66dfa2bd1ec62307a18f7ac511f6feb669)
   Caused by: client error (Connect)
   Caused by: invalid peer certificate: UnknownIssuer
error: Failed to download `pytest==8.2.2`
  Caused by: Failed to unzip wheel: pytest-8.2.2-py3-none-any.whl
  Caused by: an upstream reader returned an error: io error occurred: Request error: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=068038fb40625d69&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165714Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=a312aa448e6acf6e082954a2b783ea66dfa2bd1ec62307a18f7ac511f6feb669)
  Caused by: io error occurred: Request error: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=068038fb40625d69&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165714Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=a312aa448e6acf6e082954a2b783ea66dfa2bd1ec62307a18f7ac511f6feb669)
  Caused by: Request error: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=068038fb40625d69&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165714Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=a312aa448e6acf6e082954a2b783ea66dfa2bd1ec62307a18f7ac511f6feb669)
  Caused by: error sending request for url (https://jfrog-prod-usw1-shared-california-main.s3.amazonaws.com/aol-biotelemetry/filestore/3b/3b5de69d91711982a2521b699d8be804ab6026ac?X-Artifactory-username=syrus.nemat-nasser&X-Artifactory-repoType=local&X-Artifactory-repositoryKey=pypi-remote-cache&X-Artifactory-originPackageType=pypi&X-Artifactory-packageType=pypi&X-Artifactory-artifactPath=4e%2Fe7%2F81ebdd666d3bff6670d27349b5053605d83d55548e6bd5711f3b0ae7dd23%2Fpytest-8.2.2-py3-none-any.whl&X-Artifactory-originProjectKey=default&X-Artifactory-projectKey=default&X-Artifactory-originRepoType=remote&X-Artifactory-originRepositoryKey=pypi-remote&x-jf-traceId=068038fb40625d69&response-content-disposition=attachment%3Bfilename%3D%22pytest-8.2.2-py3-none-any.whl%22&response-content-type=application%2Foctet-stream&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMSJHMEUCIBy3JFvIASlmJNahkTXyhYl7dVdzruKwHj1vFwCwzw09AiEAhHcuXk2Wturr3HyAXeJbtGmKLZKUKBXGRf3LfXtUPUEqlwUI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgwxNTIxNTMwNjIxNDEiDJdEx6KcTwk%2F80Q5HirrBFwppRK%2BDyu9x%2F68JbrMG40b4buznH3rqjf8StTZhFEruiD6x%2Fh0vq3%2BmdQry6fYmGhQi8WOWx2WHI9RXDyYUzhljO2Z0kPIaM058L4FhImdb7fsrxNa1Pp%2FlZ3kgNYXoGLNtJnBw8tJmd0rE32WI1saloHPEuoQ74MHV6jY23siXveeHBHS99tdSKaYY56M%2BvjUvYfN8DjQvm3Q%2Fp%2FUbdcTyEsIk%2F2xYWUOBsGsZ6m3nPUQtoHDBsvBJ8ZJVGC%2FNXxfmZQ47uWiptWIzG23Juo2UEWAdi22D5lfOsSJ8mk%2Fw9jcDMNFZOWhnTwVr8pRub%2F5b0RgVul0C7ZHzkSbK04oWTrn63UQsoKZXEgH1JbFAGohO6RErmAUPMBtwYKLcuT4S5RilEoLmLHhHkyHxuACDtibopfKhpTsPtAd%2FYJRLuDfSNTbwxSMkMubsbzBVjAMIUwzOv%2Bnjuxv6isX1j%2BheQSFYTtgNSLXgg1QkZUyQqetekpzNm5jl%2FiNTIYsghuGAjPv27Dt5i7RrZmwEDa19jp8aL38bf%2F4nvsWhGQqn4e9tkM%2FTCIKswarfe0IxTB3sMUJeY8dQ01APM3k9jwAs4M8XAOXUKpaUWnrOQ6Iim3EbJlc81bn42kV1igFprQy4wOiq9UvttXnEkMifmMgsokVP1x%2BZDg1wYyIuZg1M6HmRwey6e9VeXx1stQHrtumKQXvbpXW3cNwmNSMeuXGlhKMreKMI0XCS%2BJqNU4O6qMOtWt5XlmOIYFxCHPysVvHYYiCWuzse5aHnN3IJmMZO1JZzmPMu6B4fo%2FsAupCOvL1A%2B8taoekiaIwvu%2BMswY6mwF6T8cmpYSVkeZKc5q2pOidCnx11rUqE5QoIgOANCr0gUhccefbyKL9MIg%2BPQTLdo%2BuzZpECxNsFmMO6rCVLyvHxpVHvZK64yGqgnWbBEOQ9r%2B83iKk%2BEyOrnT4eYZ078Poce11vn%2FeRZTOnP%2BlEOXm6y1spRbWuM8crcgcqZRRKsR92Pcicppz%2FtGlnziA5FwvRfYg6iklNF4H7g%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T165714Z&X-Amz-SignedHeaders=host&X-Amz-Expires=60&X-Amz-Credential=ASIASG3IHPL6SDTLRGUV%2F20240607%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=a312aa448e6acf6e082954a2b783ea66dfa2bd1ec62307a18f7ac511f6feb669)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
(junk) (base)
320159535@YY291217 MINGW64 ~/junk
$
```

---

_Closed by @Syrus-at-Philips on 2024-06-07 17:01_

---

_Comment by @zanieb on 2024-06-07 17:06_

Thank you so much for doing this investigation!

---

_Label `upstream` added by @zanieb on 2024-06-07 17:06_

---

_Comment by @gdebrun2 on 2024-12-12 23:37_

Bumping since the devs of ```rustls-native-certs``` don't plan to fix this in favor of using ```rustls-platform-verifier``` per [rustls-native-certs#133](https://github.com/rustls/rustls-native-certs/issues/131#issuecomment-2356368045) 

---

_Comment by @zanieb on 2024-12-12 23:40_

@gdebrun2 Can you open a new specific issue with a request instead of bumping?

---
