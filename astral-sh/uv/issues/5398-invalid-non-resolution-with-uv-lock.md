```yaml
number: 5398
title: "Invalid non-resolution with `uv lock`"
type: issue
state: closed
author: JP-Ellis
labels:
  - question
assignees: []
created_at: 2024-07-24T06:03:09Z
updated_at: 2024-08-09T08:04:07Z
url: https://github.com/astral-sh/uv/issues/5398
synced_at: 2026-01-10T04:53:49Z
```

# Invalid non-resolution with `uv lock`

---

_Issue opened by @JP-Ellis on 2024-07-24 06:03_

## Summary

With the most recent `0.2.28` release of uv, I am no longer able to lock the root package in my workspace, resulting in the error:

```text
  × No solution found when resolving dependencies:
  ╰─▶ Because only mypkg[openai]==0.1.1.dev209+g1c110b4.d20240724 is available and the requested Python version (>=3.8) does not satisfy Python>=3.12.0,<3.13, we can conclude that all versions
      of mypkg[openai] are incompatible.
      And because you require mypkg[openai], we can conclude that the requirements are unsatisfiable.
```

It seems to suggest that `Python >= 3.8` is not compatible with `Python ~3.12.0`, which I believe is wrong as stated (though the converse would be correct).

## Logs

The error appears with various commands that require dependency resolution. 
trying `uv -v lock` results in:

