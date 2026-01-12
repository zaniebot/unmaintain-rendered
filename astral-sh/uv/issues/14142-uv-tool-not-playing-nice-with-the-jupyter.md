```yaml
number: 14142
title: "`uv tool` not playing nice with the \"jupyter\" metapackage and in general with packages providing executables via dependencies"
type: issue
state: open
author: callegar
labels:
  - wish
assignees: []
created_at: 2025-06-19T08:08:21Z
updated_at: 2025-06-25T20:07:31Z
url: https://github.com/astral-sh/uv/issues/14142
synced_at: 2026-01-12T16:01:43Z
```

# `uv tool` not playing nice with the "jupyter" metapackage and in general with packages providing executables via dependencies

---

_@callegar_

### Summary

Installing jupyterlab using `uv tool` makes sense, since jupyter knows how to "install" specific kernels.

The problem is that `uv tool` does not play very well with how jupyter is packaged.

If you do `uv tool install jupyter` which is supposed to be the "official" way to install jupyter, that fails because "there are no executables provided" by that package. This is because the package is a metapackage: the executables are provided via the dependencies.

Similarly `uv tool install jupyterlab` does not provide a `jupyter` executable.

Currently you need to work around the issue with `uv tool install jupyter-core --with jupyterlab` that is not that nice to use and remember.

### Platform

Manjaro

### Version

0.7.13

### Python version

3.13.5

---

_Label `bug` added by @callegar on 2025-06-19 08:08_

---

_Label `bug` removed by @charliermarsh on 2025-06-21 02:36_

---

_Label `wish` added by @charliermarsh on 2025-06-21 02:36_

---

_Comment by @charliermarsh on 2025-06-21 02:38_

Un-marking this as a bug since it doesn't seem like there's a bug here, more a request for these cases to be handled more seamlessly.

---

_Comment by @callegar on 2025-06-21 09:11_

Thanks for updating the status.

Maybe, could it be a possibility to assure that when the `tool` subcommand, is used, not only the executables for the binary itself but also those of the dependencies are considered? Alternatively, could a `--with-exes` be introduced for that?

Having to remember a combination of packages (whether a should be on `--with` and b as the main tool or viceversa) is always a bit of a usability problem.

---

_Comment by @stackfun on 2025-06-25 20:07_

+1 on `--with-exes` or something along those lines. I'd also like to install the executables of the transitive dependencies of my package.

I think the [uv tool documentation](https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies) is currently unclear when the executables will be added. In my testing, sometimes `uv tool install <my pkg> --with pytest` adds the pytest executable, in other cases, it does not. I'm unable to detect a pattern in uv's behavior here.

---
