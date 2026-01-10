```yaml
number: 5141
title: Add GraalPy support
type: pull_request
state: merged
author: timfel
labels:
  - enhancement
assignees: []
merged: true
base: main
head: tim/graalpy
created_at: 2024-07-17T08:48:47Z
updated_at: 2024-07-20T08:54:33Z
url: https://github.com/astral-sh/uv/pull/5141
synced_at: 2026-01-10T13:42:52Z
```

# Add GraalPy support

---

_Pull request opened by @timfel on 2024-07-17 08:48_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Currently, `uv` refuses to install anything on GraalPy. This is currently blocking GraalPy testing with cibuildwheel, since manylinux includes both `uv` and `graalpy` (but doesn't test with `uv`), whereas cibuildwheel defaults to `uv`. See e.g. https://github.com/pypa/cibuildwheel/actions/runs/9956369360/job/27506182952?pr=1538 where it gives
```
      + python -m build /project/sample_proj --wheel --outdir=/tmp/cibuildwheel/built_wheel --installer=uv
  * Creating isolated environment: venv+uv...
  * Using external uv from /usr/local/bin/uv
  * Installing packages in isolated environment:
    - setuptools >= 40.8.0
  > /usr/local/bin/uv pip install "setuptools >= 40.8.0"
  < error: Unknown implementation: `graalpy`
```

## Test Plan

I simply based the GraalPy support on PyPy and added some small tests. I'm open to discussing how to test this. GraalPy is available for manylinux images and with setup-python, so we should be able to add tests against it to the CI. I locally confirmed by installing `uv` into a GraalPy venv and then trying things like `uv pip install Pillow` and testing those extensions.


---

_Label `enhancement` added by @zanieb on 2024-07-17 17:30_

---

_Assigned to @zanieb by @zanieb on 2024-07-17 17:30_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/bare.rs`:184 on 2024-07-17 17:32_

Do we need do make a change on Windows too?

---

_@zanieb reviewed on 2024-07-17 17:32_

---

_Comment by @zanieb on 2024-07-17 17:33_

Thanks for contributing! Welcome to the project :)

Could you add a CI integration test in the style of the PyPy one?

---

_@timfel reviewed on 2024-07-18 07:56_

---

_Review comment by @timfel on `crates/uv-virtualenv/src/bare.rs`:184 on 2024-07-18 07:56_

Yes, added.

---

_Comment by @timfel on 2024-07-18 07:59_

> Could you add a CI integration test in the style of the PyPy one?

Added. There's a bug with GraalPy 24.0 on Linux around parsing the `home` property in pyvenv.cfg that we hadn't noticed so far because the `venv` and `virtualenv` modules seem to be doing something different. I fixed this in GraalPy master already and the fix is due out in the first week of September. Until then I added a workaround to the CI job for GraalPy on Linux.

---

_@timfel reviewed on 2024-07-18 07:59_

---

_Review comment by @timfel on `.github/workflows/ci.yml`:770 on 2024-07-18 07:59_

PR to setup-python is open: https://github.com/actions/setup-python/pull/880

---

_Review comment by @timfel on `.github/workflows/ci.yml`:717 on 2024-07-18 08:00_

Workaround for 24.0 bug (fixed in 24.1, due out in September)

---

_@timfel reviewed on 2024-07-18 08:00_

---

_Comment by @zanieb on 2024-07-18 14:42_

Looks like it's failing on Windows: https://github.com/astral-sh/uv/actions/runs/9987705348/job/27602873156?pr=5141

---

_@zanieb approved on 2024-07-18 14:48_

Looks good to me once it's passing CI.

---

_Merged by @zanieb on 2024-07-19 00:28_

---

_Closed by @zanieb on 2024-07-19 00:28_

---

_Branch deleted on 2024-07-19 03:31_

---

_Comment by @mayeut on 2024-07-20 08:30_

@timfel, this does not seem to work:
```
[root@78fc86164bc0 /]# uv -V
uv 0.2.27
[root@78fc86164bc0 /]# uv venv --python /opt/python/graalpy310-graalpy240_310_native/bin/python /tmp/uv-test-graalpy3.10
Using Python 3.10.13 interpreter at: opt/python/graalpy310-graalpy240_310_native/bin/python
Creating virtualenv at: tmp/uv-test-graalpy3.10
[root@78fc86164bc0 /]# /tmp/uv-test-graalpy3.10/bin/python -V
[To redirect Truffle log output to a file use one of the following options:
* '--log.file=<path>' if the option is passed using a guest language launcher.
* '-Dpolyglot.log.file=<path>' if the option is passed using the host Java launcher.
* Configure logging using the polyglot embedding API.]
[python::PythonContext] WARNING: could not determine Graal.Python's standard library path. You need to pass --python.StdLibHome if you want to use the standard library.
[python::PythonContext] WARNING: could not determine Graal.Python's core path - you may need to pass --python.CoreHome.
[python::PythonContext] WARNING: could not determine Graal.Python's C API library path. You need to pass --python.CAPI if you want to use the C extension modules.
[python::PythonContext] WARNING: could not determine Graal.Python's C API library path. You need to pass --python.CAPI if you want to use the C extension modules.
ERROR: java.lang.UnsupportedOperationException: Could not load posix support library from path '/lib/graalpy24.0/libposix.so'. Troubleshooting: 
Check permissions of the file.
Missing runtime Maven dependency 'org.graalvm.truffle:truffle-nfi-libffi' (should be a dependency of `org.graalvm.polyglot:python{-community}`)?
org.graalvm.polyglot.PolyglotException: java.lang.UnsupportedOperationException: Could not load posix support library from path '/lib/graalpy24.0/libposix.so'. Troubleshooting: 
Check permissions of the file.
Missing runtime Maven dependency 'org.graalvm.truffle:truffle-nfi-libffi' (should be a dependency of `org.graalvm.polyglot:python{-community}`)?
	at com.oracle.graal.python.runtime.NFIPosixSupport$InvokeNativeFunction.loadLibrary(NFIPosixSupport.java:394)
	at com.oracle.graal.python.runtime.NFIPosixSupport$InvokeNativeFunction.call(NFIPosixSupport.java:330)
	at com.oracle.graal.python.runtime.NFIPosixSupport$InvokeNativeFunction.callLong(NFIPosixSupport.java:345)
	at com.oracle.graal.python.runtime.NFIPosixSupport.lseek(NFIPosixSupport.java:653)
	at com.oracle.graal.python.runtime.NFIPosixSupportGen$PosixSupportLibraryExports$Uncached.lseek(NFIPosixSupportGen.java:6569)
	at com.oracle.graal.python.runtime.ImageBuildtimePosixSupport.lseek(ImageBuildtimePosixSupport.java:261)
	at com.oracle.graal.python.runtime.ImageBuildtimePosixSupportGen$PosixSupportLibraryExports$Uncached.lseek(ImageBuildtimePosixSupportGen.java:2575)
	at com.oracle.graal.python.runtime.PosixSupportLibraryGen$UncachedDispatch.lseek(PosixSupportLibraryGen.java:7580)
	at com.oracle.graal.python.builtins.modules.io.FileIOBuiltins$SeekNode.internalSeek(FileIOBuiltins.java:812)
	at com.oracle.graal.python.builtins.modules.io.FileIOBuiltins$TellNode.internalTell(FileIOBuiltins.java:837)
	at com.oracle.graal.python.builtins.modules.io.AbstractBufferedIOBuiltins$BufferedInitNode.internalInit(AbstractBufferedIOBuiltins.java:120)
	at com.oracle.graal.python.builtins.modules.io.BufferedReaderBuiltins$BufferedReaderInit.internalInit(BufferedReaderBuiltins.java:103)
	at com.oracle.graal.python.builtins.modules.SysModuleBuiltins.initStd(SysModuleBuiltins.java:726)
	at com.oracle.graal.python.builtins.modules.SysModuleBuiltins.postInitialize(SysModuleBuiltins.java:710)
	at com.oracle.graal.python.builtins.Python3Core.postInitialize(Python3Core.java:980)
	at com.oracle.graal.python.runtime.PythonContext.patch(PythonContext.java:1485)
	at com.oracle.graal.python.PythonLanguage.patchContext(PythonLanguage.java:420)
	at com.oracle.graal.python.PythonLanguage.patchContext(PythonLanguage.java:142)
	at org.graalvm.truffle/com.oracle.truffle.api.LanguageAccessor$LanguageImpl.patchEnvContext(LanguageAccessor.java:431)
	at org.graalvm.truffle/com.oracle.truffle.polyglot.PolyglotLanguageContext.patch(PolyglotLanguageContext.java:893)
	at org.graalvm.truffle/com.oracle.truffle.polyglot.PolyglotContextImpl.patch(PolyglotContextImpl.java:3490)
	at org.graalvm.truffle/com.oracle.truffle.polyglot.PolyglotEngineImpl.loadPreinitializedContext(PolyglotEngineImpl.java:1929)
	at org.graalvm.truffle/com.oracle.truffle.polyglot.PolyglotEngineImpl.createContext(PolyglotEngineImpl.java:1809)
	at org.graalvm.truffle/com.oracle.truffle.polyglot.PolyglotEngineDispatch.createContext(PolyglotEngineDispatch.java:152)
	at org.graalvm.polyglot/org.graalvm.polyglot.Context$Builder.build(Context.java:1937)
	at com.oracle.graal.python.shell.GraalPythonMain.launch(GraalPythonMain.java:739)
	at com.oracle.graal.python.enterprise.shell.GraalPythonEnterpriseMain.launch(GraalPythonEnterpriseMain.java:53)
	at org.graalvm.launcher.AbstractLanguageLauncher.launch(AbstractLanguageLauncher.java:296)
	at org.graalvm.launcher.AbstractLanguageLauncher.launch(AbstractLanguageLauncher.java:121)
	at org.graalvm.launcher.AbstractLanguageLauncher.runLauncher(AbstractLanguageLauncher.java:168)
	Suppressed: Attached Guest Language Frames (0)
Internal GraalVM error, please report at https://github.com/oracle/graal/issues/.
[root@78fc86164bc0 /]# /opt/python/graalpy310-graalpy240_310_native/bin/python -m venv /tmp/venv-test-graalpy3.10
[root@78fc86164bc0 /]# /tmp/venv-test-graalpy3.10/bin/python -V
GraalPy 3.10.13 (Oracle GraalVM Native 24.0.2)
[root@78fc86164bc0 /]# cat /tmp/uv-test-graalpy3.10/pyvenv.cfg 
home = /opt/_internal/graalpy310-graalpy240_310_native/bin
implementation = GraalVM
uv = 0.2.27
version_info = 3.10.13
include-system-site-packages = false
[root@78fc86164bc0 /]# cat /tmp/venv-test-graalpy3.10/pyvenv.cfg 
home = /opt/_internal/graalpy310-graalpy240_310_native
include-system-site-packages = false
version = 3.10.13
``` 
Removing `/bin` in pyvenv.cfg works but seems like a workaround specific to GraalPy ?

---

_Comment by @timfel on 2024-07-20 08:54_

> @timfel, this does not seem to work:

Yeah, there's a workaround in the CI config I commented on above in this PR, the fix is in GraalPy master and will be in the next release in September. Until then we have to work around with either your suggestion or setting GRAAL_PYTHONHOME like I do in the CI here

---
