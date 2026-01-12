```yaml
number: 7373
title: uv not detecting python in termux
type: issue
state: closed
author: ghost
labels:
  - bug
  - uv python
assignees: []
created_at: 2024-09-13T18:56:42Z
updated_at: 2025-11-21T23:12:26Z
url: https://github.com/astral-sh/uv/issues/7373
synced_at: 2026-01-12T15:59:13Z
```

# uv not detecting python in termux

---

_@ghost_

Disclaimer: I used AI to help write this report because English is not my first language, I apologize if anything is unclear or too detailed. Let me know if you need any clarifications

I'm encountering an issue when trying to use `uv` on Termux. When attempting to install packages using `uv pip install`, I receive an error indicating that no virtual environment or system Python installation is found, despite Python being installed and accessible.

## Environment
- OS: Termux (Android)
- Python version: 3.11.9
- UV version: 0.4.10
- Termux version: 0.119.0-b1+monet36
- Android version: 14
- Device: vivo V2206
- CPU architecture: aarch64
- Kernel: Linux 4.19.157-perf+

## Detailed Termux Information
```
Termux Variables:
TERMUX_APP_PACKAGE_MANAGER=apt
TERMUX_APP__APK_FILE=/data/app/~~NkwmZsImblhcEj3x0C3ZBQ==/com.termux-9tzZyDv4vcJKhizjsUGNtQ==/base.apk
TERMUX_APP__APK_RELEASE=GITHUB
TERMUX_APP__APP_VERSION_CODE=1020
TERMUX_APP__APP_VERSION_NAME=0.119.0-b1+monet36
TERMUX_APP__DATA_DIR=/data/user/0/com.termux
TERMUX_APP__IS_DEBUGGABLE_BUILD=true
TERMUX_APP__IS_INSTALLED_ON_EXTERNAL_STORAGE=false
TERMUX_APP__PACKAGE_NAME=com.termux
TERMUX_APP__PID=31641
TERMUX_APP__TARGET_SDK=28
TERMUX_VERSION=0.119.0-b1+monet36
TERMUX__SE_FILE_CONTEXT=u:object_r:app_data_file:s0:c218,c258,c512,c768
TERMUX__SE_INFO=default:targetSdkVersion=28:complete
TERMUX__SE_PROCESS_CONTEXT=u:r:untrusted_app_27:s0:c218,c258,c512,c768
TERMUX__UID=10730
TERMUX__USER_ID=0

LD Variables:
LD_LIBRARY_PATH=
LD_PRELOAD=/data/data/com.termux/files/usr/lib/libtermux-exec.so

Installed termux plugins:
com.termux.api versionCode:51
```

## Steps to Reproduce
1. Install `uv` on Termux
2. Run `uv pip install scapy`
3. Attempt to create a virtual environment using `uv venv`
4. Try installing with `--system` flag: `uv pip install scapy --system`
5. Create a virtual environment using `python -m venv uvenv`
6. Activate the virtual environment and repeat steps 2-4

## Expected Behavior
The package should install successfully, either in a virtual environment or system-wide.

## Actual Behavior
Receive errors indicating that no virtual environment or system Python installation is found, despite Python being correctly installed and accessible.

## Detailed Debug Output

### Attempt 1: Installing scapy with UV (outside virtual environment)
```
~ $ uv -v pip install scapy
DEBUG uv 0.4.10
DEBUG Searching for Python interpreter in system path
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3: Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python: Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3.11: Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

### Attempt 2: Installing scapy with UV using --system flag
```
~ $ uv -v pip install scapy --system
DEBUG uv 0.4.10
DEBUG Searching for Python interpreter in system path
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3: Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python: Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3.11: Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
error: No system Python installation found
```

### Attempt 3: Creating a virtual environment with UV
```
~ $ uv -v venv
DEBUG uv 0.4.10
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3: Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python: Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3.11: Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `.local/share/uv/python`
DEBUG Released lock at `/data/data/com.termux/files/home/.local/share/uv/python/.lock`
  × No interpreter found in managed installations or
  │ system path