```text
DEBUG uv 0.2.28
warning: `uv lock` is experimental and may change without warning
DEBUG Found workspace root: `/Users/joshua.ellis/src/myproduct/mypkg`
DEBUG Adding current workspace member: /Users/joshua.ellis/src/myproduct/mypkg
DEBUG Adding discovered workspace member: /Users/joshua.ellis/src/myproduct/mypkg/mypkg-api
DEBUG Adding discovered workspace member: /Users/joshua.ellis/src/myproduct/mypkg/mypkg-cli
DEBUG Interpreter meets the requested Python: `Python >=3.8`
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
DEBUG Acquired lock for `/Users/joshua.ellis/Library/Caches/uv/built-wheels-v3/editable/4b5f6ac25df4b874`
DEBUG Acquired lock for `/Users/joshua.ellis/Library/Caches/uv/built-wheels-v3/editable/0d4c51059253629e`
DEBUG Acquired lock for `/Users/joshua.ellis/Library/Caches/uv/built-wheels-v3/editable/8533a92294f62217`
DEBUG Preparing metadata for: mypkg @ file:///Users/joshua.ellis/src/myproduct/mypkg
DEBUG Preparing metadata for: mypkg-api @ file:///Users/joshua.ellis/src/myproduct/mypkg/mypkg-api
DEBUG Preparing metadata for: mypkg-cli @ file:///Users/joshua.ellis/src/myproduct/mypkg/mypkg-cli
## installing a bunch of stuff to prepare metadata
## redacted as I don't think it is that relevant
## but happy to update in case it is useful
DEBUG Prepared metadata for: mypkg @ file:///Users/joshua.ellis/src/myproduct/mypkg
DEBUG Prepared metadata for: mypkg-cli @ file:///Users/joshua.ellis/src/myproduct/mypkg/mypkg-cli
DEBUG Prepared metadata for: mypkg-api @ file:///Users/joshua.ellis/src/myproduct/mypkg/mypkg-api
DEBUG Found workspace root: `/Users/joshua.ellis/src/myproduct/mypkg`
DEBUG Adding current workspace member: /Users/joshua.ellis/src/myproduct/mypkg
DEBUG Adding discovered workspace member: /Users/joshua.ellis/src/myproduct/mypkg/mypkg-api
DEBUG Found workspace root: `/Users/joshua.ellis/src/myproduct/mypkg`
DEBUG Adding root workspace member: /Users/joshua.ellis/src/myproduct/mypkg
DEBUG Adding current workspace member: /Users/joshua.ellis/src/myproduct/mypkg/mypkg-cli
DEBUG Found workspace root: `/Users/joshua.ellis/src/myproduct/mypkg`
DEBUG Adding root workspace member: /Users/joshua.ellis/src/myproduct/mypkg
DEBUG Adding current workspace member: /Users/joshua.ellis/src/myproduct/mypkg/mypkg-api
DEBUG Adding discovered workspace member: /Users/joshua.ellis/src/myproduct/mypkg/mypkg-cli
warning: `uv.sources` is experimental and may change without warning
warning: Missing version constraint (e.g., a lower bound) for `boto3`
warning: Missing version constraint (e.g., a lower bound) for `boto3-stubs`
warning: Missing version constraint (e.g., a lower bound) for `coverage`
warning: Missing version constraint (e.g., a lower bound) for `ipykernel`
warning: Missing version constraint (e.g., a lower bound) for `jupyter`
warning: Missing version constraint (e.g., a lower bound) for `mkdocs`
warning: Missing version constraint (e.g., a lower bound) for `mkdocs-gen-files`
warning: Missing version constraint (e.g., a lower bound) for `mkdocs-literate-nav`
warning: Missing version constraint (e.g., a lower bound) for `mkdocs-material`
warning: Missing version constraint (e.g., a lower bound) for `mkdocs-section-index`
warning: Missing version constraint (e.g., a lower bound) for `mkdocstrings`
warning: Missing version constraint (e.g., a lower bound) for `pandas`
warning: Missing version constraint (e.g., a lower bound) for `pathspec`
warning: Missing version constraint (e.g., a lower bound) for `pygments`
warning: Missing version constraint (e.g., a lower bound) for `pytest`
warning: Missing version constraint (e.g., a lower bound) for `pytest-cov`
warning: Missing version constraint (e.g., a lower bound) for `rich`
warning: Missing version constraint (e.g., a lower bound) for `types-pygments`
warning: Missing version constraint (e.g., a lower bound) for `types-pyyaml`
DEBUG Adding discovered workspace member: /Users/joshua.ellis/src/myproduct/mypkg/mypkg-api
warning: Missing version constraint (e.g., a lower bound) for `packaging`
warning: Missing version constraint (e.g., a lower bound) for `types-toml`
DEBUG Adding discovered workspace member: /Users/joshua.ellis/src/myproduct/mypkg/mypkg-cli
warning: Missing version constraint (e.g., a lower bound) for `aws-lambda-typing`
warning: Missing version constraint (e.g., a lower bound) for `botocore-stubs`
warning: Missing version constraint (e.g., a lower bound) for `requests`
warning: Missing version constraint (e.g., a lower bound) for `types-cryptography`
warning: Missing version constraint (e.g., a lower bound) for `types-requests`
warning: Missing version constraint (e.g., a lower bound) for `uvicorn`
DEBUG Solving with installed Python version: 3.12.4
DEBUG Solving with target Python version: >=3.8
DEBUG Adding direct dependency: mypkg*
DEBUG Adding direct dependency: mypkg[anthropic]*
DEBUG Adding direct dependency: mypkg[devel]*
DEBUG Adding direct dependency: mypkg[devel-docs]*
DEBUG Adding direct dependency: mypkg[devel-notebook]*
DEBUG Adding direct dependency: mypkg[devel-test]*
DEBUG Adding direct dependency: mypkg[devel-types]*
DEBUG Adding direct dependency: mypkg[openai]*
DEBUG Adding direct dependency: mypkg-api*
DEBUG Adding direct dependency: mypkg-api[devel]*
DEBUG Adding direct dependency: mypkg-api[devel-test]*
DEBUG Adding direct dependency: mypkg-api[devel-types]*
DEBUG Adding direct dependency: mypkg-cli*
DEBUG Adding direct dependency: mypkg-cli[devel]*
DEBUG Adding direct dependency: mypkg-cli[devel-test]*
DEBUG Adding direct dependency: mypkg-cli[devel-types]*
DEBUG Searching for a compatible version of mypkg @ file:///Users/joshua.ellis/src/myproduct/mypkg (*)
DEBUG No compatible version found for: Python
DEBUG Searching for a compatible version of mypkg @ file:///Users/joshua.ellis/src/myproduct/mypkg (<0.1.1.dev209+g1c110b4.d20240724 | >0.1.1.dev209+g1c110b4.d20240724)
DEBUG No compatible version found for: mypkg[openai]
  × No solution found when resolving dependencies:
  ╰─▶ Because only mypkg[openai]==0.1.1.dev209+g1c110b4.d20240724 is available and the requested Python version (>=3.8) does not satisfy Python>=3.12.0,<3.13, we can conclude that all versions
      of mypkg[openai] are incompatible.
      And because you require mypkg[openai], we can conclude that the requirements are unsatisfiable.
```

The `mypkg-cli` component has the broadest Python compatibility, while the `mypkg` and `mypkg-api` components are pinned to use `3.12.X`.

It would appear that in the resolution, instead of narrow down the Python compatibility range, it is throwing an error for `mypkg` due to its narrow Python version requirements.

