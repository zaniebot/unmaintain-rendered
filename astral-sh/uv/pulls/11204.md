```yaml
number: 11204
title: "Add docs for `uv pip compile` and renovate"
type: pull_request
state: open
author: zanieb
labels:
  - documentation
  - help wanted
assignees: []
draft: true
base: main
head: zb/renovate-compile
created_at: 2025-02-03T22:50:41Z
updated_at: 2025-09-12T13:47:11Z
url: https://github.com/astral-sh/uv/pull/11204
synced_at: 2026-01-10T06:36:15Z
```

# Add docs for `uv pip compile` and renovate

---

_Pull request opened by @zanieb on 2025-02-03 22:50_

Support landed upstream

---

_Label `documentation` added by @zanieb on 2025-02-03 22:50_

---

_Comment by @zanieb on 2025-02-03 22:56_

cc @mkniewallner (as an authority on these docs)

---

_Review comment by @mkniewallner on `docs/guides/integration/dependency-bots.md`:86 on 2025-02-03 23:09_

```suggestion
{
  $schema: "https://docs.renovatebot.com/renovate-schema.json",
  "pip-compile": {
    fileMatch: ["(^|/)requirements\\.txt$"],
  },
}
```

---

_Comment by @mkniewallner on 2025-02-03 23:51_

Tried the feature in Renovate (from `main` branch, as their GitHub app uses a version that predates the feature), but there are several things missing/not working.

First, it doesn't seem to support dependencies defined in `pyproject.toml` at all, as I've tried:

- https://github.com/mkniewallner/mkv-playground/commit/7c840df610e5ab50cbfaf560f856a428b9601442
- https://github.com/mkniewallner/mkv-playground/commit/205445f013c82ae242a5680ff2edf7ed99f28f73
- https://github.com/mkniewallner/mkv-playground/commit/aa0cbb9c2aa8c52201813ac06281e00c105d7a7c

and I ended up with different errors on the 3 attempts when running:

```shell
RENOVATE_TOKEN=<GITHUB_TOKEN> LOG_LEVEL=DEBUG npx renovate@latest mkniewallner/mkv-playground
```

With `uv pip compile pyproject.toml -o requirements.txt` I got:

```console
 WARN: pip-compile error (repository=mkniewallner/mkv-playground)
       "fileMatch": "requirements.txt",
       "errorMessage": "Option -o not supported (yet)"
```

With `uv pip compile pyproject.toml --output-file requirements.txt` I got:

```console
 WARN: pip-compile error (repository=mkniewallner/mkv-playground)
       "fileMatch": "requirements.txt",
       "errorMessage": "Option --output-file must have equal sign '=' separating it's argument"
```

With `uv pip compile pyproject.toml --output-file=requirements.txt` it seems that the header was at least fully handled, but then it showed that `pyproject.toml` does not look to be supported, and Renovate ended up "guessing" that the project uses Poetry and crashed as there is no `[tool]` section:

```console
DEBUG: pip-compile: extracted command from header (repository=mkniewallner/mkv-playground)
       "fileName": "requirements.txt",
       "argv": ["uv", "pip", "compile", "pyproject.toml", "--output-file=requirements.txt"],
       "commandType": "uv"
DEBUG: Poetry: error parsing pyproject.toml (repository=mkniewallner/mkv-playground, packageFile=pyproject.toml)
       "err": {
         "message": "Schema error",
         "stack": "ZodError: Schema error\n    at Object.get error [as error] (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/zod/lib/types.js:55:31)\n    at fromZodResult (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/util/result.ts:60:67)\n    at Function.parse (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/util/result.ts:566:12)\n    at Object.extractPackageFile (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/modules/manager/poetry/extract.ts:19:36)\n    at extractPackageFile (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/modules/manager/index.ts:75:9)\n    at getManagerPackageFiles (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/workers/repository/extract/manager-files.ts:58:43)\n    at /home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/workers/repository/extract/index.ts:57:28\n    at async Promise.all (index 4)\n    at extractAllDependencies (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/workers/repository/extract/index.ts:54:26)\n    at extract (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/workers/repository/process/extract-update.ts:160:28)\n    at extractDependencies (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/workers/repository/process/index.ts:158:26)\n    at Object.renovateRepository (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/workers/repository/index.ts:71:9)\n    at attributes.repository (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/workers/global/index.ts:206:11)\n    at start (/home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/workers/global/index.ts:191:7)\n    at /home/mathieu/.npm/_npx/59c0475bfd22776c/node_modules/renovate/lib/renovate.ts:19:22",
         "issues": {"tool": "Required"}
       }
 WARN: pip-compile: support for manager is not yet implemented (repository=mkniewallner/mkv-playground, packageFile=pyproject.toml)
       "manager": "pep621"
 WARN: pip-compile: failed to find dependencies in source file (repository=mkniewallner/mkv-playground, packageFile=pyproject.toml)
```

