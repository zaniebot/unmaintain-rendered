---
number: 14305
title: "Failed to build anyio; command `hg root` timed out after 40 seconds on Windows"
type: issue
state: open
author: zanieb
labels:
  - windows
  - ci-flake
assignees: []
created_at: 2025-06-27T03:15:52Z
updated_at: 2025-12-22T16:18:13Z
url: https://github.com/astral-sh/uv/issues/14305
synced_at: 2026-01-07T13:12:18-06:00
---

# Failed to build anyio; command `hg root` timed out after 40 seconds on Windows

---

_Issue opened by @zanieb on 2025-06-27 03:15_

The test cases
 
- `export::pep_751_sdist_url`
- `lock::no_lowest_warning_with_name_and_url`

flaked with:

```
FAIL [  57.989s] uv::it export::pep_751_sdist_url
  stdout ───

    running 1 test
    test export::pep_751_sdist_url ... FAILED

    failures:

    failures:
        export::pep_751_sdist_url

    test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 1703 filtered out; finished in 57.96s
    
  stderr ───

    thread 'export::pep_751_sdist_url' panicked at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb\library\core\src\ops\function.rs:250:5:
    Unexpected failure.
    code=1
    stderr=``````
      × Failed to build `anyio @ https://files.pythonhosted.org/packages/db/4d/3970183622f0330d3c23d9b8a5f52e365e50381fd484d08e[3285](https://github.com/astral-sh/uv/actions/runs/15915149050/job/44891290321?pr=14301#step:10:3286)104333d3/anyio-4.3.0.tar.gz`
      ├─▶ The build backend returned an error
      ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit code: 1)

          [stdout]
          running egg_info
          writing src\\anyio.egg-info\\PKG-INFO
          writing dependency_links to src\\anyio.egg-info\\dependency_links.txt
          writing entry points to src\\anyio.egg-info\\entry_points.txt
          writing requirements to src\\anyio.egg-info\\requires.txt
          writing top-level names to src\\anyio.egg-info\\top_level.txt

          [stderr]
          Traceback (most recent call last):
            File \"<string>\", line 14, in <module>
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\build_meta.py\", line 325, in get_requires_for_build_wheel
              return self._get_build_requires(config_settings, requirements=[\'wheel\'])
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\build_meta.py\", line 295, in _get_build_requires
              self.run_setup()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\build_meta.py\", line 311, in run_setup
              exec(code, locals())
            File \"<string>\", line 1, in <module>
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\__init__.py\", line 104, in setup
              return distutils.core.setup(**attrs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\_distutils\\core.py\", line 185, in setup
              return run_commands(dist)
                     ^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\_distutils\\core.py\", line 201, in run_commands
              dist.run_commands()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\_distutils\\dist.py\", line 969, in run_commands
              self.run_command(cmd)
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\dist.py\", line 967, in run_command
              super().run_command(command)
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\_distutils\\dist.py\", line 988, in run_command
              cmd_obj.run()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\egg_info.py\", line 321, in run
              self.find_sources()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\egg_info.py\", line 329, in find_sources
              mm.run()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\egg_info.py\", line 550, in run
              self.add_defaults()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\egg_info.py\", line 591, in add_defaults
              rcfiles = list(walk_revctrl())
                        ^^^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\sdist.py\", line 16, in walk_revctrl
              yield from ep.load()(dirname)
                         ^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools_scm\\_file_finders\\__init__.py\", line 103, in find_files
              res: list[str] = command(path)
                               ^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools_scm\\_file_finders\\hg.py\", line 50, in hg_find_files
              toplevel = _hg_toplevel(path)
                         ^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools_scm\\_file_finders\\hg.py\", line 18, in _hg_toplevel
              res = _run(
                    ^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools_scm\\_run_cmd.py\", line 144, in run
              res = subprocess.run(
                    ^^^^^^^^^^^^^^^
            File \"C:\\Users\\Administrator\\AppData\\Roaming\\uv\\python\\cpython-3.12.9-windows-x86_64-none\\Lib\\subprocess.py\", line 552, in run
              stdout, stderr = process.communicate(input, timeout=timeout)
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            File \"C:\\Users\\Administrator\\AppData\\Roaming\\uv\\python\\cpython-3.12.9-windows-x86_64-none\\Lib\\subprocess.py\", line 1211, in communicate
              stdout, stderr = self._communicate(input, endtime, timeout)
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            File \"C:\\Users\\Administrator\\AppData\\Roaming\\uv\\python\\cpython-3.12.9-windows-x86_64-none\\Lib\\subprocess.py\", line 1632, in _communicate
              raise TimeoutExpired(self.args, orig_timeout)
          subprocess.TimeoutExpired: Command \'[\'hg\', \'root\']\' timed out after 40 seconds

          hint: This usually indicates a problem with the package or the build environment.
    ```
    ```
    command=`"V:\\uv\\target\\debug\\uv.exe" "lock" "--cache-dir" "V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache"`
    code=1
    stdout=""
    stderr=```
      × Failed to build `anyio @ [https://files.pythonhosted.org/packages/db/4d/3970183622f0330d3c23d9b8a5f52e365e50381fd484d08e3285104333d3/anyio-4.3.0.tar.gz`](https://files.pythonhosted.org/packages/db/4d/3970183622f0330d3c23d9b8a5f52e365e50381fd484d08e3285104333d3/anyio-4.3.0.tar.gz%60)
      ├─▶ The build backend returned an error
      ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit code: 1)

          [stdout]
          running egg_info
          writing src\\anyio.egg-info\\PKG-INFO
          writing dependency_links to src\\anyio.egg-info\\dependency_links.txt
          writing entry points to src\\anyio.egg-info\\entry_points.txt
          writing requirements to src\\anyio.egg-info\\requires.txt
          writing top-level names to src\\anyio.egg-info\\top_level.txt

          [stderr]
          Traceback (most recent call last):
            File \"<string>\", line 14, in <module>
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\build_meta.py\", line 325, in get_requires_for_build_wheel
              return self._get_build_requires(config_settings, requirements=[\'wheel\'])
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\build_meta.py\", line 295, in _get_build_requires
              self.run_setup()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\build_meta.py\", line 311, in run_setup
              exec(code, locals())
            File \"<string>\", line 1, in <module>
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\__init__.py\", line 104, in setup
              return distutils.core.setup(**attrs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\_distutils\\core.py\", line 185, in setup
              return run_commands(dist)
                     ^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\_distutils\\core.py\", line 201, in run_commands
              dist.run_commands()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\_distutils\\dist.py\", line 969, in run_commands
              self.run_command(cmd)
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\dist.py\", line 967, in run_command
              super().run_command(command)
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\_distutils\\dist.py\", line 988, in run_command
              cmd_obj.run()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\egg_info.py\", line 321, in run
              self.find_sources()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\egg_info.py\", line 329, in find_sources
              mm.run()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\egg_info.py\", line 550, in run
              self.add_defaults()
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\egg_info.py\", line 591, in add_defaults
              rcfiles = list(walk_revctrl())
                        ^^^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools\\command\\sdist.py\", line 16, in walk_revctrl
              yield from ep.load()(dirname)
                         ^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools_scm\\_file_finders\\__init__.py\", line 103, in find_files
              res: list[str] = command(path)
                               ^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools_scm\\_file_finders\\hg.py\", line 50, in hg_find_files
              toplevel = _hg_toplevel(path)
                         ^^^^^^^^^^^^^^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools_scm\\_file_finders\\hg.py\", line 18, in _hg_toplevel
              res = _run(
                    ^^^^^
            File \"V:\\uv-tmp\\uv\\tests\\.tmpH1jK8V\\cache\\builds-v0\\.tmplldMC6\\Lib\\site-packages\\setuptools_scm\\_run_cmd.py\", line 144, in run
              res = subprocess.run(
                    ^^^^^^^^^^^^^^^
            File \"C:\\Users\\Administrator\\AppData\\Roaming\\uv\\python\\cpython-3.12.9-windows-x86_64-none\\Lib\\subprocess.py\", line 552, in run
              stdout, stderr = process.communicate(input, timeout=timeout)
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            File \"C:\\Users\\Administrator\\AppData\\Roaming\\uv\\python\\cpython-3.12.9-windows-x86_64-none\\Lib\\subprocess.py\", line 1211, in communicate
              stdout, stderr = self._communicate(input, endtime, timeout)
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            File \"C:\\Users\\Administrator\\AppData\\Roaming\\uv\\python\\cpython-3.12.9-windows-x86_64-none\\Lib\\subprocess.py\", line 1632, in _communicate
              raise TimeoutExpired(self.args, orig_timeout)
          subprocess.TimeoutExpired: Command \'[\'hg\', \'root\']\' timed out after 40 seconds

          hint: This usually indicates a problem with the package or the build environment.
    ```
```

more logs at https://gist.github.com/zanieb/e1841842e3f1643fc47b539d02a7eeb4

---

_Label `windows` added by @zanieb on 2025-06-27 03:15_

---

_Label `ci-flake` added by @zanieb on 2025-06-27 03:15_

---

_Comment by @ssbarnea on 2025-10-13 14:08_

@zanieb Not sure what caused it but I seen this for the first time on **LINUX** https://github.com/ansible/ansible-dev-environment/actions/runs/18468369122/job/52615906699?pr=374 and we make not use of `hg` ourselves, not sure how we can avoid it.

---

_Comment by @zanieb on 2025-10-13 14:12_

@ssbarnea the fix for you would be to avoid using a source distribution. This isn't a uv bug, this is a problem with hg which is invoked by anyio's build system.

---

_Comment by @ssbarnea on 2025-10-13 14:16_

Thanks for the update. It would be useful to link to an upstream bug to see if it goes into the right direction. TBH considering that (AFAIK) we have no dependency on anything using `hg`, we would probably not want to execute `hg` at all. Maybe they can add an option to disable the hg functionality using an environment variable. -- Might be a flake but what if it starts to become more common.

---

_Comment by @zanieb on 2025-10-13 14:19_

I don't know what the upstream bug is. Feel free to open one, but I don't have a clear enough reproduction to do so. This issue is just for tracking a flake in uv's CI.

---

_Comment by @EliteTK on 2025-12-22 16:18_

https://github.com/astral-sh/uv/actions/runs/20436824487/job/58720087106?pr=17216

---
