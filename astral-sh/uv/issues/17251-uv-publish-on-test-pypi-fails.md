```yaml
number: 17251
title: UV publish on test Pypi fails
type: issue
state: open
author: UbeyOzcan
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-12-29T14:25:24Z
updated_at: 2026-01-04T18:59:03Z
url: https://github.com/astral-sh/uv/issues/17251
synced_at: 2026-01-12T16:02:47Z
```

# UV publish on test Pypi fails

---

_@UbeyOzcan_

### Summary

Reproducing the package publication example but using `UV build` and `UV publish` : https://packaging.python.org/en/latest/tutorials/packaging-projects/

`UV publish` works perfectly on Pypi but keeps failing on testPypi **_with code 403 Forbidden  Server says: 403 Invalid or non-existent authentication information._** 

I have than try to upload using twine with this command :

`python3 -m twine upload --repository testpypi dist/*`

and that works perfectly.

Any idea why UV publish works perfectly on pypi and not on testpypi ? 

Thanks in advance !

pyproject.toml : 

```
[project]
name = "example_package_ubeyozcan"
version = "0.0.2"
authors = [
  { name="Example Author", email="author@example.com" },
]
description = "A small example package"
readme = "README.md"
requires-python = ">=3.9"
classifiers = [
    "Programming Language :: Python :: 3",
    "Operating System :: OS Independent",
]
license = "MIT"
license-files = ["LICEN[CS]E*"]

[project.urls]
Homepage = "https://github.com/pypa/sampleproject"
Issues = "https://github.com/pypa/sampleproject/issues"

[build-system]
requires = ["uv_build >= 0.9.18, <0.10.0"]
build-backend = "uv_build"

[[tool.uv.index]]
name = "example_package_ubeyozcan"
url = "https://test.pypi.org/simple/"
publish-url = "https://test.pypi.org/legacy/"
explicit = true
```

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.9.18 (0cee76417 2025-12-16)

### Python version

Python 3.13.5

---

_Label `bug` added by @UbeyOzcan on 2025-12-29 14:25_

---

_Comment by @EliteTK on 2025-12-29 15:38_

I can only reproduce this if I use an invalid token. This could be from the command line (`--token`), from the environment (`UV_PUBLISH_TOKEN`), from the credential store (`uv help auth`), etc.

One good way to figure out where it's coming would be to run `uv publish` with `-v` or `-vv`.

Feel free to post the output, but be careful not to accidentally publish your tokens (`uv` shouldn't print these, but you should double check anyway).

---

_Comment by @woodruffw on 2025-12-29 16:10_

In addition to what @EliteTK said, make sure you're not using `.pypirc` for your TestPyPI credentials, since `uv publish` doesn't support that: #7676

---

_Comment by @UbeyOzcan on 2025-12-29 21:29_

Thanks for the reply @EliteTK  and @woodruffw. 

@EliteTK  I will have a look with -v or -vv to check this.

@woodruffw I'm not using `.pypirc` as I was just trying to publish using only `uv` and i put the `TOKEN` manually in the command line.


---

_Label `needs-mre` added by @charliermarsh on 2026-01-04 18:59_

---
