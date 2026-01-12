```yaml
number: 11089
title: uv can not find a package for an existing version
type: issue
state: open
author: mneumei
labels:
  - needs-mre
assignees: []
created_at: 2025-01-30T08:03:50Z
updated_at: 2025-02-24T07:39:31Z
url: https://github.com/astral-sh/uv/issues/11089
synced_at: 2026-01-12T16:00:27Z
```

# uv can not find a package for an existing version

---

_@mneumei_

### Summary

In our corporate index, we have a package xxx with the same name as a package in pypi.
We use a virtual index in artifactory to resolve the packages, and when i query the simple index, it will find all packages (both from pypi and our local index).
It looks something like this, where the smaller version is in pypi, the higher in the local index:
xxx-0.1.tar.gz xxx-24.25.0-py3-none-any.whl xxx-24.25.0.tar.gz

When i run uv (even with no cache and index-strategy unsafe-best-match), it will not find the package at all. When running straight with pip, i have no issues at all. When i click the link to the index, it will show me the full list like above.

```
C:\Work\my-app>uv pip install xxx==24.25.0 -vvv --index-strategy unsafe-best-match --no-cache
    0.000025s DEBUG uv uv 0.5.9 (0652800cb 2024-12-13)
 uv_requirements::specification::from_source source=xxx==24.25.0
    0.008219s DEBUG uv_python::discovery Searching for default Python interpreter in virtual environments
    0.163261s DEBUG uv_python::discovery Found `cpython-3.11.11-windows-x86_64-none` at `C:\Work\my-app\.venv\Scripts\python.exe` (active virtual environment)
    0.163867s DEBUG uv::commands::pip::operations Using Python 3.11.11 environment at: .venv
    0.165121s DEBUG uv_fs Acquired lock for `.venv`
    0.169164s DEBUG uv::commands::pip::install At least one requirement is not satisfied: xxx==24.25.0
 uv_client::linehaul::linehaul
    0.170651s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
 uv_resolver::resolver::solve
    0.172669s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.11.11
    0.172901s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.11.11
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.173616s   1ms DEBUG uv_resolver::resolver Adding direct dependency: xxx>=24.25.0, <24.25.0+
   uv_resolver::resolver::choose_version package=xxx
 uv_resolver::resolver::process_request request=Versions xxx
   uv_client::registry_client::simple_api package=xxx
     uv_client::cached_client::get_cacheable_with_retry
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myuser\AppData\Local\Temp\.tmpUtlUAn\simple-v14\index\2669b11cd426ef45\xxx.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myuser\\AppData\\Local\\Temp\\.tmpUtlUAn\\simple-v14\\index\\2669b11cd426ef45\\xxx.rkyv"
 uv_resolver::resolver::process_request request=Prefetch xxx>=24.25.0, <24.25.0+
          0.175016s   0ms DEBUG uv_client::cached_client No cache entry for: https://my-company.com/artifactory/api/pypi/my-repo-pypi-virtual/simple/xxx/
         uv_client::cached_client::fresh_request url="https://my-company.com/artifactory/api/pypi/my-repo-pypi-virtual/simple/xxx/"
         uv_client::cached_client::new_cache file=C:\Users\myuser\AppData\Local\Temp\.tmpUtlUAn\simple-v14\index\2669b11cd426ef45\xxx.rkyv
         uv_client::registry_client::parse_simple_api package=xxx
           uv_client::html::parse url=https://my-company.com/artifactory/api/pypi/my-repo-pypi-virtual/simple/xxx/
   uv_resolver::version_map::from_metadata
      0.331855s 158ms DEBUG uv_resolver::resolver Searching for a compatible version of xxx(>=24.25.0, <24.25.0+)
    0.331968s 159ms DEBUG uv_resolver::resolver No compatible version found for: xxx
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of xxx==24.25.0 and you require xxx==24.25.0, we can conclude that your requirements are unsatisfiable.
    0.334206s DEBUG uv_fs Released lock at `C:\Work\my-app\.venv\.lock`
```

### Platform

Win11 64-bit operating system, x64-based processor

### Version

uv 0.5.9 (0652800cb 2024-12-13)

### Python version

Python 3.11.11

---

_Label `bug` added by @mneumei on 2025-01-30 08:03_

---

_Comment by @konstin on 2025-01-30 11:11_

Can you try with the latest uv version and check if the error still occurs?

---

_Comment by @mneumei on 2025-01-30 12:44_

0.000030s DEBUG uv uv 0.5.25 (9c07c3fc5 2025-01-28) yes, same behavior

---

_Comment by @charliermarsh on 2025-01-30 14:12_

It's hard to know what's happening without seeing the HTML response from https://my-company.com/artifactory/api/pypi/my-repo-pypi-virtual/simple/xxx/. Can you share a redacted snippet of that HTML?

---

