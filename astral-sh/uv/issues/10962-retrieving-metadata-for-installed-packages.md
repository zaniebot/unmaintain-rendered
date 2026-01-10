---
number: 10962
title: Retrieving metadata for installed packages
type: issue
state: open
author: tjni
labels:
  - enhancement
assignees: []
created_at: 2025-01-25T17:53:01Z
updated_at: 2025-08-16T22:10:22Z
url: https://github.com/astral-sh/uv/issues/10962
synced_at: 2026-01-10T01:24:59Z
---

# Retrieving metadata for installed packages

---

_Issue opened by @tjni on 2025-01-25 17:53_

### Summary

I have a use case in which I'd like to programmatically work with the metadata for every installed package. My ideal workflow would be to run a single command:

```
❯ uv pip list --python <some python> --format=json --metadata-fields=name,keywords --requires
[
  {
    "name": "normalized-name",
    "version": "1.0.0",
    "metadata": {
      "name": "original_name",
      "keywords": "1,2,3"
    },
    "requires": [
      { "name": "normalized-dependency-name" }
    ]
  },
  ...
]
```
where the `--requires` option is something I'm proposing in https://github.com/astral-sh/uv/pull/10886.

When researching the history of this feature being asked for in pip, I stumbled across https://github.com/pypa/pip/pull/11097 and https://github.com/pypa/pip/issues/5261, in which the pip maintainers felt that adding this to `pip list` (or `pip show`) as JSON output was feature creep, and instead created `pip inspect`. Less optimal for my use case, but still quite reasonable (I can join this information in application code):

```
❯ uv pip list --python <some python> --format=json --requires
[
  {
    "name": "normalized-name",
    "version": "1.0.0",
    "requires": [
      { "name": "normalized-dependency-name" }
    ]
  },
  ...
]

❯ uv pip inspect --python <some python>
{
  "version": "1",
  "uv_version": "0.5.24",
  "installed": [
    {
      "metadata": {
        "name": "original_name",
        "keywords": "1,2,3",
        ...
    }
  ]
  "environment": {
    ...
  }
}
```

I feel that this feature request shares some overlap with:

- https://github.com/astral-sh/uv/issues/2526
- https://github.com/astral-sh/uv/issues/7532

### Example

_No response_

---

_Label `enhancement` added by @tjni on 2025-01-25 17:53_

---

_Comment by @Flimm on 2025-08-16 22:10_

You can get there using `pip show -v <packagename>`. Loop over all installed packages, and run:

     uv run -- pip show -v <packagename>

The verbose flag `-v` will print out additional fields like `Project-URLs`, `Classifiers` and `Entry-points`, assuming the package has that metadata.

To prevent problems, you need to make sure that `pip` is installed in your virtual environment. To do that you can run `uv add pip`. Or you can run `uv venv --seed` when creating the virtual environment. If you don't install `pip` in your virtual environment, the globally installed `pip` may be run instead, which may handle the global environment, rather than the virtual environment.

---
