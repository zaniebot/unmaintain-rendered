```yaml
number: 1442
title: "Implement `--report` for `uv pip install`"
type: issue
state: open
author: edgarrmondragon
labels:
  - enhancement
assignees: []
created_at: 2024-02-16T06:16:55Z
updated_at: 2024-08-14T08:33:19Z
url: https://github.com/astral-sh/uv/issues/1442
synced_at: 2026-01-10T04:53:49Z
```

# Implement `--report` for `uv pip install`

---

_Issue opened by @edgarrmondragon on 2024-02-16 06:16_

From the `pip` docs:

> The `--report` option of the pip install command produces a detailed JSON report of what it did install (or what it would have installed, if used with the `--dry-run` option).

This produces a nice JSON report that's essentially `pip compile` but with a different and more detailed output format.

## Related

* https://github.com/astral-sh/uv/issues/1244

## References:

* https://pip.pypa.io/en/stable/cli/pip_install/#obtaining-information-about-what-was-installed
* https://pip.pypa.io/en/stable/reference/installation-report/

---

_Label `enhancement` added by @charliermarsh on 2024-02-16 06:18_

---

_Comment by @T-256 on 2024-02-17 11:36_

related https://github.com/astral-sh/uv/issues/411

---

_Comment by @yajo on 2024-06-25 11:50_

Do we have any current workaround for this?

---

_Comment by @sfc-gh-nsharma on 2024-06-25 14:41_

+1, this feature will be very useful for us for creating reproducible environments.

---

_Comment by @charliermarsh on 2024-06-25 14:42_

Can you expand on the use-case? Why would this be an output of `pip install` rather than `pip compile`?

---

_Comment by @sfc-gh-nsharma on 2024-06-25 14:54_

We are basically trying to get parity with https://pip.pypa.io/en/stable/cli/pip_install/#obtaining-information-about-what-was-installed. The goal is to get the list of files that will be downloaded along with their SHA hashes (without actually creating the environemtn). We could use that to recreate the exact environment for CI/CD

---

_Comment by @zanieb on 2024-06-25 15:07_

This sounds like the purpose of `uv pip compile` — I think this is just a flag in `pip install` because they don't have the `pip-compile` interface.

---

_Comment by @charliermarsh on 2024-06-25 15:45_

I think the difference is that compile won’t tell you the specific wheels or source distributions that you’d need to install for the current platform.

---

_Comment by @sfc-gh-nsharma on 2024-06-25 15:51_

Yes, `--dry-run` + `--report` has a very detailed machine-readable output.
https://pip.pypa.io/en/stable/reference/installation-report/#example

---

_Comment by @edgarrmondragon on 2024-06-25 15:54_

> Can you expand on the use-case? Why would this be an output of `pip install` rather than `pip compile`?

For us, we're PoCing a requirements-lock feature and we support two "venv backends":

* virtualenv + pip
* uv

For the first one we were planning to use the `--report` output to get the resolved direct dependencies. The benefit of having this feature in uv would be writing the parse logic only once (for the JSON output). Without this, we either need a dependency on `pip-tools` and parse the same requirements.txt style output from both tools, or write different parsing logic for pip and uv.

---

_Comment by @yajo on 2024-06-26 11:20_

My use case is that I package a huge project with lots of dependencies (a meta-repo; some are editable) using https://github.com/nix-community/dream2nix. In Nix world, all files must either be built offline or hash-verified if online. The locking process uses pip and is terribly slow. I'd love to speed it up with uv, but uv lacks the report feature, which is the one used by the lock process to produce valid nix FODs.

I checked by using `uv pip compile --generate-hashes`, but the output is hard to parse and also doesn't have details about what files use what hashes. It's just a list of hashes but I don't know which is which. Also it seems to build wheels for sdists, when I'd be happy with just the hash of the sdist as `pip --dry-run --report` gets, as the real build is done later by nix anyway.

---

_Comment by @charliermarsh on 2024-06-26 11:59_

> Also it seems to build wheels for sdists...

(Just as an aside: you often need to build a wheel in order to perform dependency resolution.)


---

_Comment by @yorickvP on 2024-08-14 08:33_

@yajo I implemented a uv solver with dream2nix at https://github.com/datakami/cognix/blob/main/modules/pip-uv/default.nix, but it requires a [patched uv](https://github.com/astral-sh/uv/compare/7dfb562585e75137226fa5e4dbcf248c816925c3...yorickvP:uv:yorickvp/dream2nix-resolver-2)

My patch isn't really upstreamable, but it would be great to instead have `--report` upstream.

---