_Label `bug` removed by @charliermarsh on 2025-01-30 14:12_

---

_Label `needs-mre` added by @charliermarsh on 2025-01-30 14:12_

---

_Comment by @mneumei on 2025-01-30 14:25_

xxx-0.1.tar.gz xxx-0.3.0-cp39-cp39-macosx_12_0_arm64.whl xxx-0.3.0.tar.gz xxx-23.29.0-py3-none-any.whl xxx-23.29.0.tar.gz xxx-23.3.0-py3-none-any.whl xxx-23.3.0.tar.gz xxx-24.25.0-py3-none-any.whl xxx-24.25.0.tar.gz

This is how it would look like. As you might already have guessed, i replace the package name as i´d rather not have the package name documented here.

---

_Comment by @charliermarsh on 2025-01-30 14:27_

Sorry, that's not quite what I mean. I need to see the actual HTML that we're parsing from the page. Presumedly there's something strange going on the structure of the page that's causing us to miss the packages.

---

_Comment by @mneumei on 2025-01-30 14:33_

"""
<!DOCTYPE html>
    <html><head><title>Simple Index</title><meta name="api-version" value="2" /></head><body>
    <a href="../../packages/packages/ab/ab/somehex/xxx-0.1.tar.gz#sha256=somesha" rel="internal">xxx-0.1.tar.gz</a>
    <a href="../../packages/packages/ab/ab/somehex/xxx-0.3.0-cp39-cp39-macosx_12_0_arm64.whl#sha256=somesha" rel="internal">xxx-0.3.0-cp39-cp39-macosx_12_0_arm64.whl</a>
    <a href="../../packages/packages/ab/ab/somehex/xxx-0.3.0.tar.gz#sha256=somesha" rel="internal">xxx-0.3.0.tar.gz</a>
    <a href="../../packages/xxx/23.29.0/xxx-23.29.0-py3-none-any.whl#sha256=somesha" data-requires-python="&gt;=3.7" rel="internal">xxx-23.29.0-py3-none-any.whl</a>
    <a href="../../packages/xxx/23.29.0/xxx-23.29.0.tar.gz#sha256=somesha" data-requires-python="&gt;=3.7" rel="internal">xxx-23.29.0.tar.gz</a>
    <a href="../../packages/xxx/23.3.0/xxx-23.3.0-py3-none-any.whl#sha256=somesha" data-requires-python="&lt;3.10,&gt;=3.7" rel="internal">xxx-23.3.0-py3-none-any.whl</a>
    <a href="../../packages/xxx/23.3.0/xxx-23.3.0.tar.gz#sha256=somesha" data-requires-python="&lt;3.10,&gt;=3.7" rel="internal">xxx-23.3.0.tar.gz</a>
    <a href="../../packages/xxx/24.25.0/xxx-24.25.0-py3-none-any.whl#sha256=somesha" data-requires-python="&gt;=3.7" rel="internal">xxx-24.25.0-py3-none-any.whl</a>
    <a href="../../packages/xxx/24.25.0/xxx-24.25.0.tar.gz#sha256=somesha" data-requires-python="&gt;=3.7" rel="internal">xxx-24.25.0.tar.gz</a>
</body></html>
"""

I hope that one fits better

---

_Comment by @zanieb on 2025-01-30 15:28_

