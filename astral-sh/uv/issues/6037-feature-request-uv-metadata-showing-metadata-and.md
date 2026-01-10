```yaml
number: 6037
title: "Feature request: `uv metadata` showing metadata and entry points of any pypi package."
type: issue
state: open
author: zsimic
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-08-12T14:30:46Z
updated_at: 2026-01-08T18:05:27Z
url: https://github.com/astral-sh/uv/issues/6037
synced_at: 2026-01-10T03:11:31Z
```

# Feature request: `uv metadata` showing metadata and entry points of any pypi package.

---

_Issue opened by @zsimic on 2024-08-12 14:30_

It would be great if `uv` provided a command that could describe the metadata of a given python package. The output would be json, similar to how `cargo metadata` does it.

A proof of concept written in python is available in https://github.com/zsimic/uv-metadata

For example:

```
~: uv metadata cowsay
{
    "author": "Vaasudevan Srinivasan",
    "author_email": "vaasuceg.96@gmail.com",
    "classifiers": [
        "Operating System :: OS Independent",
        "Programming Language :: Python :: 3.8",
        "Programming Language :: Python :: 3.9",
        "Programming Language :: Python :: 3.10",
        "Programming Language :: Python :: 3.11",
        "Programming Language :: Python :: 3.12"
    ],
    "description_content_type": "text/markdown",
    "entry_points": {
        "console_scripts": {
            "cowsay": "cowsay.__main__:cli"
        }
    },
    "home_page": "https://github.com/VaasuDevanS/cowsay-python",
    "license": "GNU-GPL",
    "license_file": "LICENSE.txt",
    "metadata_version": "2.1",
    "name": "cowsay",
    "requires_python": ">=3.8",
    "summary": "The famous cowsay for GNU/Linux is now available for python",
    "top_level": [
        "cowsay"
    ],
    "version": "6.1"
}
```

Any valid package spec that would be accepted in a `requirements.txt` for example is accepted here as argument. For `cowsay` one could refer to it via its project url, or with a constraint and get the same outcome, for example:
- `uv metdata https://github.com/VaasuDevanS/cowsay-python.git`
- `uv metdata cowsay~=6.0`


---

_Label `enhancement` added by @charliermarsh on 2024-08-18 18:16_

---

_Label `cli` added by @charliermarsh on 2024-08-18 18:18_

---

_Renamed from "Feature request: `uv pip describe` showing metadata and entry points of any pypi package." to "Feature request: `uv describe` showing metadata and entry points of any pypi package." by @zsimic on 2025-02-17 01:16_

---

_Comment by @mikenerone on 2025-09-08 04:38_

Console scripts aren't the only "entry points". If this is implemented in some form (yes, please :D), then please show all entry points, for _all_ entry point groups, that the package might provide. Regular old `pip` already does this. `pip show -v black` (though I don't use `black` these days) provides a good example of the info that should be exposed (entry point group names, entry point names, where each one points, what extras each depends on if any):
```
> pip show -v black
...
Entry-points:
  [console_scripts]
  black = black:patched_main
  blackd = blackd:patched_main [d]
  
  [validate_pyproject.tool_schema]
  black = black.schema:get_schema
...
```

Any pytest plugin also serves as a good example of a non-console-script entry point.

---

_Comment by @Niedzwiedzw on 2026-01-08 12:40_

Hey, I'm willing to work on this issue - would the proposed structure work?

---

_Comment by @konstin on 2026-01-08 14:44_

That's too big of a feature for us to currently accept as a contribution, there's a bunch of questions around this such as fitting with the rest of the CLI interface.

---

_Comment by @zsimic on 2026-01-08 17:53_

I think it would be an awesome fit. A better name for the command is actually `uv metadata`, and it would output json only, similar (in a sense) to `cargo metadata`.

I have written a proof of concept in python here: https://github.com/zsimic/uv-metadata
It's one standalone script that uses `uv` in order to show metadata of a project (folder or pypi package name, or a constrained package name like `'urllib3<2.6'`).

I will update the description of this issue to mention `uv metadata` instead of `uv describe`

---

_Renamed from "Feature request: `uv describe` showing metadata and entry points of any pypi package." to "Feature request: `uv metadata` showing metadata and entry points of any pypi package." by @zsimic on 2026-01-08 17:54_

---

_Comment by @zsimic on 2026-01-08 18:05_

@mikenerone I've updated the description to focus on json only. Yes I agree this `uv metadata` command would show the package metadata ideally, so it would show all entry points (ideally:).

For `black` that would be:
```
~: uv metadata black
{
    "author_email": "\u0141ukasz Langa <lukasz@langa.pl>",
...
    "entry_points": {
        "console_scripts": {
            "black": "black:patched_main",
            "blackd": "blackd:patched_main [d]"
        },
        "validate_pyproject.tool_schema": {
            "black": "black.schema:get_schema"
        }
    },
}
...
```

---