What did work though is defining dependencies in `requirements.in` (https://github.com/mkniewallner/mkv-playground/commit/9f8323abc2b81fe60ffa45cd8b1bc3a8ad8ce52a): https://github.com/mkniewallner/mkv-playground/pull/23/files. But I'm pretty confident it suffers from the same limitations as above, meaning:
- if `uv pip compile requirements.in -o requirements.txt` or `uv pip compile requirements.in --output-file requirements.txt` was used, Renovate won't be able to create updates
- if `uv pip compile requirements.in --output-file=requirements.txt` was used, it will though

So in the current state, I would not recommend announcing support for it. I can raise the issues I've found in Renovate repository and/or work on some of the issues/features.

If support for `-o <file>` and `--output-file <file>` ends up being added, we could at least announce that `requirements.in` is handled, but not `pyproject.toml`.

---

_@mkniewallner reviewed on 2025-02-03 23:52_

---

_Comment by @zanieb on 2025-02-04 00:03_

Thank you so much! Appreciate it.

I guess I should have tried it.. :)

---

_Converted to draft by @zanieb on 2025-02-04 00:03_

---

_Comment by @adrianeboyd on 2025-02-10 10:29_

Now that the github app has updated to a newer version of renovate, I tried the version of replacing `pip-compile` with `uv pip compile` in the headers (with `--output-file=`), and end up with this error:

```
ExecError: Command failed: uv pip compile requirements.in --output-file=requirements.txt
/bin/sh: 1: uv: not found

    at ChildProcess.<anonymous> (/usr/local/renovate/lib/util/exec/common.ts:119:11)
    at ChildProcess.emit (node:events:536:35)
    at ChildProcess.emit (node:domain:489:12)
    at Process.ChildProcess._handle.onexit (node:internal/child_process:293:12)

```

I can see it installing python and pip-tools in the logs, so possibly there's somewhere else in the config settings to add `uv` as a tool, but I don't know where. I also tried adding `[tool.uv]` section to `pyproject.toml` but run into the same poetry error as mentioned earlier in this thread.

---

_Comment by @mkniewallner on 2025-02-11 12:25_

> Now that the github app has updated to a newer version of renovate, I tried the version of replacing `pip-compile` with `uv pip compile` in the headers (with `--output-file=`), and end up with this error:
> 
> ```
> ExecError: Command failed: uv pip compile requirements.in --output-file=requirements.txt
> /bin/sh: 1: uv: not found
> 
>     at ChildProcess.<anonymous> (/usr/local/renovate/lib/util/exec/common.ts:119:11)
>     at ChildProcess.emit (node:events:536:35)
>     at ChildProcess.emit (node:domain:489:12)
>     at Process.ChildProcess._handle.onexit (node:internal/child_process:293:12)
> ```
> 
> I can see it installing python and pip-tools in the logs, so possibly there's somewhere else in the config settings to add `uv` as a tool, but I don't know where. I also tried adding `[tool.uv]` section to `pyproject.toml` but run into the same poetry error as mentioned earlier in this thread.

Renovate installs the tools it needs based on the package managers it detects. In Renovate, `uv pip compile` support was added on top of the existing `pip-compile` manager, but there is one missing piece it seems, as the required tools are constructed [here](https://github.com/renovatebot/renovate/blob/3caa1bbe8ddcd757dc3056256a95b936befcb439/lib/modules/manager/pip-compile/artifacts.ts#L140-L145) and [here](https://github.com/renovatebot/renovate/blob/d0916b18b32746470c2682893ca53354110ebc1a/lib/modules/manager/pip-compile/common.ts#L46-L77), which for now always assume that `pip-tools` is used, and doesn't handle uv.

As this is an issue in Renovate itself, feel free to open a [discussion](https://github.com/renovatebot/renovate/discussions) on Renovate repository to report the issue.

---

_Comment by @mkniewallner on 2025-02-11 12:45_

> As this is an issue in Renovate itself, feel free to open a [discussion](https://github.com/renovatebot/renovate/discussions) on Renovate repository to report the issue.

Actually, there is already a PR in review for handling this: https://github.com/renovatebot/renovate/pull/34029.

---

_Comment by @zanieb on 2025-04-21 19:40_

I wonder if this is safe to continue with now?

---

_Comment by @adrianeboyd on 2025-04-22 06:28_

We've been using it since the beginning of March now and as long as you're aware of some quirks, you can get it to work as intended within its limitations.

As already mentioned above as a potential blocker, there is still relatively limited support for parsing the options from the requirements header line, so for example the only supported variants are `--output-file=` and `--constraints=`, and no other alternatives, see:

https://github.com/renovatebot/renovate/blob/5fe91f64ec2fe5dd908caca6702206392e660773/lib/modules/manager/pip-compile/common.ts#L117-L159

https://github.com/renovatebot/renovate/blob/5fe91f64ec2fe5dd908caca6702206392e660773/lib/modules/manager/pip-compile/common.ts#L308-L314

None of this is documented very well and it can take some trial and error to get it set up, but dependabot with `pip-compile` hadn't been working for a larger project on our end anymore due to github runner timeouts and renovate with `uv pip compile` is a definite improvement.

---

_Comment by @zanieb on 2025-09-12 13:41_

If someone could help me finish these docs I'd appreciate it. I don't have time to focus on it.

---

_Label `help wanted` added by @zanieb on 2025-09-12 13:46_

---

_Comment by @zanieb on 2025-09-12 13:47_

(It seems worth covering those caveats in a note admonition or something)

---
