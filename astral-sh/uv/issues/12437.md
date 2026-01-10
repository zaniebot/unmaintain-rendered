```yaml
number: 12437
title: "Issue with installing `fasttext` Using `uv` despite Fix #1446"
type: issue
state: closed
author: ThomasFaria
labels:
  - bug
assignees: []
created_at: 2025-03-24T16:04:11Z
updated_at: 2025-03-25T01:50:37Z
url: https://github.com/astral-sh/uv/issues/12437
synced_at: 2026-01-10T03:50:31Z
```

# Issue with installing `fasttext` Using `uv` despite Fix #1446

---

_Issue opened by @ThomasFaria on 2025-03-24 16:04_

### Summary

### Issue with Installing `fasttext` Using `uv` Despite Fix #1446

I encountered an issue when attempting to install `fasttext` using `uv`, which appears identical to the previously reported (and closed) issue #1469. Although it was indicated in #1469 that this was resolved by the fix described in issue #1446, the problem persists in my case.

Thanks a lot for your support and your work on this excellent tool!


**Environment:**
- OS: Ubuntu 22.04
- Python Version: 3.12.6
- `uv` Version: 0.6.9




### Platform

Ubuntu 22.04 amd64

### Version

0.6.9

### Python version

3.12.6

---

_Label `bug` added by @ThomasFaria on 2025-03-24 16:04_

---

_Comment by @charliermarsh on 2025-03-24 16:25_

Are you installing from the GitHub directly? I don't think they released to PyPI since fixing this.

---

_Comment by @notatallshaw on 2025-03-24 16:42_

Actually the error is quite different:

```
Resolved 4 packages in 5ms
  × Failed to build `fasttext==0.9.3`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/build_meta.py", line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/build_meta.py", line 304, in _get_build_requires
          self.run_setup()
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/build_meta.py", line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 171, in <module>
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/__init__.py", line 116, in setup
          _install_setup_requires(attrs)
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/__init__.py", line 87, in _install_setup_requires
          dist.parse_config_files(ignore_option_errors=True)
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/_virtualenv.py", line 20, in parse_config_files
          result = old_parse_config_files(self, *args, **kwargs)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/dist.py", line 730, in parse_config_files
          self._parse_config_files(filenames=inifiles)
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/dist.py", line 599, in _parse_config_files
          opt = self._enforce_underscore(opt, section)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/dshaw/.cache/uv/builds-v0/.tmp3zLwhL/lib/python3.12/site-packages/setuptools/dist.py", line 629, in _enforce_underscore
          raise InvalidConfigError(
      setuptools.errors.InvalidConfigError: Invalid dash-separated key 'description-file' in 'metadata' (setup.cfg), please use the underscore name 'description_file' instead.

      hint: This usually indicates a problem with the package or the build environment.
```

The error is coming from setuptools, it is complaining that [setup.cfg](https://github.com/facebookresearch/fastText/blob/main/setup.cfg#L2) is using an invalid key format.

This is a new requirement by setuptools, you need to report this to fasttext and check the release notes / issues page of setuptools if you think this requirement is wrong (although it's their config file format so it's up to them how they define it).

To use an old version of setuptools to install the package do the following:

1. Create a `build-constraints.txt` with the following contents:

```
setuptools<78
```

2. Install with the build-constraints defined `uv pip  install fasttext --build-constraints build-constraints.txt`:

```
Resolved 4 packages in 285ms
      Built fasttext==0.9.3
Prepared 4 packages in 30.88s
Installed 4 packages in 493ms
 + fasttext==0.9.3
 + numpy==2.2.4
 + pybind11==2.13.6
 + setuptools==78.0.1
```

---

_Comment by @zanieb on 2025-03-24 16:43_

See https://github.com/astral-sh/uv/issues/12434

---

_Comment by @ThomasFaria on 2025-03-24 17:42_

Thank you for the quick fix. 2 comments :
- Actually, I would prefer use the 0.9.2 version if possible and it that case I get the error described in #1469. 
- I managed to install fasttext 0.9.3 with your command `uv pip  install fasttext --build-constraints build-constraints.txt`, however, when I try to re-sync with `uv sync` I still get the same error.

---

_Comment by @notatallshaw on 2025-03-24 17:53_

> Actually, I would prefer use the 0.9.2 version if possible and it that case I get the error described in https://github.com/astral-sh/uv/issues/1469.

Then you will have to install the build dependencies yourself and use no-build-isolation: https://docs.astral.sh/uv/reference/settings/#no-build-isolation

> I managed to install fasttext 0.9.3 with your command `uv pip install fasttext --build-constraints build-constraints.txt`, however, when I try to re-sync with `uv sync` I still get the same error.

See https://github.com/astral-sh/uv/issues/12441

---

_Closed by @zanieb on 2025-03-25 01:50_

---
