---
number: 7450
title: Switching from poetry to uv breaks CI
type: issue
state: closed
author: aeroaks
labels: []
assignees: []
created_at: 2024-09-17T07:34:48Z
updated_at: 2024-09-17T08:33:50Z
url: https://github.com/astral-sh/uv/issues/7450
synced_at: 2026-01-07T13:12:17-06:00
---

# Switching from poetry to uv breaks CI

---

_Issue opened by @aeroaks on 2024-09-17 07:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I am switching from `poetry `to `uv`. I created the `project `successfully in my Windows laptop. I am trying to update my CI pipeline with `uv`.
uv version is:
```
$ uv --version
uv 0.4.9
```
When I run `sync` command, I get the following output and the build fails:
```
$ uv sync --dev
Using Python 3.10.11
Creating virtualenv at: .venv
Resolved 100 packages in 1ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: statsmodels==0.14.3
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_py
creating build/lib.linux-x86_64-cpython-310/statsmodels
copying statsmodels/_version.py -> build/lib.linux-x86_64-cpython-310/statsmodels
copying statsmodels/__init__.py -> build/lib.linux-x86_64-cpython-310/statsmodels
copying statsmodels/api.py -> build/lib.linux-x86_64-cpython-310/statsmodels
copying statsmodels/conftest.py -> build/lib.linux-x86_64-cpython-310/statsmodels
creating build/lib.linux-x86_64-cpython-310/statsmodels/othermod
copying statsmodels/othermod/__init__.py -> build/lib.linux-x86_64-cpython-310/statsmodels/othermod
copying statsmodels/othermod/api.py -> build/lib.linux-x86_64-cpython-310/statsmodels/othermod
copying statsmodels/othermod/betareg.py -> build/lib.linux-x86_64-cpython-310/statsmodels/othermod
creating build/lib.linux-x86_64-cpython-310/statsmodels/src
copying statsmodels/src/__init__.py -> build/lib.linux-x86_64-cpython-310/statsmodels/src
tests/results
...
creating build/lib.linux-x86_64-cpython-310/statsmodels/tsa/arima
copying statsmodels/tsa/arima/params.py -> build/lib.linux-x86_64-cpython-310/statsmodels/tsa/arima
copying statsmodels/tsa/arima/__init__.py -> build/lib.linux-x86_64-cpython-310/statsmodels/tsa/arima
copying statsmodels/tsa/arima/tools.py -> build/lib.linux-x86_64-cpython-310/statsmodels/tsa/arima
copying statsmodels/miscmodels/tests/results/ologit_ucla.csv -> build/lib.linux-x86_64-cpython-310/statsmodels/miscmodels/tests/results
copying statsmodels/treatment/tests/results/cataneo2.csv -> build/lib.linux-x86_64-cpython-310/statsmodels/treatment/tests/results
running build_ext
building 'statsmodels.tsa.stl._stl' extension
creating build/temp.linux-x86_64-cpython-310/statsmodels/tsa/stl
clang -pthread -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -fPIC -I/tools/deps/include -I/tools/deps/include/ncursesw -I/tools/deps/libedit/include -fPIC -DCYTHON_TRACE_NOGIL=0 -DNPY_NO_DEPRECATED_API=NPY_1_7_API_VERSION -I/root/.cache/uv/builds-v0/.tmphV1MXd/lib/python3.10/site-packages/numpy/_core/include -I/root/.cache/uv/builds-v0/.tmphV1MXd/include -I/root/.local/share/uv/python/cpython-3.10.11-linux-x86_64-gnu/include/python3.10 -c statsmodels/tsa/stl/_stl.c -o build/temp.linux-x86_64-cpython-310/statsmodels/tsa/stl/_stl.o
--- stderr:
error: command 'clang' failed: No such file or directory
---
```

The CI pipeline was working nicely with `poetry`.

Do I need to install clang separately?

---

_Comment by @aeroaks on 2024-09-17 07:49_

