```yaml
number: 6301
title: Conda install of ruff still at version 0.280
type: issue
state: closed
author: vigneshmanick
labels:
  - release
assignees: []
created_at: 2023-08-03T09:21:51Z
updated_at: 2023-08-03T17:03:27Z
url: https://github.com/astral-sh/ruff/issues/6301
synced_at: 2026-01-12T15:54:46Z
```

# Conda install of ruff still at version 0.280

---

_@vigneshmanick_

Hello,

The conda forge available version of [ruff](https://anaconda.org/conda-forge/ruff) is still at v0.280. The repository from which the data is generated seems to be https://github.com/conda-forge/ruff-feedstock  and the builds seem to have failed for both `0.281` and `0.282`. 

Unreleated but the links in the repo are not the current ones.

```
Home: https://github.com/charliermarsh/ruff
Development: https://github.com/charliermarsh/ruff
```

Thanks!



---

_Comment by @charliermarsh on 2023-08-03 15:25_

Thanks! I'm a bit confused because the PRs for those versions passed and were automerged: https://github.com/conda-forge/ruff-feedstock/pull/127. But then the builds on the merged commit failed, I guess? Due to upload issues? Hm.

---

_Comment by @zanieb on 2023-08-03 15:32_

For reference here's an upload log

```
  0%|          | 0.00/4.83M [00:00<?, ?B/s]
 31%|███       | 1.50M/4.83M [00:00<00:00, 15.7MB/s]
4.83MB [00:00, 19.7MB/s]                            
ERROR getting output validation information from the webservice: JSONDecodeError('Expecting value: line 1 column 1 (char 0)')
copy results:
{}
Failed to upload due to copy from staging to production channel failed. Trying again in 153.93679428100586 seconds
Traceback (most recent call last):
  File "/opt/conda/bin/upload_package", line 11, in <module>
    sys.exit(upload_package())
  File "/opt/conda/lib/python3.10/site-packages/click/core.py", line 1157, in __call__
    return self.main(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/click/core.py", line 1078, in main
    rv = self.invoke(ctx)
  File "/opt/conda/lib/python3.10/site-packages/click/core.py", line 1434, in invoke
    return ctx.invoke(self.callback, **ctx.params)
  File "/opt/conda/lib/python3.10/site-packages/click/core.py", line 783, in invoke
    return __callback(*args, **kwargs)
  File "/opt/conda/lib/python3.10/site-packages/conda_forge_ci_setup/build_utils.py", line 232, in upload_package
    retry_upload_or_check(
  File "/opt/conda/lib/python3.10/site-packages/conda_forge_ci_setup/upload_or_check_non_existence.py", line 317, in retry_upload_or_check
    raise TimeoutError("Did not manage to upload package.  Failing.")
TimeoutError: Did not manage to upload package.  Failing.
##[error]Bash exited with code '1'.

```

[source](https://dev.azure.com/conda-forge/feedstock-builds/_build/results?buildId=755231&view=logs&j=d0d954b5-f111-5dc4-4d76-03b6c9d0cf7e&t=6d4b912b-175d-51da-0fd9-4d30fe1eb4e7&l=1539)

---

_Comment by @charliermarsh on 2023-08-03 15:36_

I don't know how to "retry" the build.

---

_Comment by @zanieb on 2023-08-03 16:04_

We're resolving this with the conda-forge team. Thanks for the report!

---

_Label `release` added by @charliermarsh on 2023-08-03 16:24_

---

_Assigned to @zanieb by @charliermarsh on 2023-08-03 16:24_

---

_Comment by @zanieb on 2023-08-03 16:58_

This should be resolved (though it may take a second for everything to be available) please let us know if you have any problems!

---

_Closed by @zanieb on 2023-08-03 16:58_

---

_Comment by @vigneshmanick on 2023-08-03 17:03_

Thanks for the quick response. I can confirm that the installer now shows `v0.282` as the available version.

---
