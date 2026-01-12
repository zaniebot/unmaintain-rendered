```yaml
number: 1491
title: uv pip install inconsistent failure on Windows
type: issue
state: closed
author: Wofiel
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-16T14:07:16Z
updated_at: 2025-11-07T15:38:07Z
url: https://github.com/astral-sh/uv/issues/1491
synced_at: 2026-01-12T15:58:28Z
```

# uv pip install inconsistent failure on Windows

---

_@Wofiel_

- Windows 22H2
- Using PowerShell env (in either Windows Terminal or powershell.exe, non-elevated)

When using `uv pip install -r requirements.txt`, I will frequently get the following error:

```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: setproctitle==1.3.3
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\Temp\1\.tmp56ZHLz\.tmpMXOwQn to \\?\C:\Users\[____]\AppData\Local\Temp\1\.tmp56ZHLz\archive-v0\7RXhEx7TcTNKRTlP3vSED
  Caused by: Access is denied. (os error 5)
```

This appears to be independent of the package, though seemingly happens more often with some than others.

If run with `--no-cache`, it's almost impossible to install, as at least one thing is likely to fail in the process and progress restarted.

If run without `--no-cache`, retrying a number of times will eventually succeed (however, it will leave `.tmp[_______]` folders listed in the error messages in `C:\Users\[____]\AppData\Local\uv\cache`).

---

_Comment by @AucaCoyan on 2024-02-16 14:48_