I get the following information from the `uv.lock` file:
```
[[package]]
name = "statsmodels"
version = "0.14.3"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "numpy" },
    { name = "packaging" },
    { name = "pandas" },
    { name = "patsy" },
    { name = "scipy" },
]
sdist = { url = "https://files.pythonhosted.org/packages/05/d0/cf49ceca73f459b9b848ca2ce2a884ac8823284bc8b78aec161417d7bd78/statsmodels-0.14.3.tar.gz", hash = "sha256:ecf3502643fa93aabe5f0bdf238efb59609517c4d60a811632d31fcdce86c2d2", size = 20354488 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/f7/f3/fc3038fdd8e08f8705f5186dd39e63bc2f84a7d6ed4fc35f6e4f1fca9046/statsmodels-0.14.3-cp310-cp310-macosx_10_9_x86_64.whl", hash = "sha256:7372c92f18b8afb06355e067285abb94e8b214afd9f2fda6d3c26f3ea004cbdf", size = 10215249 },
    { url = "https://files.pythonhosted.org/packages/21/df/38fceddfe961b8fca531b2ee62606c0293742f0f6c9fd7415599814dd9c5/statsmodels-0.14.3-cp310-cp310-macosx_11_0_arm64.whl", hash = "sha256:42459cdaafe217f455e6b95c05d9e089caf02dd53295aebe63bc1e0206f83176", size = 9911102 },
    { url = "https://files.pythonhosted.org/packages/cf/87/cbfd4951472978ffe9b100c0c714298393be9c05366430f2eba562aec6ca/statsmodels-0.14.3-cp310-cp310-win_amd64.whl", hash = "sha256:53212f597747534bed475bbd89f4bc39a3757c20692bb7664021e30fbd967c53", size = 9844446 },
    { url = "https://files.pythonhosted.org/packages/1a/15/6e4a7d4b245d858b69ef3c6442912936e5520e6f5c2ab3fb5e5c68af1e0d/statsmodels-0.14.3-cp311-cp311-macosx_10_9_x86_64.whl", hash = "sha256:e49a63757e12269ef02841f05906e91bdb70f5bc358cbaca97f171f4a4de09c4", size = 10219695 },
    { url = "https://files.pythonhosted.org/packages/ce/24/16f9ea64dcbbed15fa1c3eb6122fdf2be250503f3d950809b3fa6f6a1c4e/statsmodels-0.14.3-cp311-cp311-macosx_11_0_arm64.whl", hash = "sha256:de4b989f0fea684f89bdf5ff641f9acb7acddfd712459f28365904a974afaeff", size = 9910887 },
    { url = "https://files.pythonhosted.org/packages/06/d5/fd9356fe3827621e3b0b579c1a34c803596fe6a98032d30ff584da1af6cf/statsmodels-0.14.3-cp311-cp311-win_amd64.whl", hash = "sha256:efb946ced8243923eb78909834699be55442172cea3dc37158e3e1c5370e4189", size = 9851931 },
    { url = "https://files.pythonhosted.org/packages/39/2e/663d64a5475fd842a4476bdfdd1b9ce8b58b829fb34f6fbdca88496d59ee/statsmodels-0.14.3-cp312-cp312-macosx_10_9_x86_64.whl", hash = "sha256:9bf3690f71ebacff0c976c1584994174bc1bb72785b5a35645b385a00a5107e0", size = 10217800 },
    { url = "https://files.pythonhosted.org/packages/65/b0/20a9dc57d4507fd93524c5e03d4bdf01e3e61574bf689fd0810bd9dcf5eb/statsmodels-0.14.3-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:197bcb1aeaaa5c7e9ba4ad87c2369f9600c6cd69d6e2db829eb46d3d9fe534c9", size = 9910951 },
    { url = "https://files.pythonhosted.org/packages/33/35/46c3dcd04bb6813e766ad209ac35ab6fe30d3cb426a6ce47be0b8748a1f5/statsmodels-0.14.3-cp312-cp312-win_amd64.whl", hash = "sha256:5724e51a370227655679f1a487f429919f03de325d7b5702e919526353d0cb1d", size = 9822533 },
]
```
looks like `manylinux` wheels are not listed. `manylinux` wheels are listed in poetry's lock file.

---

_Comment by @aeroaks on 2024-09-17 08:32_

#7347 helped. Used [build isolation](https://docs.astral.sh/uv/concepts/projects/#build-isolation) docs. Closing the issue.

---

_Closed by @aeroaks on 2024-09-17 08:32_

---
