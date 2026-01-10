---
number: 10679
title: uv fails building a wheel (e. g. mysqlclient)
type: issue
state: closed
author: laxas
labels:
  - question
assignees: []
created_at: 2025-01-16T13:57:41Z
updated_at: 2025-03-31T07:44:45Z
url: https://github.com/astral-sh/uv/issues/10679
synced_at: 2026-01-10T01:24:56Z
---

# uv fails building a wheel (e. g. mysqlclient)

---

_Issue opened by @laxas on 2025-01-16 13:57_

uv tryes to build a wheel for `mysqlclient` but fails. I had this problem bevor on another server and could solve it by installing clang. This time it did not. 

Is there a list of OS dependecies uv needs to build wheels? Anny other reason the build fails?

OS: Ubuntu 24.04.1 (with clang, gcc and build-essential installed)

```bash
>>uv sync --no-group dev         
Resolved 273 packages in 2ms
  × Failed to build `mysqlclient==2.2.7`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stdout]
      Trying pkg-config --exists mysqlclient
      Command 'pkg-config --exists mysqlclient' returned non-zero exit status 1.
      Trying pkg-config --exists mariadb
      Command 'pkg-config --exists mariadb' returned non-zero exit status 1.
      Trying pkg-config --exists libmariadb
      Command 'pkg-config --exists libmariadb' returned non-zero exit status 1.
      Trying pkg-config --exists perconaserverclient
      Command 'pkg-config --exists perconaserverclient' returned non-zero exit status 1.

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "/home/.../.cache/uv/builds-v0/.tmpkm2aYb/lib/python3.12/site-packages/setuptools/build_meta.py", line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/.../.cache/uv/builds-v0/.tmpkm2aYb/lib/python3.12/site-packages/setuptools/build_meta.py", line 304, in _get_build_requires
          self.run_setup()
        File "/home/.../.cache/uv/builds-v0/.tmpkm2aYb/lib/python3.12/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 156, in <module>
        File "<string>", line 49, in get_config_posix
        File "<string>", line 28, in find_package_name
      Exception: Can not find valid pkg-config name.
      Specify MYSQLCLIENT_CFLAGS and MYSQLCLIENT_LDFLAGS env vars manually

      hint: This usually indicates a problem with the package or the build environment.
  help: `mysqlclient` (v2.2.7) was included because `web` (v0.3.0) depends on `mysqlclient`
```

```bash
>> clang --version                
Ubuntu clang version 18.1.3 (1ubuntu1)
Target: x86_64-pc-linux-gnu
Thread model: posix
InstalledDir: /usr/bin
```

```bash
>> gcc --version 
gcc (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```


---

_Comment by @notatallshaw on 2025-01-16 14:26_

> Is there a list of OS dependecies uv needs to build wheels? Anny other reason the build fails?

The general approach to these sort of problems is to google the message at the bottom the traceback, in this case this is probably the relevant line:

> Exception: Can not find valid pkg-config name.

I also added the name of your distro to a google search and this was the top result, see if the answers there help you: https://stackoverflow.com/questions/76585758/mysqlclient-cannot-install-via-pip-cannot-find-pkg-config-name-in-ubuntu

---

_Label `question` added by @konstin on 2025-01-16 14:27_

---

_Comment by @charliermarsh on 2025-01-16 15:02_

Please make sure that you've reinstalled Python too in case you're on an old install (e.g., `uv python install --reinstall 3.12`).

---

_Comment by @zanieb on 2025-01-16 17:53_

See also https://docs.astral.sh/uv/reference/build_failures/

---

_Comment by @samypr100 on 2025-01-17 01:13_

Agreed with @notatallshaw, in this case its simply a matter of installing the necessary headers via apt, e.g. `apt install libmysqlclient-dev`

---

_Comment by @laxas on 2025-01-17 08:28_

Many thanks to everyone who commented! `apt install libmysqlclient-dev` solved the problem.

I wasn't aware that it was a specific MySQL problem. The hint in the error message made it seem like I was missing a general dependency for building wheels.

---

_Closed by @charliermarsh on 2025-01-17 17:29_

---