You need to do use code blocks, e.g., visit view-source:pypi.org/simple/anyio/ (for your index) and share with ```

```html
<!DOCTYPE html>
  | <html>
  | <head>
  | <meta name="pypi:repository-version" content="1.3">
  | <title>Links for anyio</title>
  | </head>
  | <body>
  | <h1>Links for anyio</h1>
  | <a href="https://files.pythonhosted.org/packages/d1/51/27bdba2ca663be87b1d7409da926df79fd715a25faa7fa577f0b5e77c903/anyio-1.0.0a1-py3-none-any.whl#sha256=ff9972164eb84d62ed3e56267c873918ebfed7da26f4ed739355017ea05ef7da" data-requires-python="&gt;= 3.5.3" data-dist-info-metadata="sha256=fc865a829782998cadff8a94fb241eb701662a5f718e1f67e02b8292abf7edc0" data-core-metadata="sha256=fc865a829782998cadff8a94fb241eb701662a5f718e1f67e02b8292abf7edc0">anyio-1.0.0a1-py3-none-any.whl</a><br />
  | <a href="https://files.pythonhosted.org/packages/db/20/1fd39e27cde5d2e3f3c48e4e798ee52440bfa090a1a449a0b0eae9ef42bd/anyio-1.0.0a1.tar.gz#sha256=6afd26907514e46cc57727adee4555008617402525dd6c7b83b994ff1bd7bdea" data-requires-python="&gt;= 3.5.3" >anyio-1.0.0a1.tar.gz</a><br />
  | <a href="https://files.pythonhosted.org/packages/c6/fc/32947a4760f6096e2f1ffd4baefa8d49d91c4d0ffe2be7b2726cce5fa11e/anyio-1.0.0a2-py3-none-any.whl#sha256=65d7b8f977d78d6042d70dfb73a38015456b557cf8f3d97314f932e15b317d0c" data-requires-python="&gt;= 3.5.3" data-dist-info-metadata="sha256=68eef81ae791cc2060a15e6055f18bc889a12c41a4e591c32031d75fcc9df75e" data-core-metadata="sha256=68eef81ae791cc2060a15e6055f18bc889a12c41a4e591c32031d75fcc9df75e">anyio-1.0.0a2-py3-none-any.whl</a><br />
  | <a href="https://files.pythonhosted.org/packages/33/34/adffc1e83c6f2c3f01cbbbd4649d87d293816d5e5d5f53a78e937e9ad4df/anyio-1.0.0a2.tar.gz#sha256=b070af68ef5f2497541d664f93e4bc0c67898090b368ae5fa897b1ea01d51ba6" data-requires-python="&gt;= 3.5.3" >anyio-1.0.0a2.tar.gz</a><br />
  | <a href="https://files.pythonhosted.org/packages/ed/27/e1e2095985829626a46d2d7dea4888b336547c691006723adb7567dc680e/anyio-1.0.0b1-py3-none-any.whl#sha256=9bd4a9ec060d6fe275d26a686a661fde2aa1306d2a7142a012c2cf2bb73a7eed" data-requires-python="&gt;= 3.5.3" data-dist-info-metadata="sha256=bd1fcd768c390b177fdb4ade4ddf87048b8c0fe232f787d3a2b36942fe31be58" data-core-metadata="sha256=bd1fcd768c390b177fdb4ade4ddf87048b8c0fe232f787d3a2b36942fe31be58">anyio-1.0.0b1-py3-none-any.whl</a><br />
  | <a href="https://files.pythonhosted.org/packages/a9/41/c3ed77c957c9eb1c4b843ba24d76441d8f848cc74a4cc87b18477fd0cd3c/anyio-1.0.0b1.tar.gz#sha256=82f179093163e6004fd3a7a41c0ac68d325a323fd58989764f979553ef30e913" data-requires-python="&gt;= 3.5.3" >anyio-1.0.0b1.tar.gz</a><br />
  | <a href="https://files.pythonhosted.org/packages/7f/ef/62a85666466587a31f093f1c2c85463528552a7e51b1609d27587c76a4ab/anyio-1.0.0b2-py3-none-any.whl#sha256=7f94a6f023765ace95afca9c728dbc3443a2c2a77a9b0824341365d0472bf7ea" data-requires-python="&gt;= 3.5.3" data-dist-info-metadata="sha256=346d6137b42486767a8620611a047f0cdfd1b7f45fb0d683e0fa2d0cf6b24369" data-core-metadata="sha256=346d6137b42486767a8620611a047f0cdfd1b7f45fb0d683e0fa2d0cf6b24369">anyio-1.0.0b2-py3-none-any.whl</a><br />
  | <a href="https://files.pythonhosted.org/packages/17/bc/35e0b2ffb1d0fc1bcbf1164792c3b0d18c4634f6440a30ebb5acf08163ae/anyio-1.0.0b2.tar.gz#sha256=a749204790705e7ef74d880068b07a11d094e1994d3f59febd2d232066af2d03" data-requires-python="&gt;= 3.5.3" >anyio-1.0.0b2.tar.gz</a><br />
  | <a href="https://files.pythonhosted.org/packages/40/1b/614827dc317cce9ba53818d5a2f77710496ae672a63b504b64de36f7d466/anyio-1.0.0rc1-py3-none-any.whl#sha256=292d3f47a45c7d7fc2c3f128eecccc065f0ac44db2c73e06c0c5c711d15f7904" data-requires-python="&gt;= 3.5.3" data-dist-info-metadata="sha256=bb5754450a205939f42b9971b4685cc65eb1a65368555b6c6393dc13fe04510d" data-core-metadata="sha256=bb5754450a205939f42b9971b4685cc65eb1a65368555b6c6393dc13fe04510d">anyio-1.0.0rc1-py3-none-any.whl</a><br />
  | <a href="https://files.pythonhosted.org/packages/fa/d7/e3a8a278bc611672599e2be4c174ea60bb153b6bf44afa16fe5548ea74cf/anyio-1.0.0rc1.tar.gz#sha256=5d0cc20ed4e84b8d9c355eef368cfd62a8daf7f14e2b22f02abb1b7628df28ce" data-requires-python="&gt;= 3.5.3" >anyio-1.0.0rc1.tar.gz</a><br />
 

<br class="Apple-interchange-newline">
...
```

---

_Comment by @mneumei on 2025-02-24 07:39_

Is the information i updated better now?

---
