```yaml
number: 2533
title: Compatibility with GitHub CI python install action
type: issue
state: open
author: BrianSipos
labels: []
assignees: []
created_at: 2026-01-16T14:31:54Z
updated_at: 2026-01-16T14:32:40Z
url: https://github.com/astral-sh/ty/issues/2533
synced_at: 2026-01-16T14:57:38Z
```

# Compatibility with GitHub CI python install action

---

_@BrianSipos_

### Summary

I am running GitHub CI jobs which have a Python distribution installed by [actions/setup-python](https://github.com/marketplace/actions/setup-python), which configures a few environment variables and adds the python executables to the job `PATH`.

If the job later runs `ty` without any custom environemnt, it will fail to detect python packages installed under the `setup-python` target. As a workaround, I can add a job step to explicitly define `PYTHONPATH` to use the target directories as below:

```
    - name: Explicit PYTHONPATH
      run: echo "PYTHONPATH=${PWD}:${Python3_ROOT_DIR}/lib/python3.12/site-packages" >> $GITHUB_ENV
```

Maybe ty could use the python executable in normal path to derive an appropriate PYTHONPATH on its own. I thought that is what was happening already, but I suppose not because that explicit envar is currently needed.

### Version

ty 0.0.9

---

_Comment by @BrianSipos on 2026-01-16 14:32_

This is a distinct issue from #1323 about Python path for Debian/Ubuntu hosts.

---
