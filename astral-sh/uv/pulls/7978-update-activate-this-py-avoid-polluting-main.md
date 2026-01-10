```yaml
number: 7978
title: "Update `activate_this.py`; avoid polluting `__main__`"
type: pull_request
state: closed
author: t-kalinowski
labels: []
assignees: []
base: main
head: fix-activate_this.py
created_at: 2024-10-07T16:23:15Z
updated_at: 2024-10-08T20:36:08Z
url: https://github.com/astral-sh/uv/pull/7978
synced_at: 2026-01-10T12:54:00Z
```

# Update `activate_this.py`; avoid polluting `__main__`

---

_Pull request opened by @t-kalinowski on 2024-10-07 16:23_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Currently, running `activate_this.py` leaves the following symbols in `__main__`:
```
abs_file annotations base bin_dir lib os path prev_length site sys
```
This PR updates the `activate_this.py` script so it does not leave any detritus in `__main__`.

## Test Plan

This could be tested with something like this:

```bash
activate_this=$(python -c '__import__("sys").exec_prefix + "/bin/activate_this.py"')
pollution=$(python -c "start = set(dir()); __import__('runpy').run_path($activate_this); print(start.difference(['start']+dir()))'

# if len(pollution), error
```


---

_Review requested from @konstin by @zanieb on 2024-10-07 17:06_

---

_Assigned to @konstin by @zanieb on 2024-10-07 17:06_

---

_Review comment by @j178 on `crates/uv-virtualenv/src/activator/activate_this.py`:34 on 2024-10-08 07:27_

Is this valid?

https://docs.python.org/3/reference/simple_stmts.html#future 

> A future statement must appear near the top of the module.

---

_@j178 reviewed on 2024-10-08 07:27_

---

_@t-kalinowski reviewed on 2024-10-08 12:25_

---

_Review comment by @t-kalinowski on `crates/uv-virtualenv/src/activator/activate_this.py`:34 on 2024-10-08 12:25_

Good catch, thank you! I removed it (it appears to be unused in the file).

---

_Comment by @konstin on 2024-10-08 12:46_

Hi! I've tried the reproducer command, but for me

```console
python -c "start = set(dir()); __import__('runpy').run_path('.venv/bin/activate_this.py'); print(start.difference(['start']+dir()))"
```

returns an empty set. I'm probably using a different setup though since `activate_this=$(python -c '__import__("sys").exec_prefix + "/bin/activate_this.py"')` resolves to an empty string in my bash on ubuntu 22.04 (`sys.exec_prefix` is `/usr` fwiw), could you tell me some more about your setup?

---

_Label `needs-mre` added by @konstin on 2024-10-08 12:46_

---

_Comment by @t-kalinowski on 2024-10-08 13:01_

Thanks. I see now that runpy does some additional isolation and doesn't execute directly in `__main__`.

In reticulate, we run the script using the Python C API: `PyRun_StringFlags()` (https://github.com/rstudio/reticulate/blob/d2c83b04ebc6fbecab057c369c2562adb332db67/src/python.cpp#L2749). 

Perhaps running the activate script in a new dict is more correct, 🤔. Still, that's old, and very exercised code, and this is the first time I've seen this issue. 

---

_Comment by @t-kalinowski on 2024-10-08 13:02_

But I see that `virtualenv` also takes a similar approach as here, https://github.com/pypa/virtualenv/blob/main/src/virtualenv/activation/python/activate_this.py. I'm starting to think the fix needs to be in reticulate.

---

_Label `needs-mre` removed by @konstin on 2024-10-08 16:55_

---

_Comment by @t-kalinowski on 2024-10-08 18:22_

I've updated reticulate, and this PR is no longer needed from our end. Feel free to close or merge (I think it's still a net improvement, but you have more context). Sorry for the noise!

---

_Closed by @konstin on 2024-10-08 20:36_

---