I did a test on my windows pc and debian on a VM, and did experiment the same results as the issue.
On Windows 10, using [nushell](https://github.com/nushell/nushell/)
```nushell
"distributions=2.2.1" | save requirements.txt
uv venv
uv pip install -r requirements.txt
```
gives
```bash
error: Failed to download and build: distributions==2.2.1
  Caused by: Failed to build: distributions==2.2.1
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel`:
--- stdout:

--- stderr:
<string>:177: SyntaxWarning: invalid escape sequence '\S'
Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "C:\Users\aucac\AppData\Local\Temp\.tmpn2KafH\.venv\Lib\site-packages\setuptools\build_meta.py", line 366, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "C:\Users\aucac\AppData\Local\Temp\.tmpn2KafH\.venv\Lib\site-packages\setuptools\build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "C:\Users\aucac\AppData\Local\Temp\.tmpn2KafH\.venv\Lib\site-packages\setuptools\build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 31, in <module>
ModuleNotFoundError: No module named 'numpy'
---
```
and in Debian is the same (give or take the paths and other stuff).

Then I compared with pip
```bash
Collecting distributions==2.2.1 (from -r requirements.txt (line 1))
  Downloading distributions-2.2.1.tar.gz (1.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.5/1.5 MB 6.1 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [21 lines of output]
      <string>:177: SyntaxWarning: invalid escape sequence '\S'
      Traceback (most recent call last):
        File "C:\Users\aucac\scoop\apps\python\current\Lib\site-packages\pip\_vendor\pyproject_hooks\_in_process\_in_process.py", line 353, in <module>
          main()
        File "C:\Users\aucac\scoop\apps\python\current\Lib\site-packages\pip\_vendor\pyproject_hooks\_in_process\_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\aucac\scoop\apps\python\current\Lib\site-packages\pip\_vendor\pyproject_hooks\_in_process\_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\aucac\AppData\Local\Temp\pip-build-env-z4nk3ptj\overlay\Lib\site-packages\setuptools\build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\aucac\AppData\Local\Temp\pip-build-env-z4nk3ptj\overlay\Lib\site-packages\setuptools\build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "C:\Users\aucac\AppData\Local\Temp\pip-build-env-z4nk3ptj\overlay\Lib\site-packages\setuptools\build_meta.py", line 480, in run_setup
          super().run_setup(setup_script=setup_script)
        File "C:\Users\aucac\AppData\Local\Temp\pip-build-env-z4nk3ptj\overlay\Lib\site-packages\setuptools\build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 31, in <module>
      ModuleNotFoundError: No module named 'numpy'
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× Getting requirements to build wheel did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: python.exe -m pip install --upgrade pip
```
And have around the same result.
Also, [distributions is not updated 2017](https://pypi.org/project/distributions/)
Can you copy and paste your `requirements.txt` dependencies?

---

_Comment by @Wofiel on 2024-02-16 15:37_

That appears to be a different error. This isn't about the particular package `distributions`. This appears to be very specifically just an error somewhere in the renaming of folders.

Just tried using a requirements file off the shelf: https://github.com/google-research/kubric/blob/main/requirements.txt

I've added the full log including retries this time, showing how it's not always the same package, and retries will eventually succeed. (Also generating 300+MB of trash in the leftover `.tmp[______]` folders in `C:\Users\[____]\AppData\Local\uv\cache`)

Subsequent installs are fine after the first install (and fast!), as everything is already in the cache.

Contrasted with regular Python pip, which installs first try, with or without cache.

```
PS C:\dev\uv_test_uv> uv venv; .\.venv\Scripts\activate.ps1; uv pip install -r requirements.txt
Using Python 3.11.6 interpreter at C:\dev\uv_test_uv\.venv\Scripts\python.exe
Creating virtualenv at: .venv
Resolved 136 packages in 31.02s
error: Failed to download distributions
  Caused by: Failed to fetch wheel: regex==2023.12.25
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\uv\cache\.tmpXU7l5Y to \\?\C:\Users\[____]\AppData\Local\uv\cache\archive-v0\IPIdPmnxfhPlAbb4aByib
  Caused by: Access is denied. (os error 5)
(.venv) PS C:\dev\uv_test_uv> uv pip install -r requirements.txt
Resolved 136 packages in 216ms
   Built dill==0.3.1.1
error: Failed to download distributions
  Caused by: Failed to fetch wheel: fastavro==1.9.4
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\uv\cache\.tmpvibAxB to \\?\C:\Users\[____]\AppData\Local\uv\cache\archive-v0\xESJmy5hSHMPWyOAki-mV
  Caused by: Access is denied. (os error 5)
(.venv) PS C:\dev\uv_test_uv> uv pip install -r requirements.txt
Resolved 136 packages in 202ms
   Built crcmod==1.7
error: Failed to download distributions
  Caused by: Failed to unzip wheel: crcmod==1.7
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\uv\cache\.tmpuSxG40 to \\?\C:\Users\[____]\AppData\Local\uv\cache\archive-v0\8seE9kMwlOJb0FO7kkUiO
  Caused by: Access is denied. (os error 5)
(.venv) PS C:\dev\uv_test_uv> uv pip install -r requirements.txt
Resolved 136 packages in 168ms
   Built hdfs==2.7.3
   Built docopt==0.6.2
   Built promise==2.3
   Built pyjsparser==2.7.1
   Built cloudml-hypertune==0.1.0.dev6
error: Failed to download distributions
  Caused by: Failed to fetch wheel: rapidfuzz==3.6.1
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\uv\cache\.tmpnchaw9 to \\?\C:\Users\[____]\AppData\Local\uv\cache\archive-v0\fslC-i4Ih5c8itkG3OCU1
  Caused by: Access is denied. (os error 5)
(.venv) PS C:\dev\uv_test_uv> uv pip install -r requirements.txt
Resolved 136 packages in 199ms
   Built google-apitools==0.5.31
error: Failed to download distributions
  Caused by: Failed to fetch wheel: scikit-learn==1.4.1.post1
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\uv\cache\.tmpyPARfj to \\?\C:\Users\[____]\AppData\Local\uv\cache\archive-v0\BTqIbBmeNiUJZ4iFLjvr5
  Caused by: Access is denied. (os error 5)
(.venv) PS C:\dev\uv_test_uv> uv pip install -r requirements.txt
Resolved 136 packages in 187ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: scikit-learn==1.4.1.post1
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\uv\cache\.tmpQ9zLLc to \\?\C:\Users\[____]\AppData\Local\uv\cache\archive-v0\N6_d31F0MmeWfay8zuoy1
  Caused by: Access is denied. (os error 5)
(.venv) PS C:\dev\uv_test_uv> uv pip install -r requirements.txt
Resolved 136 packages in 198ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: pyarrow==11.0.0
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\uv\cache\.tmpoDjYqG to \\?\C:\Users\[____]\AppData\Local\uv\cache\archive-v0\77wbDpLZdJXaCe7d8Bgl4
  Caused by: Access is denied. (os error 5)
(.venv) PS C:\dev\uv_test_uv> uv pip install -r requirements.txt
Resolved 136 packages in 180ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: scipy==1.12.0
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\[____]\AppData\Local\uv\cache\.tmpJrQ1Eo to \\?\C:\Users\[____]\AppData\Local\uv\cache\archive-v0\oJCgnzumPreaL33Y2A_jb
  Caused by: Access is denied. (os error 5)
(.venv) PS C:\dev\uv_test_uv> uv pip install -r requirements.txt
Resolved 136 packages in 193ms
Downloaded 3 packages in 1m 30s
Installed 136 packages in 41.33s
 + absl-py==1.4.0
 + apache-beam==2.54.0
 + astunparse==1.6.3
 + attrs==23.2.0
 + bidict==0.23.0
 + cachetools==5.3.2
 + certifi==2024.2.2
 + charset-normalizer==3.3.2
 + click==8.1.7
 + cloudml-hypertune==0.1.0.dev6
 + cloudpickle==2.2.1
 + colorama==0.4.6
 + crcmod==1.7
 + dataclasses==0.6
 + deprecated==1.2.14
 + dill==0.3.1.1
 + dm-tree==0.1.8
 + dnspython==2.5.0
 + docopt==0.6.2
 + etils==1.7.0
 + fastavro==1.9.4
 + fasteners==0.19
 + flatbuffers==23.5.26
 + fsspec==2024.2.0
 + gast==0.5.4
 + google-api-core==2.17.1
 + google-apitools==0.5.31
 + google-auth==2.28.0
 + google-auth-httplib2==0.1.1
 + google-auth-oauthlib==1.2.0
 + google-cloud-aiplatform==1.42.0
 + google-cloud-bigquery==3.17.2
 + google-cloud-bigquery-storage==2.24.0
 + google-cloud-bigtable==2.23.0
 + google-cloud-core==2.4.1
 + google-cloud-datastore==2.19.0
 + google-cloud-dlp==3.15.1
 + google-cloud-language==2.13.1
 + google-cloud-pubsub==2.19.4
 + google-cloud-pubsublite==1.9.0
 + google-cloud-recommendations-ai==0.10.8
 + google-cloud-resource-manager==1.12.1
 + google-cloud-spanner==3.42.0
 + google-cloud-storage==2.14.0
 + google-cloud-videointelligence==2.13.1
 + google-cloud-vision==3.7.0
 + google-crc32c==1.5.0
 + google-pasta==0.2.0
 + google-resumable-media==2.7.0
 + googleapis-common-protos==1.62.0
 + grpc-google-iam-v1==0.13.0
 + grpc-interceptor==0.15.4
 + grpcio==1.60.1
 + grpcio-status==1.60.1
 + h5py==3.10.0
 + hdfs==2.7.3
 + httplib2==0.22.0
 + idna==3.6
 + imageio==2.34.0
 + importlib-resources==6.1.1
 + joblib==1.3.2
 + js2py==0.74
 + jsonpickle==3.0.2
 + jsonschema==4.21.1
 + jsonschema-specifications==2023.12.1
 + keras==2.15.0
 + levenshtein==0.25.0
 + libclang==16.0.6
 + markdown==3.5.2
 + markupsafe==2.1.5
 + ml-dtypes==0.2.0
 + munch==4.0.0
 + numpy==1.24.4
 + oauth2client==4.1.3
 + oauthlib==3.2.2
 + objsize==0.7.0
 + opt-einsum==3.3.0
 + orjson==3.9.14
 + overrides==7.7.0
 + packaging==23.2
 + pandas==2.2.0
 + pillow==10.2.0
 + promise==2.3
 + proto-plus==1.23.0
 + protobuf==4.25.3
 + psutil==5.9.8
 + pyarrow==11.0.0
 + pyarrow-hotfix==0.6
 + pyasn1==0.5.1
 + pyasn1-modules==0.3.0
 + pydot==1.4.2
 + pyjsparser==2.7.1
 + pymongo==4.6.1
 + pyparsing==3.1.1
 + pypng==0.20220715.0
 + pyquaternion==0.9.9
 + python-dateutil==2.8.2
 + python-levenshtein==0.25.0
 + pytz==2024.1
 + rapidfuzz==3.6.1
 + referencing==0.33.0
 + regex==2023.12.25
 + requests==2.31.0
 + requests-oauthlib==1.3.1
 + rpds-py==0.18.0
 + rsa==4.9
 + scikit-learn==1.4.1.post1
 + scipy==1.12.0
 + setuptools==69.1.0
 + shapely==2.0.3
 + singledispatchmethod==1.0
 + six==1.16.0
 + sqlparse==0.4.4
 + tensorboard==2.15.2
 + tensorboard-data-server==0.7.2
 + tensorflow==2.15.0
 + tensorflow-datasets==4.9.4
 + tensorflow-estimator==2.15.0
 + tensorflow-intel==2.15.0
 + tensorflow-io-gcs-filesystem==0.31.0
 + tensorflow-metadata==1.13.1
 + termcolor==2.4.0
 + threadpoolctl==3.3.0
 + toml==0.10.2
 + tqdm==4.66.2
 + traitlets==5.14.1
 + trimesh==4.1.3
 + typing-extensions==4.9.0
 + tzdata==2024.1
 + tzlocal==5.2
 + urllib3==2.2.0
(.venv) PS C:\dev\uv_test_uv>
```

---

_Comment by @AucaCoyan on 2024-02-16 15:49_

I see, thanks for the log! Again, I have a different output sadly 😢. I think I am doing something wrong too.

In my case, it didn't found a compatible version of tensorflow. If you are interested, here I leave the details, but I think my case is unrelated to this issue.

<details>

```bash
(.venv) PS C:\Users\aucac\repos\exp-python\uv> uv pip install -r .\requirements.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of tensorflow are available:
          tensorflow<=2.5.3
          tensorflow>=2.6.0,<=2.6.5
          tensorflow>=2.7.0,<=2.7.4
          tensorflow>=2.8.0,<=2.8.4
          tensorflow>=2.9.0,<=2.9.3
          tensorflow>=2.10.0,<=2.10.1
          tensorflow>=2.11.0,<=2.11.1
          tensorflow>=2.12.0,<=2.12.1
          tensorflow>=2.13.0,<=2.13.1
          tensorflow>=2.14.0,<=2.14.1
          tensorflow>=2.15.0
      and tensorflow==0.12.0 is unusable because no wheels are available with a matching Python ABI, we can conclude that any of:
          tensorflow<0.12.1
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==0.12.1 is unusable because no wheels are available with a matching Python ABI, we can conclude that any of:
          tensorflow<1.0.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.0.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.0.1 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.1.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.1.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.2.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.2.1
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.2.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.3.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.4.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.4.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.4.1 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.5.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.5.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.5.1 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.6.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.6.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.7.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.7.1
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.7.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.8.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.9.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.9.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.10.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.10.1
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.10.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.11.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.12.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.12.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.12.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.12.3
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.12.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.13.1 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.13.2
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.13.2 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.14.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.15.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.15.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.15.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.15.3
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.15.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==1.15.4 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<1.15.5
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==1.15.5 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.0.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.0.1
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.0.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.0.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.0.3
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.0.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.0.4 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.1.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.1.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.1.1 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.1.2
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.1.2 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.1.3 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.1.4
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.1.4 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.2.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.2.1
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.2.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.2.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.2.3
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.2.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.3.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.3.1
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.3.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.3.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.3.3
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.3.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.3.4 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.4.0
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.4.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.4.1 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.4.2
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.4.2 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.4.3 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.4.4
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.4.4 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.5.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.5.1
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.5.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.5.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.5.3
          tensorflow>2.5.3,<2.6.0
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.5.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.6.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.6.1
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.6.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.6.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.6.3
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.6.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.6.4 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.6.5
          tensorflow>2.6.5,<2.7.0
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.6.5 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.7.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.7.1
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.7.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.7.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.7.3
          tensorflow>2.7.4,<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.7.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.7.4 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.8.0
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.8.0 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.8.1 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.8.2
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.8.2 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.8.3 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.8.4
          tensorflow>2.8.4,<2.9.0
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.8.4 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.9.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.9.1
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.9.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.9.2 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.9.3
          tensorflow>2.9.3,<2.10.0
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.9.3 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.10.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.10.1
          tensorflow>2.10.1,<2.11.0
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.10.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.11.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.11.1
          tensorflow>2.11.1,<2.12.0
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.11.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.12.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.12.1
          tensorflow>2.12.1,<2.13.0
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.12.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.13.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.13.1
          tensorflow>2.13.1,<2.14.0
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.13.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.14.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that any of:
          tensorflow<2.14.1
          tensorflow>2.14.1,<2.15.0
       cannot be used.
      And because tensorflow==2.14.1 is unusable because no wheels are available with a matching Python ABI and tensorflow==2.15.0 is unusable because no wheels are available with a
      matching Python ABI, we can conclude that tensorflow<2.15.0.post1 cannot be used.
      And because tensorflow==2.15.0.post1 is unusable because no wheels are available with a matching Python ABI and you require tensorflow, we can conclude that the requirements are
      unsatisfiable.

      hint: Pre-releases are available for tensorflow in the requested range (e.g., 2.15.0rc1), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

</details>

If somebody wants me to try something, just tell! 😄 

---

_Comment by @omdaniel on 2024-02-20 14:55_

Just to add some more context to this, I get the same error and it will fail on different packages on each attempt if running uv clean in between

```
(.venv) PS C:\Program Files\envs\naduv> uv pip install ipython

Resolved 16 packages in 1.68s

error: Failed to download distributions
  Caused by: Failed to fetch wheel: prompt-toolkit==3.0.43
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Program Files\envs\pipx\venvs\venvs\uv\uvcache\.tmpvKXHjN to \\?\C:\Program Files\envs\pipx\venvs\venvs\uv\uvcache\archive-v0\VF2P3BmAtyIqJo8V9e9Jb
  Caused by: Access is denied. (os error 5)

(.venv) PS C:\Program Files\envs\naduv> uv clean

Clearing cache at: C:\Program Files\envs\pipx\venvs\venvs\uv\uvcache
Removed 369 files (3.6MiB)

(.venv) PS C:\Program Files\envs\n\n\n\naduv> uv pip install ipython

Resolved 16 packages in 1.77s

error: Failed to download distributions
  Caused by: Failed to fetch wheel: pygments==2.17.2
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Program Files\envs\pipx\venvs\venvs\uv\uvcache\.tmpA8BLxj to \\?\C:\Program Files\envs\pipx\venvs\venvs\uv\uvcache\archive-v0\xoGgYMWlHY-hrr7FtBrhm
  Caused by: Access is denied. (os error 5)
```

I suspect something in the temp file naming is randomly causing issues and after I tried 5 times with "uv clean" in-between it finished but it is happening frequently enough that there is a real issue. I am using the latest uv release 0.15 at the time of posting this issue

---

_Comment by @omdaniel on 2024-02-20 15:44_

For many packages I can just run uv pip install [package] again without using "uv clean" and it will work without the "os error 5" but with numpy 1.26.4 it triggers every time and I tried it 15 times in a row

---

_Comment by @charliermarsh on 2024-02-20 15:55_

I think this is an error when attempting to persist the unzipped wheel to the wheel cache?

---

_Label `windows` added by @MichaReiser on 2024-02-20 17:11_

---

_Comment by @omdaniel on 2024-02-20 18:44_

> I think this is an error when attempting to persist the unzipped wheel to the wheel cache?

The contents of the temp folder in $UV_CACHE_DIR are unzipped after the install fails but not sure if the process is asking the os to move the contents before the temp folder is created

---

_Comment by @charliermarsh on 2024-02-20 18:46_

The overall approach is: unzip into a temporary folder, then move that folder into the cache to persist it. (This avoids persisting incomplete downloads that may error partway through.)

---

_Comment by @omdaniel on 2024-02-22 16:22_

I think this issue may crop up in corporate or more secure environments that have some layer of security running over all processes. I think there is a slight lag between when uv unzips the files where that process is ran in a sandbox and there is a slight delay before the os will allow access to the resulting folder while its scanned, I think simply catching the "os error" and retrying with some timeout would prevent this error on systems that have some security layer that temporarily effects permissions and also why a package like numpy (big and many small files) would consistently trip and medium packages would occasionally go through and occasionally trip.

i.e. you may not be able to re-produce this error in dev without setting up a test box that use one of these paid corporate security software to trigger it. You generally only create windows binaries on tagged releases, if you can create a dev build binary I can test it on my system

---

_Comment by @Wofiel on 2024-02-22 19:21_

> I think this issue may crop up in corporate or more secure environments that have some layer of security running over all processes.

Ah, this is consistent with the experience in the first post.

---

_Comment by @omdaniel on 2024-02-27 14:42_

@Wofiel Our issue may be too niche to get traction (which I don't disagree with at this stage of uv) time to learn rust and attempt a PR

---

_Comment by @nathanjmcdougall on 2024-02-28 23:52_

> > I think this issue may crop up in corporate or more secure environments that have some layer of security running over all processes.
> 
> Ah, this is consistent with the experience in the first post.

This is also consistent with my experience.

FWIW, the versions that it tripped up on were `debugpy==1.8.1` and `markdown-it-py==3.0.0`.

---

_Comment by @mkleinbort-ic on 2024-02-29 13:13_

I hit this same error today.

trying to run: 

```
 python -m uv pip install --system -r pyproject.toml --prerelease=allow
```

on windows.

I keep hitting

```
error: Failed to download and build: {SOME_PACKAGE_MAME}=={VERSION}
  Caused by: Failed to write to the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\{USERNAME}\AppData\Local\Temp\.tmplED7gi\.tmpxkU2s1\{SOME_PACKAGE_MAME}-{VERSION} to \\?\C:\Users\{USERNAME}\AppData\Local\Temp\.tmplED7gi\built-wheels-v0\pypi\{SOME_PACKAGE_MAME}\{VERSION}\wwYdTWRMDuqFPxs7feX1m\{SOME_PACKAGE_MAME}-{VERSION}.tar.gz
  Caused by: Access is denied. (os error 5)

```

The package that causes the error seems to be random if I re-run the command.

---

_Comment by @konstin on 2024-02-29 15:53_

Is anyone of you able to share (here or in private to konsti@astral.sh) what kind of security software or special windows settings you are running, so we can reproduce the failures you are experiencing?

---

_Comment by @mkleinbort-ic on 2024-03-02 14:51_

I don't think there is anything special on my setup.

1. Runing gitbash
2. On windows 11
3. running `python -m pip install -r requirements.txt` works
4. running `python -m uv pip install --system -r requirements.txt` fails

---

_Comment by @charliermarsh on 2024-03-02 14:53_

Is it possible that the use of the UNC prefix is itself the problem?

If I created a branch that omitted them, would anyone here be willing to `cargo build` locally and test it?

---

_Comment by @mkleinbort-ic on 2024-03-02 14:56_

I could try it in the week - but don't have the tooling set up at the moment.

---

_Comment by @mkleinbort-ic on 2024-03-02 14:57_

Do you mean this: `"\\?\C:\..."`?

---

_Comment by @charliermarsh on 2024-03-02 15:00_

Yeah, that prefix on the file paths.

---

_Comment by @mkleinbort-ic on 2024-03-02 15:08_

I don't know enough to help - but I definitely can't cd into "\\?\C:\" using powershell or bash 

in powershel:
this works: `cd C:\`
this does not `cd \\?\C:\`
```
PS C:\> cd \\?\C:\
cd : Cannot process argument because the value of argument "path" is not valid. Change the value of the "path"
argument and run the operation again.
At line:1 char:1
+ cd \\?\C:\
+ ~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Set-Location], PSArgumentException
    + FullyQualifiedErrorId : Argument,Microsoft.PowerShell.Commands.SetLocationCommand

```

---

_Comment by @charliermarsh on 2024-03-02 15:17_

Just curious -- what if you take the output of `pwd`, and then `ls` that prefixed with `\\?\`?

---

_Comment by @charliermarsh on 2024-03-02 15:17_

Either way I'll put together a branch to remove these.

---

_Comment by @mkleinbort-ic on 2024-03-02 15:19_

![image](https://github.com/astral-sh/uv/assets/112868935/9b620d50-f3ec-4d81-8401-567fb9bc9f89)


---

_Comment by @gregbedwell on 2024-03-02 15:32_

The \\\\?\ prefix is just a way around the MAX_PATH (256 char) limitation in Windows API calls. I'd personally be surprised if it had an impact although I couldn't say for sure.

Having dealt with similar issues in other projects where Windows antivirus software is holding file locks after any file system operations, the best solution is usually just to retry the operation on failure a handful of times with a slightly increasing sleep in between attempts unfortunately. 

---

_Comment by @mkleinbort on 2024-03-07 10:17_

That's interesting - though retrying multiple times sort of defeats the purpose of UV being so fast. 

I wonder why pip success but UV fails. 

(Note we do use an antivirus) 

---

_Comment by @omdaniel on 2024-03-07 14:24_

> That's interesting - though retrying multiple times sort of defeats the purpose of UV being so fast.
> 
> I wonder why pip success but UV fails.
> 
> (Note we do use an antivirus)

I think the effect on speed would be imperceptible to the end user. The uv speed up is more to do with dependency resolution and retrying is to deal with likely milliseconds of time that a security layer on Windows locks access to the extracted zip contents before uv can read and copy the data from the tmp folder to the archive

---

_Comment by @mkleinbort-ic on 2024-03-07 14:36_

Oh, yes, happy if this is handled by uv

I just don't want to write a bash script on my end to retry the installation till it works

---

_Comment by @charliermarsh on 2024-03-07 14:41_

Definitely want to fix this.

---

_Comment by @charliermarsh on 2024-03-09 14:52_

Oh interesting, I just noticed that this is _always_ related (except for one comment here) to persisting to `archive-v0`. But it succeeds if you rerun?

---

_Comment by @charliermarsh on 2024-03-09 15:01_

I really need to be able to reproduce this in order to fix it. (The answer might be to retry with a timeout, but even that needs testing, and I need to determine the appropriate durations.) Can anyone share what antivirus they might be running that would trigger this?

---

_Comment by @charliermarsh on 2024-03-09 15:27_

Installed Avast but couldn't reproduce.

---

_Comment by @omdaniel on 2024-03-09 17:53_

I used watchdog to see what files a being touched and it shows that numpy always fails when it gets to 37 MB libopenblas64 dll unsurprisingly

---

_Comment by @omdaniel on 2024-03-09 17:58_

Maybe a mock test can be made that ensures uv pip install is tolerant to a file temporarily being locked for a few milliseconds

---

_Comment by @mattibo on 2024-03-13 09:58_

I got the same issue and i use Trellix and Zscaler.

---

_Comment by @omdaniel on 2024-03-13 12:54_

Looking through documentation for Trellix, this may be triggered by https://docs.trellix.com/bundle/application-change-control-8.3.x-product-guide-windows/page/GUID-EE9619ED-4BAE-4A2D-9F5B-1D613B197F8C.html where the newly created (unzipped) DLL files are temporarily read/write protected while a determination is being made, ultimately an "allow" decision is made but uv tries to move those files while they are temporarily blocked, this may be highly architecturally dependent as there seems to be some communication between the end user client PC and the ePO policy server and additional latency there may impact the the frequency of this bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-13 16:04_

---

_Label `bug` added by @charliermarsh on 2024-03-13 16:04_

---

_Comment by @charliermarsh on 2024-03-13 16:05_

I will own this.

---

_Comment by @charliermarsh on 2024-03-13 16:15_

This will take some trial and error. I'll add some improvements in the next release, and then look to folks in this thread for testing.

---

_Comment by @mkleinbort-ic on 2024-03-14 00:36_

Tested on uv==0.1.19 (with the exponential backoff). So far all good - no issues on my setup.

If it breaks again I'll comment here, but it seems to be fixed

---

_Comment by @charliermarsh on 2024-03-14 01:07_

I'll optimistically close, but can re-open if anything comes up.

---

_Closed by @charliermarsh on 2024-03-14 01:07_

---

_Comment by @robmcd on 2024-03-14 04:24_

Upgrading to uv==0.1.20 fixed this for me.

---

_Comment by @charliermarsh on 2024-03-14 04:45_

Great, thank you!

---

_Comment by @mattibo on 2024-03-14 07:10_

for me most of the packages synced fine with version uv==0.1.20! But i still have issues with one package of i think bigger size (https://squidfunk.github.io/mkdocs-material/).

This leads to following error:

```
Resolved 1 package in 4ms
DEBUG No cache entry for: https://files.pythonhosted.org/packages/64/dc/9817689e3500b8e3967302201341dda7878f33d4ad089f9deb1126a091d7/mkdocs_material-9.5.13-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/64/dc/9817689e3500b8e3967302201341dda7878f33d4ad089f9deb1126a091d7/mkdocs_material-9.5.13-py3-none-any.whl
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
WARN Retrying rename from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0 due to transient error: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
error: Failed to download distributions
  Caused by: Failed to fetch wheel: mkdocs-material==9.5.13
  Caused by: Failed to read from the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\x\AppData\Local\uv\cache\.tmpXVEqoW to \\?\C:\Users\x\AppData\Local\uv\cache\archive-v0\ivw1_SgSoJofW7-c2dlZ0
  Caused by: Zugriff verweigert (os error 5)
```

---

_Comment by @ofek on 2024-03-14 13:21_

Did this happen yet? https://github.com/astral-sh/uv/issues/1491#issuecomment-1974827245

---

_Comment by @charliermarsh on 2024-03-14 13:23_

Nope.

---

_Comment by @ofek on 2024-03-14 13:24_

Ah okay, is there a tracking issue for that?

---

_Comment by @charliermarsh on 2024-03-14 13:39_

I can do it, I just haven’t seen evidence that it’s a problem.

---

_Comment by @charliermarsh on 2024-03-14 13:45_

To clarify: I understand that device paths can lead to confusing and incorrect behaviors. But we strip the device path prefix everywhere that matters (in my experience). So I'd like to understand why it would matter in the case above, for example.

---

_Comment by @ofek on 2024-03-14 13:50_

One compelling reason, to me at least, is confusion for users. Until Rust happened I had quite literally never seen a UNC path in the wild; it was just some abstract concept I read about in the Windows APIs.

cc @mitsuhiko I'd be curious to get your opinion on this

---

_Comment by @mkleinbort-ic on 2024-03-14 14:04_

I hit the issue again (on uv 0.1.20)

Running:

`python -m uv pip install --python 3.11 --reinstall pylance==0.10.3`

Resulted in 
```
error: failed to remove file `C:\Users\x\AppData\Local\Programs\Python\Python311\Lib\site-packages\numpy.libs/libopenblas64__v0.3.23-293-gc2f4bdbb-gcc_10_3_0-2bde3a66a51006b2b53eb373ff767a3f.dll`
  Caused by: Access is denied. (os error 5)
```
and re-running it resulted in 

```
error: Cannot uninstall package; RECORD file not found at: C:\Users\x\AppData\Local\Programs\Python\Python311\Lib\site-packages\numpy-1.26.4.dist-info\RECORD     
```

---

_Comment by @gregbedwell on 2024-03-14 14:29_

> One compelling reason, to me at least, is confusion for users. Until Rust happened I had quite literally never seen a UNC path in the wild; it was just some abstract concept I read about in the Windows APIs.
> 

Note that by removing the UNC prefix you're potentially introducing a regression if the path is longer than 256 characters. In practice I don't know whether this is likely here, but I'd be interested to know the motivating reason for adding them in the first place. Was it in response to a long path issue?

See this page for more info:
https://learn.microsoft.com/en-gb/windows/win32/fileio/naming-a-file?redirectedfrom=MSDN#win32-file-namespaces


---

_Comment by @ofek on 2024-03-14 14:34_

> Was it in response to a long path issue?

I'm not a maintainer but I know the answer: that is the behavior of the standard library function to resolve a path. From my understanding it was [basically a mistake](https://github.com/rust-lang/rust/issues/42869) but nobody can change it now. Instead, it's common to use the [dunce](https://crates.io/crates/dunce) crate or the [normpath](https://crates.io/crates/normpath) crate when you don't want to resolve symlinks.

edit: there is now also https://doc.rust-lang.org/std/path/fn.absolute.html on nightly

---

_Comment by @charliermarsh on 2024-03-14 14:50_

We do use `dunce` and strip the prefix anywhere that it interfaces outside of standard library filesystem operations.


---

_Comment by @charliermarsh on 2024-03-14 14:51_

@mkleinbort - Did you install that package prior to the latest `uv`? It could be leftover from your virtual environment being in a bad state.

---

_Comment by @charliermarsh on 2024-03-14 14:56_

I'm open to changing to use the `dunce`-canonicalized version everywhere, but note that it doesn't, like, fully solve the problem (if it is a problem). `dunce` will still return a UNC prefix if the path is longer than 260 characters.

---

_Comment by @omdaniel on 2024-03-14 19:29_

The merged PR #2419 has thus far eliminated failures on the handful pip installs I've done since upgrading. Thanks @charliermarsh 

---

_Comment by @mkleinbort-ic on 2024-03-15 10:43_

@charliermarsh 

Not sure - I hit this error just now trying to upgrade to uv==0.1.21 (I'm on 0.1.20)

![image](https://github.com/astral-sh/uv/assets/112868935/72aa2849-c959-4729-8adc-460b6d2e086d)

(Running on gitbash on a windows machine)

Running `pythom -m pip install uv==0.1.21` succeeded

---

_Comment by @FredStober on 2024-05-02 18:05_

Hi, I observed exactly this issue "Caused by: Failed to fetch wheel..."
when testing my dev environment on windows with uv==0.1.39.

The problem randomly appears on two different machines. One is running windows directly - and the other is using WSL with a windows mount. So far, the issue disappared after trying it another time. 

---

_Comment by @jbcpollak on 2024-06-14 18:21_

Ran into this issue with uv == 0.2.11, Python 3.12 and Windows 11 Pro. No specific enterprise extensions installed to Windows but we do have McAfee Antivirus installed.

---

_Comment by @axel-kah on 2024-06-27 15:01_

Hi,

the latest version (`uv 0.2.15 (bfc342da9 2024-06-24)`) still has these issues. We see this on a certain percentage every on our Windows CI runners. They do have `CrowdStrike` messing with our workloads unfortunately - corporate policy 😢 

The common denominator seems to be source distributions which `uv` tries to build wheels from. I've never seen this error for packages that are available as wheel.

Hypothesis: snakeoil scanners need more time to scan compressed archives (because of the decompression step) and `uv`s backoff algorithm just needs a little tweaking to accommodate for that.

A few source distribution only packages:  `fire`, `odfpy`, `mapdata`, `pywinusb`
<details>
<summary>A bunch of error messages for these packages</summary>

```shell
$ .\hatch.exe run all.py$INTERPRETER_VERSION`:cov
Creating environment: all.py3.11
Installing project
error: Failed to download and build `pywinusb==0.4.2`
  Caused by: Failed to write to the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmp3V6aqK\built-wheels-v3\.tmpsxHt5d\pywinusb-0.4.2 to \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmp3V6aqK\built-wheels-v3\index\70e6dbc41e17fe7c\pywinusb\0.4.2\w4S16F7Zw7fjYe2QmhqM_\pywinusb-0.4.2.zip
  Caused by: Access is denied. (os error 5)
  
$ .\hatch.exe run all.py$INTERPRETER_VERSION`:cov
Creating environment: all.py3.9
Installing Python distribution: 3.9
Installing project
Checking dependencies
Syncing dependencies
error: Failed to download and build `odfpy==1.4.1`
  Caused by: Failed to write to the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmp30woUu\built-wheels-v3\.tmp7d4zhJ\odfpy-1.4.1 to \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmp30woUu\built-wheels-v3\index\70e6dbc41e17fe7c\odfpy\1.4.1\J27cgvxiwwWDBSM2Tnauu\odfpy-1.4.1.tar.gz
  Caused by: Access is denied. (os error 5)

$ hatch run uv --version
Creating environment: default
Installing Python distribution: 3.11
Installing project in development mode
Checking dependencies
Syncing dependencies
error: Failed to download and build `mapdata==3.11.1`
  Caused by: Failed to write to the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmptBDtnM\built-wheels-v3\.tmpqFOiKl\mapdata-3.11.1 to \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmptBDtnM\built-wheels-v3\index\70e6dbc41e17fe7c\mapdata\3.11.1\T7WG4FPsti03XVV0KCULn\mapdata-3.11.1.tar.gz
  Caused by: Access is denied. (os error 5)

$ hatch run uv --version
Creating environment: default
Installing Python distribution: 3.11
Installing project in development mode
Checking dependencies
Syncing dependencies
error: Failed to download and build `fire==0.6.0`
  Caused by: Failed to write to the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpVXpz23\built-wheels-v3\.tmpnCevMt\fire-0.6.0 to \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpVXpz23\built-wheels-v3\index\70e6dbc41e17fe7c\fire\0.6.0\pD3XrlBXA8_x3S3TYSW2o\fire-0.6.0.tar.gz
  Caused by: Access is denied. (os error 5)
  ```
</details>

---

_Reopened by @zanieb on 2024-06-27 15:09_

---

_Comment by @johannesloibl on 2024-06-27 15:11_

Never saw this kind of responsiveness in a project, respect! 🥇 

---

_Comment by @zanieb on 2024-06-27 20:52_

@axel-kah With `--verbose` logs do you see logs indicating that we are retrying? e.g. "Retrying rename from...."

We have a maximum backoff of 10s which is pretty long. Do you think we should be waiting longer than that?

---

_Comment by @axel-kah on 2024-06-27 22:37_

@zanieb I modified the hatch script to install source-only packages with the latest `uv` and it seems it does not use any retry at all. Maybe that's the root cause.

<details>
<summary>verbose output</summary>

```shell
$ hatch run provoke-issue
Creating environment: default
Installing project in development mode
Checking dependencies
Syncing dependencies
cmd [1] | D:\glb\GwsU8_zK\0\uv.exe --version
uv 0.2.15 (bfc342da9 2024-06-24)
cmd [2] | D:\glb\GwsU8_zK\0\uv.exe --verbose pip install fire pywinusb nptdms odfpy ipyvuetable
DEBUG uv 0.2.15
DEBUG Searching for Python interpreter in system toolchains
DEBUG Found cpython 3.11.9 at `d:\glb\GwsU8_zK\0\.hatch_data\env\virtual\foo\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.11.9 environment at .hatch_data\env\virtual\foo\Scripts\python.exe
DEBUG Acquired lock for `.hatch_data\env\virtual\foo`
DEBUG At least one requirement is not satisfied: ipyvuetable
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: fire*
DEBUG Adding direct dependency: pywinusb*
DEBUG Adding direct dependency: nptdms*
DEBUG Adding direct dependency: odfpy*
DEBUG Adding direct dependency: ipyvuetable*
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/simple/fire/
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/simple/pywinusb/
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/simple/nptdms/
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/simple/odfpy/
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/simple/ipyvuetable/
DEBUG Acquired lock for `\\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\index\70e6dbc41e17fe7c\pywinusb\0.4.2`
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/packages/packages/38/b4/ecce4a3a0dac3b1bf5776943530cf4f36406fc9b3f4f3c31c8dcab2249eb/pywinusb-0.4.2.zip#sha256=e2f5e89f7b74239ca4843721a9bda0fc99014750630c189a176ec0e1b35e86df
DEBUG Acquired lock for `\\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\index\70e6dbc41e17fe7c\odfpy\1.4.1`
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/packages/packages/97/73/8ade73f6749177003f7ce3304f524774adda96e6aaab30ea79fd8fda7934/odfpy-1.4.1.tar.gz#sha256=db766a6e59c5103212f3cc92ec8dd50a0f3a02790233ed0b52148b70d3c438ec
DEBUG Searching for a compatible version of fire (*)
DEBUG Selecting: fire==0.6.0 (fire-0.6.0.tar.gz)
DEBUG Acquired lock for `\\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\index\70e6dbc41e17fe7c\fire\0.6.0`
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/packages/packages/1b/1b/84c63f592ecdfbb3d77d22a8d93c9b92791e4fa35677ad71a7d6449100f8/fire-0.6.0.tar.gz#sha256=54ec5b996ecdd3c0309c800324a0703d6da512241bc73b553db959d98de0aa66
DEBUG Acquired lock for `\\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\index\70e6dbc41e17fe7c\nptdms\1.9.0`
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/packages/packages/ff/05/8f560020155c1843d664248fb114e33eac0c1b3ad44fce6bfc2b5dd143c2/npTDMS-1.9.0.tar.gz#sha256=0e65c237e9d50b9b8e162b9c34171353a5ea05f4019c99c3e8ebc00722361cbc
DEBUG Downloading source distribution: odfpy==1.4.1
DEBUG Downloading source distribution: fire==0.6.0
DEBUG Downloading source distribution: nptdms==1.9.0
DEBUG Acquired lock for `\\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\index\70e6dbc41e17fe7c\ipyvuetable\0.4.0`
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/packages/packages/7f/dc/aeb142c5ab3563a8600c8d2409212d9ff7f869ff87adea1e1de252032f08/ipyvuetable-0.4.0.tar.gz#sha256=d88e4d87be451b3813e18a999bd3bb5800db38ba59c612e943029fd0804482e2
DEBUG Downloading source distribution: ipyvuetable==0.4.0
DEBUG Downloading source distribution: pywinusb==0.4.2
DEBUG Preparing metadata for: ipyvuetable==0.4.0
DEBUG No static `PKG-INFO` available for: ipyvuetable==0.4.0 (PkgInfo(UnsupportedMetadataVersion("2.1")))
DEBUG No static `pyproject.toml` available for: ipyvuetable==0.4.0 (MissingPyprojectToml)
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG No cache entry for: https://artifactory.acme.com/artifactory/api/pypi/pypi-acme-vir/simple/setuptools/
DEBUG Preparing metadata for: fire==0.6.0
DEBUG No static `PKG-INFO` available for: fire==0.6.0 (PkgInfo(UnsupportedMetadataVersion("2.1")))
DEBUG No static `pyproject.toml` available for: fire==0.6.0 (MissingPyprojectToml)
INFO Ignoring empty directory
DEBUG Preparing metadata for: nptdms==1.9.0
DEBUG No static `PKG-INFO` available for: nptdms==1.9.0 (PkgInfo(UnsupportedMetadataVersion("2.1")))
DEBUG No static `pyproject.toml` available for: nptdms==1.9.0 (PyprojectToml(FieldNotFound("project")))
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: setuptools*
error: Failed to download and build `pywinusb==0.4.2`
  Caused by: Failed to write to the distribution cache
  Caused by: failed to rename file from \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\.tmpc3E2Z7\pywinusb-0.4.2 to \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\index\70e6dbc41e17fe7c\pywinusb\0.4.2\or1SrhePMDCdKnzdyzMki\pywinusb-0.4.2.zip
  Caused by: Access is denied. (os error 5)
```
  </details>

I also set `RUST_LOG=trace` and found only 1 or 2 instances of "Retrying rename from...." - but only during the initialization of the venv by `hatch` and not when `uv` installs the source-only packages.

EDIT: same is true for `uv 0.2.17 (2eb1e6693 2024-06-26)` (you guys are moving fast 😉 )

---

_Comment by @zanieb on 2024-06-27 22:54_

Thanks for the details! I'll investigate.

---

_Comment by @zanieb on 2024-06-27 23:05_

And I believe I've found a suspicious line :) if you want to give it a try https://github.com/astral-sh/uv/pull/4605

edit: and another suspicious call addressed in https://github.com/astral-sh/uv/pull/4606


---

_Comment by @thecityofguanyu on 2024-07-15 13:18_

Just want to share that we still see these failures quite regularly as of `uv 0.2.21 (ebfe6d8fc 2024-07-03)`
```
[2024-07-13T08:03:24.537Z] error: Failed to download and build `conan==2.4.1`
[2024-07-13T08:03:24.537Z]   Caused by: Failed to write to the distribution cache
[2024-07-13T08:03:24.537Z]   Caused by: failed to rename file from \\?\D:\Jenkins\workspace\shared\temp\.tmp3Js7SG\built-wheels-v3\.tmpIcR0og\conan-2.4.1 to \\?\D:\Jenkins\workspace\shared\temp\.tmp3Js7SG\built-wheels-v3\index\f85683082b98592e\conan\2.4.1\rxFYsdKad4Hg7UOBh8zRC\conan-2.4.1.tar.gz
[2024-07-13T08:03:24.537Z]   Caused by: Access is denied. (os error 5)
```

---

_Comment by @omdaniel on 2024-07-15 14:54_

> Just want to share that we still see these failures quite regularly as of `uv 0.2.21 (ebfe6d8fc 2024-07-03)`
> 
> ```
> [2024-07-13T08:03:24.537Z] error: Failed to download and build `conan==2.4.1`
> [2024-07-13T08:03:24.537Z]   Caused by: Failed to write to the distribution cache
> [2024-07-13T08:03:24.537Z]   Caused by: failed to rename file from \\?\D:\Jenkins\workspace\shared\temp\.tmp3Js7SG\built-wheels-v3\.tmpIcR0og\conan-2.4.1 to \\?\D:\Jenkins\workspace\shared\temp\.tmp3Js7SG\built-wheels-v3\index\f85683082b98592e\conan\2.4.1\rxFYsdKad4Hg7UOBh8zRC\conan-2.4.1.tar.gz
> [2024-07-13T08:03:24.537Z]   Caused by: Access is denied. (os error 5)
> ```

I wonder in this case if the back off procedure is being used in the case where a temp build environment is being created to build a wheel for a package in isolation. This seems like in the case where an isolated build is needed the back off approach is not being used.

I have on different issue related to the temp build directory tries to run executables where IT blocks any user executable not under "Program Files", and I couldn't figure out how to override the location were the temp build environment is created (still haven't figured it out)

But that issue gave me insight into what may be going on here which may be a control flow were the back off is not being applied when trying to copy the compressed package to the archive

---

_Comment by @thecityofguanyu on 2024-07-15 15:49_

I recorded a failure through Process Monitor in case any of that information is useful here. I can't share it directly (because it came from a corporate environment), but I can redact and provide pieces. Just noting in case this for whatever reason cannot be replicated by the codeowners.

One item of note is that I don't think I see a retry? All I can find is a single `SetRenameInformationFile` with a corresponding `ACCESS DENIED`. This was from testing with `uv 0.2.23 (4bc36c0cb 2024-07-08)`.

```
11:27:28.2285886 AM	uv.exe	11604	SetRenameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0	ACCESS DENIED	ReplaceIfExists: True, FileName: C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\index\f85683082b98592e\conan\2.5.0\Ac6VfyuHm-z0Y9-KNpzw5\conan-2.5.0.tar.gz
```

The command run to generate above was `uv pip install -r requirements.txt --reinstall --no-cache`.

Our CI runs with `--vvv` ever since first encountering this error. Anecdotally (logs aren't in front of me), it seemed like it was *previously* going through a handful of retries before sometimes failing. Now I don't see it doing that anymore.

---

_Comment by @charliermarsh on 2024-07-15 15:49_

Yeah my guess is we're missing it somewhere but not sure where... I think @zanieb is planning to take a second look.

---

_Comment by @thecityofguanyu on 2024-07-15 16:03_

Looks like @zanieb already pushed a PR and may have found a fix, but should it be useful, here's the IO activity of the thread from https://github.com/astral-sh/uv/issues/1491#issuecomment-2228830850

```
11:27:27.3794232 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\api\subapi\remove.py	SUCCESS	Offset: 0, Length: 2,176, Priority: Normal
11:27:27.4956009 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\cli\commands\download.py	SUCCESS	Offset: 0, Length: 3,256, Priority: Normal
11:27:27.5824242 AM	uv.exe	11604	CreateFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\cli\commands\search.py	SUCCESS	Desired Access: Generic Write, Read Attributes, Disposition: Create, Options: Synchronous IO Non-Alert, Non-Directory File, Open Reparse Point, Attributes: n/a, ShareMode: Read, Write, Delete, AllocationSize: 0, OpenResult: Created
11:27:27.5828526 AM	uv.exe	11604	QueryNameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\cli\commands\search.py	BUFFER OVERFLOW	Name: \Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan
11:27:27.5828966 AM	uv.exe	11604	QueryNameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\cli\commands\search.py	SUCCESS	Name: \Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\cli\commands\search.py
11:27:27.5829343 AM	uv.exe	11604	QueryBasicInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\cli\commands\search.py	SUCCESS	CreationTime: 7/15/2024 11:27:27 AM, LastAccessTime: 7/15/2024 11:27:27 AM, LastWriteTime: 7/15/2024 11:27:27 AM, ChangeTime: 7/15/2024 11:27:27 AM, FileAttributes: A
11:27:27.6779132 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\internal\cache\conan_reference_layout.py	SUCCESS	Offset: 0, Length: 3,802, Priority: Normal
11:27:27.7193186 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\test\assets\genconanfile.py	SUCCESS	Offset: 0, Length: 8,192, Priority: Normal
11:27:27.7570835 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\tools\apple\xcodedeps.py	FAST IO DISALLOWED	Offset: 8,192, Length: 8,047
11:27:27.7571054 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\tools\apple\xcodedeps.py	SUCCESS	Offset: 8,192, Length: 8,047, Priority: Normal
11:27:27.7995744 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\tools\cmake\cmakedeps\templates\target_data.py	SUCCESS	Offset: 16,384, Length: 4,231
11:27:27.8362180 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\tools\files\packager.py	SUCCESS	Offset: 0, Length: 4,177, Priority: Normal
11:27:27.8763317 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\tools\google\toolchain.py	SUCCESS	Offset: 0, Length: 8,113, Priority: Normal
11:27:27.8764108 AM	uv.exe	11604	ReadFile	C:\$Extend\$UsnJrnl:$J:$DATA	SUCCESS	Offset: 41,971,712, Length: 88, I/O Flags: Non-cached, Paging I/O, Synchronous Paging I/O, Priority: Normal
11:27:27.9159855 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan\tools\premake\premakedeps.py	SUCCESS	Offset: 0, Length: 8,192, Priority: Normal
11:27:27.9610159 AM	uv.exe	11604	CreateFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\cache\__init__.py	SUCCESS	Desired Access: Generic Write, Read Attributes, Disposition: Create, Options: Synchronous IO Non-Alert, Non-Directory File, Open Reparse Point, Attributes: n/a, ShareMode: Read, Write, Delete, AllocationSize: 0, OpenResult: Created
11:27:27.9615439 AM	uv.exe	11604	QueryNameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\cache\__init__.py	BUFFER OVERFLOW	Name: \Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan
11:27:27.9615946 AM	uv.exe	11604	QueryNameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\cache\__init__.py	SUCCESS	Name: \Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\cache\__init__.py
11:27:27.9616186 AM	uv.exe	11604	QueryBasicInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\cache\__init__.py	SUCCESS	CreationTime: 7/15/2024 11:27:27 AM, LastAccessTime: 7/15/2024 11:27:27 AM, LastWriteTime: 7/15/2024 11:27:27 AM, ChangeTime: 7/15/2024 11:27:27 AM, FileAttributes: A
11:27:28.0134359 AM	uv.exe	11604	CreateFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\graph\compute_pid.py	SUCCESS	Desired Access: Generic Write, Read Attributes, Disposition: Create, Options: Synchronous IO Non-Alert, Non-Directory File, Open Reparse Point, Attributes: n/a, ShareMode: Read, Write, Delete, AllocationSize: 0, OpenResult: Created
11:27:28.0140592 AM	uv.exe	11604	QueryNameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\graph\compute_pid.py	BUFFER OVERFLOW	Name: \Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan
11:27:28.0141104 AM	uv.exe	11604	QueryNameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\graph\compute_pid.py	SUCCESS	Name: \Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\graph\compute_pid.py
11:27:28.0141272 AM	uv.exe	11604	QueryBasicInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\graph\compute_pid.py	SUCCESS	CreationTime: 7/15/2024 11:27:28 AM, LastAccessTime: 7/15/2024 11:27:28 AM, LastWriteTime: 7/15/2024 11:27:28 AM, ChangeTime: 7/15/2024 11:27:28 AM, FileAttributes: A
11:27:28.0494167 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\client\remote_manager.py	SUCCESS	Offset: 0, Length: 8,192, Priority: Normal
11:27:28.1010791 AM	uv.exe	11604	CreateFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model	SUCCESS	Desired Access: Read Data/List Directory, Synchronize, Disposition: Create, Options: Directory, Synchronous IO Non-Alert, Open Reparse Point, Attributes: N, ShareMode: Read, Write, AllocationSize: 0, OpenResult: Created
11:27:28.1013980 AM	uv.exe	11604	QueryNameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model	BUFFER OVERFLOW	Name: \Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conan
11:27:28.1014297 AM	uv.exe	11604	QueryNameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model	SUCCESS	Name: \Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model
11:27:28.1014421 AM	uv.exe	11604	QueryBasicInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model	SUCCESS	CreationTime: 7/15/2024 11:27:28 AM, LastAccessTime: 7/15/2024 11:27:28 AM, LastWriteTime: 7/15/2024 11:27:28 AM, ChangeTime: 7/15/2024 11:27:28 AM, FileAttributes: D
11:27:28.1014725 AM	uv.exe	11604	CloseFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model	SUCCESS	
11:27:28.1015137 AM	uv.exe	11604	IRP_MJ_CLOSE	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model	SUCCESS	
11:27:28.1323508 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model\profile.py	FAST IO DISALLOWED	Offset: 8,192, Length: 62
11:27:28.1323753 AM	uv.exe	11604	WriteFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0\conans\model\profile.py	SUCCESS	Offset: 8,192, Length: 62, Priority: Normal
11:27:28.2281959 AM	uv.exe	11604	CreateFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0	SUCCESS	Desired Access: Read Attributes, Delete, Synchronize, Disposition: Open, Options: Synchronous IO Non-Alert, Open Reparse Point, Attributes: n/a, ShareMode: Read, Write, Delete, AllocationSize: n/a, OpenResult: Opened
11:27:28.2283100 AM	uv.exe	11604	QueryAttributeTagFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0	SUCCESS	Attributes: D, ReparseTag: 0x0
11:27:28.2283283 AM	uv.exe	11604	QueryBasicInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0	SUCCESS	CreationTime: 7/15/2024 11:27:27 AM, LastAccessTime: 7/15/2024 11:27:28 AM, LastWriteTime: 7/15/2024 11:27:28 AM, ChangeTime: 7/15/2024 11:27:28 AM, FileAttributes: D
11:27:28.2284142 AM	uv.exe	11604	CreateFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\index\f85683082b98592e\conan\2.5.0\Ac6VfyuHm-z0Y9-KNpzw5	SUCCESS	Desired Access: Append Data/Add Subdirectory/Create Pipe Instance, Synchronize, Disposition: Open, Options: , Attributes: n/a, ShareMode: Read, Write, AllocationSize: n/a, OpenResult: Opened
11:27:28.2285886 AM	uv.exe	11604	SetRenameInformationFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0	ACCESS DENIED	ReplaceIfExists: True, FileName: C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\index\f85683082b98592e\conan\2.5.0\Ac6VfyuHm-z0Y9-KNpzw5\conan-2.5.0.tar.gz
11:27:28.2288963 AM	uv.exe	11604	CloseFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\index\f85683082b98592e\conan\2.5.0\Ac6VfyuHm-z0Y9-KNpzw5	SUCCESS	
11:27:28.2289789 AM	uv.exe	11604	IRP_MJ_CLOSE	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\index\f85683082b98592e\conan\2.5.0\Ac6VfyuHm-z0Y9-KNpzw5	SUCCESS	
11:27:28.2290180 AM	uv.exe	11604	CloseFile	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0	SUCCESS	
11:27:28.2290773 AM	uv.exe	11604	IRP_MJ_CLOSE	C:\Users\username\AppData\Local\Temp\2\.tmpcUIakH\built-wheels-v3\.tmpQYLAdW\conan-2.5.0	SUCCESS	
11:27:29.1293378 AM	uv.exe	11604	Thread Exit		SUCCESS	Thread ID: 10608, User Time: 0.0156250, Kernel Time: 0.0312500

```

---

_Comment by @charliermarsh on 2024-07-15 18:03_

Ok more fixes coming in the next release.

---

_Comment by @axel-kah on 2024-07-17 16:32_

> Ok more fixes coming in the next release.

I stress-tested `v0.2.25` on our windows CI by focussing on source distributions only. They all passed and instead of failing because files in the cache can't be renamed, I finally see retrying warnings like these:

```shell
WARN Retrying rename from \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpTFyAcI\built-wheels-v3\.tmpFqzuFu\pywinusb-0.4.2 
to \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpTFyAcI\built-wheels-v3\index\70e6dbc41e17fe7c\pywinusb\0.4.2\seIYCJmH_UpZwtxYdRGAe\pywinusb-0.4.2.zip due to transient error: 
failed to rename file from \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpTFyAcI\built-wheels-v3\.tmpFqzuFu\pywinusb-0.4.2 
to \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpTFyAcI\built-wheels-v3\index\70e6dbc41e17fe7c\pywinusb\0.4.2\seIYCJmH_UpZwtxYdRGAe\pywinusb-0.4.2.zip
```
🎉 
Thanks a lot for your effort!

---

_Comment by @zanieb on 2024-07-17 17:18_

Wonderful! Thanks for following up <3

---

_Closed by @zanieb on 2024-07-17 17:18_

---

_Comment by @paveldikov on 2024-07-24 11:46_

Clearly separate from the root cause here but since it's mentioned here:

> I'm open to changing to use the dunce-canonicalized version everywhere, but note that it doesn't, like, fully solve the problem (if it is a problem). dunce will still return a UNC prefix if the path is longer than 260 characters.

@charliermarsh are you still open to this change? We're hitting this problem with `pyvenv.cfg` (the `home` path in particular) -- importing dynamic libraries from UNC-prefixed paths fails miserably for at least some Python versions. Happy to open another issue.

---

_Comment by @charliermarsh on 2024-07-24 12:52_

Do you mind opening a separate issue with more details? That sounds like a bug (as opposed to a spurious failure).On Jul 24, 2024, at 7:46 AM, Pavel Dikov ***@***.***> wrote:﻿
Clearly separate from the root cause here but since it's mentioned here:

I'm open to changing to use the dunce-canonicalized version everywhere, but note that it doesn't, like, fully solve the problem (if it is a problem). dunce will still return a UNC prefix if the path is longer than 260 characters.

@charliermarsh are you still open to this change? We're hitting this problem with pyvenv.cfg (the home path in particular) -- importing dynamic libraries from UNC-prefixed paths fails miserably for at least some Python versions. Happy to open another issue.

—Reply to this email directly, view it on GitHub, or unsubscribe.You are receiving this because you were mentioned.Message ID: ***@***.***>

---

_Comment by @Super1Windcloud on 2024-09-20 07:10_

#   There is no doubt that this is a problem caused by hard links, and pnpm often has various problems



---

_Comment by @JackyZhangFuDan on 2025-01-06 09:41_

For your reference.

I met the same issue in windows when run 
uv python install 3.12

my solution: use CLI git bash instead of windows terminal. it works

---

_Comment by @wdscxsj on 2025-03-18 03:21_

Same issue with uv 0.6.6 on Windows 10. `link-mode = 'copy'` always works.

---

_Comment by @MarcCharron on 2025-11-07 15:38_

Hello,

I'm the author of the mypy issue [20200](https://github.com/python/mypy/issues/20200)
I tried with this configuration :
```toml
[tool.uv]
native-tls = true
link-mode = "copy"
```

But it still doesn't work in entreprise environnement (I have Trellix running on my computer).

---
