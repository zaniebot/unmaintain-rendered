```yaml
number: 2533
title: Compatibility with GitHub CI python install action
type: issue
state: closed
author: BrianSipos
labels: []
assignees: []
created_at: 2026-01-16T14:31:54Z
updated_at: 2026-01-16T15:33:37Z
url: https://github.com/astral-sh/ty/issues/2533
synced_at: 2026-01-16T15:58:04Z
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

_Comment by @carljm on 2026-01-16 15:30_

Thanks for the report!

ty does not currently ever use the shell path to find a Python installation to use. I suspect, though, that you are installing ty into the Python environment you want to use, meaning that this would be fixed by #2068 .

I think as a workaround, explicitly setting `--python ${Python3_ROOT_DIR}` in your invocation of ty would be a bit simpler than setting `PYTHONPATH` to the site-packages directory, and more correct in that it would cause ty to actually find the Python installation and treat the site-packages directory as a site-packages dir.

---

_Comment by @carljm on 2026-01-16 15:33_

Going to go ahead and close this as duplicate of #2068 for now, as I think it's quite likely that's the fix here. Please comment if you are _not_ installing ty into the setup-python Python installation -- if so, this would represent a different use case we need to consider.

---

_Closed by @carljm on 2026-01-16 15:33_

---
