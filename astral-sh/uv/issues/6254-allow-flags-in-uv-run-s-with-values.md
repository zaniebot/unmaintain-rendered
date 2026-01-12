```yaml
number: 6254
title: "Allow flags in `uv run`'s  `--with` values"
type: issue
state: closed
author: mgaitan
labels:
  - cli
assignees: []
created_at: 2024-08-20T15:14:37Z
updated_at: 2024-08-20T19:04:48Z
url: https://github.com/astral-sh/uv/issues/6254
synced_at: 2026-01-12T15:59:02Z
```

# Allow flags in `uv run`'s  `--with` values

---

_@mgaitan_


In a Makefile, I run something like this (where `app_dir` is an argument):

```
uv run --isolated --with-requirements $(app_dir)/requirements/requirements-test.txt pytest $(app_dir)/tests
```

I also need to install `-e .` to make a few other packages in my monorepo available for the app. For legacy reasons, I can't add that as part of `requirements-test.txt`, so I would like to install it ad hoc in the `uv run` call.

I expected this to work:

```
uv run --isolated --with "-e ." --with-requirements $(app_dir)/requirements/requirements-test.txt pytest $(app_dir)/tests
```

but it fails like this:

```
error: Failed to parse: `-e .`
  Caused by: Expected package name starting with an alphanumeric character, found '-'
-e .
^
```

The workaround I found is this:

```
echo "-e ." >> /tmp/local.txt
uv run --isolated --with-requirements /tmp/local.txt --with-requirements $(app_dir)/requirements/requirements-test.txt pytest -vv $(app_dir)/tests
```

So, could `--with` be more flexible and accept flags? Actually, with this change, `--with-requirements` would be equivalent to `--with "-r <file>"`.


---

_Comment by @zanieb on 2024-08-20 15:27_

Thanks for the write-up!

What about `--with-editable`? I worry about the nested flags being really confusing to read.

---

_Comment by @zanieb on 2024-08-20 15:27_

Also uhh sorry but why not `--with .`? Why do they need to be editable?

---

_Label `cli` added by @zanieb on 2024-08-20 15:27_

---

_Comment by @mgaitan on 2024-08-20 15:58_

well that would work but isn't too inefficient? I have several fairly large packages in the monorepo that would be installed  by copying to an isolated virtualenv every time I run my tests, right?

---

_Comment by @zanieb on 2024-08-20 16:07_

It'd be good to have a concrete demonstration of it being inefficient, but yeah in theory an editable install would be faster.

---

_Comment by @charliermarsh on 2024-08-20 17:17_

I'm not too opposed to supporting editables here, though I'd also prefer `--with-editable`.

---

_Comment by @mgaitan on 2024-08-20 17:23_

`--with-editable` works for me

---

_Assigned to @zanieb by @zanieb on 2024-08-20 17:27_

---

_Closed by @zanieb on 2024-08-20 19:04_

---

_Closed by @zanieb on 2024-08-20 19:04_

---