```

### Attempt 4: Installing scapy with UV (inside virtual environment)
```
(uvenv) ~ $ uv -v pip install scapy
DEBUG uv 0.4.10
DEBUG Searching for Python interpreter in system path
DEBUG Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/home/uvenv/bin/python3: Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3`
DEBUG Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/home/uvenv/bin/python3: Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3`
DEBUG Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/home/uvenv/bin/python: Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python`
DEBUG Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3.11`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/home/uvenv/bin/python3.11: Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3.11`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3: Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python: Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3.11: Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

### Attempt 5: Installing scapy with UV using --system flag (inside virtual environment)
```
(uvenv) ~ $ uv -v pip install scapy --system
DEBUG uv 0.4.10
DEBUG Searching for Python interpreter in system path
DEBUG Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/home/uvenv/bin/python3: Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3`
DEBUG Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/home/uvenv/bin/python: Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python`
DEBUG Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3.11`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/home/uvenv/bin/python3.11: Can't use Python at `/data/data/com.termux/files/home/uvenv/bin/python3.11`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3: Can't use Python at `/data/data/com.termux/files/usr/bin/python3`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python: Can't use Python at `/data/data/com.termux/files/usr/bin/python`
DEBUG Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/python3.11: Can't use Python at `/data/data/com.termux/files/usr/bin/python3.11`
error: No system Python installation found
```

### Python and UV versions
```
(uvenv) ~ $ which python
/data/data/com.termux/files/home/uvenv/bin/python
(uvenv) ~ $ python --version
Python 3.11.9
(uvenv) ~ $ uv --version
uv 0.4.10
```

## Additional Observations
1. UV consistently fails to detect or use any Python installation, whether system-wide or in a virtual environment.
2. The error messages suggest that UV is unable to use the Python interpreters it finds, but doesn't provide a specific reason why.
3. The issue persists even after confirming that Python is correctly installed and accessible.
4. Attempts to create a virtual environment with UV also fail due to the same interpreter detection issue.
5. The problem occurs consistently across different scenarios and command variations, suggesting a fundamental issue with how UV interacts with the Python installation on Termux.

## Attempted Solutions
1. Upgraded Python:
   ```
   $ pkg upgrade python
   python is already the newest version (3.11.9-5).
   ```
2. Tried both system-wide and virtual environment installations.
3. Verified Python installation and accessibility.

## Additional Notes
- The issue occurs both in and out of a virtual environment.
- Python is correctly installed and accessible, but UV seems unable to detect or use it.
- Termux is using the apt package manager.
- The Termux app is installed from GitHub (TERMUX_APP__APK_RELEASE=GITHUB).

Any assistance in resolving this issue would be greatly appreciated. Let me know if you need any additional information.

---

_Comment by @samypr100 on 2024-09-14 04:04_

Interesting, seems like uv is not detecting python in Android (installed as part of the Termux APK).
How did you install `uv` within Termux? Downloading an aarch64 binary directly?