---

_Comment by @charliermarsh on 2024-07-24 13:30_

If your project declares a Python compatibility range of `Python >= 3.8`, then all of your dependencies need to be at least as compatible as that range. If you have a package that only supports Python 3.12 or later, then there are some Python versions for which we wouldn't be able to construct a successful resolution. Does that make sense?

---

_Label `question` added by @charliermarsh on 2024-07-24 19:53_

---

_Comment by @JP-Ellis on 2024-07-24 21:27_

That makes sense, but my situation is in fact the other way around; my project restricts Python >=3.12, but depends on a project requiring Python >= 3.8.

I also suspect it is an issue with `uv lock` as I am able to `uv pip install .`

---

_Comment by @charliermarsh on 2024-07-24 21:28_

The logs suggest you have a project in your workspace with `Python >= 3.8` though -- is that wrong?

---

_Comment by @JP-Ellis on 2024-07-24 21:39_

Indeed there is, but the dependency is the wrong way around. 

`mypkg` is the root package and is in the workspace root, and requires Python 3.12. It has a development dependency of `mypkg-cli` (kept under the `devel` feature group) which requires a Python >= 3.8. The `mypkg-cli` dependency is a standalone package with no dependence on anything else within the workspace.

Hope that makes sense? If it helps, I can also try and create a MWE.

---

_Comment by @charliermarsh on 2024-07-24 21:40_

The thing is, we're trying to lock the entire workspace, not just the root package. You could make `mypkg-cli` just a path dependency rather than part of the workspace (so it'd have its own lockfile if you wanted to run it), but for the workspace, we have to come up with a single lockfile that works for everything in the environment.

---

_Comment by @JP-Ellis on 2024-07-24 21:44_

That makes sense! 

My follow up question then: does running `uv lock` within one of the subprojects lock the whole workspace again? Or only the subproject? This behaviour should probably be made clear, as it wasn't to me. (I understand `uv lock` is still in preview, so hopefully that's useful feedback!)

---

_Comment by @zanieb on 2024-07-24 22:05_

(This would be good to clarify at https://github.com/astral-sh/uv/blob/main/docs/projects.md#lock-file if not present)

---

_Assigned to @konstin by @konstin on 2024-07-29 09:53_

---

_Unassigned @konstin by @konstin on 2024-08-07 12:26_

---

_Comment by @konstin on 2024-08-07 12:28_

We improved the resolver docs including the way more details:
* https://github.com/astral-sh/uv/blob/main/docs/concepts/resolution.md
* https://github.com/astral-sh/uv/blob/main/docs/reference/resolver-internals.md

Do those sufficiently explain the behavior you're seeing?

---

_Comment by @JP-Ellis on 2024-08-07 23:50_

Thanks for the heads up! I have gone of the two docs, I think the only thing that might be missing (which is what caught me off guard) is that `uv lock` works at the workspace level, unlike for example `uv pip compile`.

If we suppose a workspace as follows:

```text
mypkg
├── mypkg-api          <== API wrapper for core, pinned to specific Py version
│  ├── pyproject.toml
│  └── README.md
├── mypkg-cli          <== Targets all OS+Python combinations
│  ├── pyproject.toml
│  └── README.md
├── src
│  └── mypkg           <== Core code, library
├── pyproject.toml
└── README.md
```

I ran into the issue with `uv lock` with the intent of pinning all dependencies of `mypkg-api` to ensure reproducible builds. However, the command failed due to incompatible Python requirements between `mypkg-api` and `mypkg-cli`, even though these two packages have orthogonal purposes which would only come together on the dev's machine when (for example) testing the API locally.

I'm not sure whether it belongs in these docs or elsewhere, but I think there should be clarity as to how workspaces affects the various uv subcommands.

---

_Comment by @konstin on 2024-08-08 08:30_

Good note, we have workspace docs too, even though the answer currently is that we don't really support workspaces with different `requires-python` per packages: https://github.com/astral-sh/uv/blob/main/docs/concepts/workspaces.md

---

_Comment by @JP-Ellis on 2024-08-08 11:30_

The workspace docs do make it this behaviour much clearer! And even explain how `uv lock` can be narrowed down to a single package within a workspace.

Thanks 👍 

I think this issue can be closed, though I'll let you all make the final determination.

---

_Comment by @konstin on 2024-08-09 08:04_

We have workspaces tracked at https://github.com/astral-sh/uv/issues/5594, will close.

---

_Closed by @konstin on 2024-08-09 08:04_

---