I decided to give it a quick try, and check if [get_interpreter_info.py](https://github.com/astral-sh/uv/blob/main/crates/uv-python/python/get_interpreter_info.py) even works, but it was definitely going the branch of
```
print(json.dumps({"result": "error", "kind": "libc_not_found"}))
sys.exit(0)
```
which is likely the main culprit here to some degree.

I modified it to return `unknown` for OS name intead and got below:

```json
{
  "result": "success",
  "markers": {
    "implementation_name": "cpython",
    "implementation_version": "3.11.9",
    "os_name": "posix",
    "platform_machine": "aarch64",
    "platform_python_implementation": "CPython",
    "platform_release": "[Redacted Android Release ID]",
    "platform_system": "Linux",
    "platform_version": "[Redacted Android Version ID]",
    "python_full_version": "3.11.9",
    "python_version": "3.11",
    "sys_platform": "linux"
  },
  "sys_base_prefix": "/data/data/com.termux/files/usr",
  "sys_base_exec_prefix": "/data/data/com.termux/files/usr",
  "sys_prefix": "/data/data/com.termux/files/usr",
  "sys_base_executable": "/data/data/com.termux/files/usr/bin/python",
  "sys_executable": "/data/data/com.termux/files/usr/bin/python",
  "sys_path": [
    "/data/data/com.termux/files/home",
    "/data/data/com.termux/files/usr/lib/python311.zip",
    "/data/data/com.termux/files/usr/lib/python3.11",
    "/data/data/com.termux/files/usr/lib/python3.11/lib-dynload",
    "/data/data/com.termux/files/usr/lib/python3.11/site-packages"
  ],
  "stdlib": "/data/data/com.termux/files/usr/lib/python3.11",
  "scheme": {
    "platlib": "/data/data/com.termux/files/usr/lib/python3.11/site-packages",
    "purelib": "/data/data/com.termux/files/usr/lib/python3.11/site-packages",
    "include": "/data/data/com.termux/files/usr/include/python3.11",
    "scripts": "/data/data/com.termux/files/usr/bin",
    "data": "/data/data/com.termux/files/usr"
  },
  "virtualenv": {
    "purelib": "lib/python3.11/site-packages",
    "platlib": "lib/python3.11/site-packages",
    "include": "include/site/python3.11",
    "scripts": "bin",
    "data": ""
  },
  "platform": {
    "os": {
      "name": "unknown",
      "major": -1,
      "minor": -1
    },
    "arch": "aarch64"
  },
  "manylinux_compatible": true,
  "gil_disabled": false,
  "pointer_size": "64"
}
```

---

_Comment by @ghost on 2024-09-14 08:29_

> Interesting, seems like uv is not detecting python in Android (installed as part of the Termux APK).
> How did you install uv within Termux? Downloading an aarch64 binary directly?

I installed uv normally using pkg in Termux. The exact command I used was:

```
pkg install uv
```

I didn't download any binaries directly or use any special installation methods.

> I decided to give it a quick try, and check if get_interpreter_info.py even works, but it was definitely going the branch of
> 
> print(json.dumps({"result": "error", "kind": "libc_not_found"}))
> sys.exit(0)
> which is likely the main culprit here to some degree.

Your findings about the get_interpreter_info.py script are interesting. Is there a way for me to modify this script on my end to help diagnose the issue further? I'm willing to run any tests or provide any additional information that might be helpful in resolving this problem.

Also, given that the modified script you ran was able to detect the Python installation correctly, do you have any ideas about potential fixes or workarounds I could try?

---

_Closed by @ghost on 2024-09-16 02:07_

---

_Comment by @ilotoki0804 on 2024-09-25 01:23_

@Noth1ngLol Did you solve the problem? If so, can you share your solution?

---

_Comment by @mgedmin on 2024-10-04 12:13_

Came here because of the same problem.  I think the issue was closed prematurely?

It's probably a duplicate of #2408 anyway.

---

_Reopened by @zanieb on 2024-10-04 13:37_

---

_Comment by @zanieb on 2024-10-04 13:38_

Let's use this issue to track the Python interpreter query bug and #2408 to track broader support.

---

_Label `bug` added by @zanieb on 2024-10-04 13:38_

---

_Label `uv python` added by @zanieb on 2024-10-04 13:38_

---

_Comment by @Neurovert on 2024-10-25 08:43_

> Interesting, seems like uv is not detecting python in Android (installed as part of the Termux APK). How did you install `uv` within Termux? Downloading an aarch64 binary directly?
> 
> I decided to give it a quick try, and check if [get_interpreter_info.py](https://github.com/astral-sh/uv/blob/main/crates/uv-python/python/get_interpreter_info.py) even works, but it was definitely going the branch of
> 
> ```
> print(json.dumps({"result": "error", "kind": "libc_not_found"}))
> sys.exit(0)
> ```
> 
> which is likely the main culprit here to some degree.
> 
> I modified it to return `unknown` for OS name intead and got below:
> 
> ```json
> {
>   "result": "success",
>   "markers": {
>     "implementation_name": "cpython",
>     "implementation_version": "3.11.9",
>     "os_name": "posix",
>     "platform_machine": "aarch64",
>     "platform_python_implementation": "CPython",
>     "platform_release": "[Redacted Android Release ID]",
>     "platform_system": "Linux",
>     "platform_version": "[Redacted Android Version ID]",
>     "python_full_version": "3.11.9",
>     "python_version": "3.11",
>     "sys_platform": "linux"
>   },
>   "sys_base_prefix": "/data/data/com.termux/files/usr",
>   "sys_base_exec_prefix": "/data/data/com.termux/files/usr",
>   "sys_prefix": "/data/data/com.termux/files/usr",
>   "sys_base_executable": "/data/data/com.termux/files/usr/bin/python",
>   "sys_executable": "/data/data/com.termux/files/usr/bin/python",
>   "sys_path": [
>     "/data/data/com.termux/files/home",
>     "/data/data/com.termux/files/usr/lib/python311.zip",
>     "/data/data/com.termux/files/usr/lib/python3.11",
>     "/data/data/com.termux/files/usr/lib/python3.11/lib-dynload",
>     "/data/data/com.termux/files/usr/lib/python3.11/site-packages"
>   ],
>   "stdlib": "/data/data/com.termux/files/usr/lib/python3.11",
>   "scheme": {
>     "platlib": "/data/data/com.termux/files/usr/lib/python3.11/site-packages",
>     "purelib": "/data/data/com.termux/files/usr/lib/python3.11/site-packages",
>     "include": "/data/data/com.termux/files/usr/include/python3.11",
>     "scripts": "/data/data/com.termux/files/usr/bin",
>     "data": "/data/data/com.termux/files/usr"
>   },
>   "virtualenv": {
>     "purelib": "lib/python3.11/site-packages",
>     "platlib": "lib/python3.11/site-packages",
>     "include": "include/site/python3.11",
>     "scripts": "bin",
>     "data": ""
>   },
>   "platform": {
>     "os": {
>       "name": "unknown",
>       "major": -1,
>       "minor": -1
>     },
>     "arch": "aarch64"
>   },
>   "manylinux_compatible": true,
>   "gil_disabled": false,
>   "pointer_size": "64"
> }
> ```

@samypr100 so does `uv venv` work properly for you? The json result you're showing is probably result of running just the get_interpreter_info.py script, right? Or did you edit it successfully and now uv detects the venv correctly?

---

_Comment by @samypr100 on 2024-10-30 01:37_

> @samypr100 so does `uv venv` work properly for you? The json result you're showing is probably result of running just the get_interpreter_info.py script, right? Or did you edit it successfully and now uv detects the venv correctly?

Sorry, I did not recompile uv with the changes on Android to be able to test that.

---

_Comment by @MatiasHiltunen on 2024-11-10 17:31_

I have encountered similar errors with 0.51 version installed using pkg.

I tried multiple other installation methods with no success, this was first issue while building with cargo install: 
```
error: subprocess-exited-with-error
warning: sys-info@0.9.1: c/linux.c:96:11: error: call to undeclared library function 'index' with type 'char *(const char *, int)';
ISO C99 and later do not support implicit function declarations [-Wimplicit-function-declaration]
error: failed to run custom build command for `sys-info v0.9.1`
```

For the Python-based installation using pipx:

```
pip failed to build package:
    uv

Some possibly relevant errors from pip install: 
error: subprocess-exited-with-error           warning: sys-info@0.9.1: c/linux.c:96:11: error: call to undeclared library function 'index' with type 'char *(const char *, int)'; ISO C99 and later do not support implicit function declarations [-Wimplicit-function-declaration]
    error: failed to run custom build command for `sys-info v0.9.1`
    cargo:warning=c/linux.c:96:11: error: call to undeclared library function 'index' with type 'char *(const char *, int)'; ISO C99 and later do not support implicit function declarations [-Wimplicit-function-declaration]            Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/data/data/com.termux/files/home/.local/share/pipx/venvs/uv/bin/python', '--compatibility', 'off'] returned non-zero exit status 1                                         ERROR: ERROR: Failed to build installable wheels for some pyproject.toml based projects (uv)
```

This ssue appears to stem from the sys-info crate, specifically in c/linux.c. The code calls the index function, which is deprecated and not supported in ISO C99 or later. Android's Bionic libc (used by Termux) is stricter and does not include this deprecated function.

I'm going to keep looking more into this as I suspect the issue is more related to external dependencies or env-variables that termux does differently.


---

_Comment by @dead10ck on 2024-11-11 02:50_

I tried my hand at this in #9005. Not sure if this is a good solution, but it seems to work.

---

_Comment by @MatiasHiltunen on 2024-11-13 18:24_

> I tried my hand at this in #9005. Not sure if this is a good solution, but it seems to work.

Good to know that there is a solution! Have you tried building the project in termux with these changes?

---

_Comment by @MatiasHiltunen on 2024-11-13 23:06_

About the build error that I posted, there is a pending PR that worked for me to fix the issue above that I got during build process: https://github.com/FillZpp/sys-info-rs/pull/118

Not sure how actively maintained the sys-info crate is as last commit has been 2 years ago

---

_Closed by @konstin on 2024-11-21 11:35_

---

_Comment by @ChristianF88 on 2024-11-26 07:31_

Fyi: I just tried and `uv` is still not detecting my python interpreter in termux:

Here is what I did:
```bash
pkg install uv
uv --version 
# uv 0.5.4
python --version
# Python 3.12.7
which python
# /data/data/com.termux/files/usr/bin/python
uv python find 3.12
# error: No interpreter found for Python 3.12 in virtual environments, managed installations, or search path 
```
Installing a new version does also not work:
```bash
uv python install 3.11
# error: No download found for request: cpython-3.11-linux-aarch64-none
```

Cheers 


---

_Comment by @MatiasHiltunen on 2024-11-26 13:50_

Also as long as the sys-info (v.0.9.1) crate is there it is not possible to use other installation methods that involves build process due to android 14(?) compiler restrictions with deprecated C-code. I think I saw somewhere discussion about replacing sys-info crate with more up-to-date crate, couldn't find it anymore though. With rust I have noticed that many crates that target android as a platform can not be compiled to termux because platform is detected to be aarch64-linux-android and those packages expect (correctly) to bind to android activity as they would if we were building android application, even though termux does not need the program to bind to android activity to work. When activity is not found, compile can not go trough. In some cases, I have modified the platform detection code so that compiler skips the activity check and build goes nicely through natively to aarch64-linux-android. This is not a good practise though. Perhaps this is a discussion that should be taken to Termux (or rust) community and see if there is something that could be done to help rust compiler to detect termux better as emulated linux environment and prevent the rust from expecting the android activity to be available when its not needed?

---

_Comment by @samypr100 on 2024-11-26 15:46_

Has there been a release yet that includes the fix from the PR? We'd have to wait for pkg to also have the update on termux repos.

---

_Comment by @dead10ck on 2024-11-27 00:25_

No I don't believe there has been a release yet that includes the fix

---

_Comment by @Biswa96 on 2024-11-28 20:03_

I have tested the new uv version 0.5.5 and it is working in termux now.

```
$ uv python find
/data/data/com.termux/files/usr/bin/python

$ uv python list
cpython-3.12.7-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3.12
cpython-3.12.7-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3 -> python3.12
cpython-3.12.7-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python -> python3.12
```

There are some warnings though.

```
$ uv venv venv
Using CPython 3.12.7 interpreter at: /data/data/com.termux/files/usr/bin/python
Creating virtual environment at: venv
Activate with: source venv/bin/activate

$ uv pip install pathlib2
Using Python 3.12.7 environment at: venv
Resolved 2 packages in 869ms
Prepared 2 packages in 77ms
░░░░░░░░░░░░░░░░░░░░ [0/2] Installing wheels...
         warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 2 packages in 8ms
 + pathlib2==2.3.7.post1
 + six==1.16.0
```


---

_Comment by @Vinfall on 2025-02-02 06:32_

I know this is closed but post here anyway since it's the most relevant issue. uv now works quite well except `uv python install` but that's a upstream issue and on Termux we have official PyPy packages and TUR for older Pythons.

However, if I have installed both `python3` and `pypy3`, only python3 is detected (despite pypy being in `$PATH`):

```sh
# python interpreter and uv
$ pkg install python3 python3-pip pypy3 uv
$ python3 -V
Python 3.12.8
$ pip -V
pip 24.3.1 from /data/data/com.termux/files/usr/lib/python3.12/site-packages/pip (python 3.12)
$ pypy3 -V
Python 3.9.18 (9c4f8ef178b6, Oct 06 2024, 19:31:31)
[PyPy 7.3.15 with GCC Clang 19.1.1]
$ uv -V
uv 0.5.25

# path
$ echo $PATH | sed 's/:/\n/g'
/data/data/com.termux/files/home/.local/bin
/data/data/com.termux/files/home/.local/bin-sh
/data/data/com.termux/files/home/.local/bin-rust
/data/data/com.termux/files/usr/bin
$ which python3
/data/data/com.termux/files/usr/bin/python3
$ which pypy3
/data/data/com.termux/files/usr/bin/pypy3

$ uv python list
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3.12
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3 -> python3.12
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python -> python3.12
```

Moreover, if I install older python from TUR, uv would detect that successfully. Given that on my x86_64 Linux uv does show download for pypy, this should be a bug:

```sh
# this is from tur-packages
pkg install python3.10
$ uv python list
cpython-3.12.8-linux-aarch64-none     /data/data/com.termux/files/usr/bin/python3.12
cpython-3.12.8-linux-aarch64-none     /data/data/com.termux/files/usr/bin/python3 -> python3.12
cpython-3.12.8-linux-aarch64-none     /data/data/com.termux/files/usr/bin/python -> python3.12
cpython-3.10.15-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3.10
```

By the way, in Termux there is a `$PREFIX` (` /data/data/com.termux/files/`) which acts as `/` in regular Linux so maybe you can check the path here.

---

_Comment by @zanieb on 2025-02-02 07:02_

@Vinfall please open a new issue with verbose logs.

---

_Comment by @mecrayavcin on 2025-11-21 22:26_

Same problem:
`uv python install` does not work at all.

---

_Comment by @zanieb on 2025-11-21 23:12_

@mecrayavcin please see #9452 and open a new issue with details.

---
