---
number: 7454
title: "✨ Allow patching third-party packages via new command `uv patch <pkg_name>`"
type: issue
state: open
author: jd-solanki
labels:
  - question
assignees: []
created_at: 2024-09-17T10:47:11Z
updated_at: 2025-09-19T07:45:23Z
url: https://github.com/astral-sh/uv/issues/7454
synced_at: 2026-01-07T13:12:17-06:00
---

# ✨ Allow patching third-party packages via new command `uv patch <pkg_name>`

---

_Issue opened by @jd-solanki on 2024-09-17 10:47_

Hi 👋🏻 

When working on node based project there's [pnpm](https://pnpm.io/) package manager that allows patching third-party packages installed in node_modules if we want to fix something for all the contributors of the project.

pnpm allows this via [patch](https://pnpm.io/cli/patch)

I came across similar situation where I want to patch the package "fastapi-users" until I get response on my PR/discussion. I noticed UV don't have such feature.

Thanks for the UV it's superfast 🚀 

---

_Comment by @charliermarsh on 2024-09-17 13:01_

You _can_ patch third-party dependencies via `tool.uv.sources`. Like, you could clone anyio locally, and then add:

```toml
[tool.uv.sources]
anyio = { path = "/path/to/anyio" }
```

Assuming you make `anyio` a direct dependency. Is this different than the patch workflow?

---

_Label `question` added by @charliermarsh on 2024-09-17 13:01_

---

_Comment by @jd-solanki on 2024-09-18 05:39_

Hi Charlie,

Glad to see you. Unfortunetly, this is not patch workflow. In our case with above solution everyone has to either go through steps you mentioned to start working on project or else we have to commit third-package along with source code with is really bad.

Assume I want to do minor fix in some unmaintained library but we don't prefer forking and making a seperate package just for that minor change. So, in pnpm we run `pnpm patch <that-lib>` and it'll copy that dep somewhere in temp location where we can edit the dependency however we want for example adding print in python or console.log in node. Once we are done with editing we run `pnpm patch-commit <temp-path-where-pnpm-gen-clone>` this generates patch (git diff) in our source code as you can see [here](https://youtu.be/0GjLqRGRbcY?t=118). Now, whenever someone clone our repo and run `pnpm install` these patch inside `patches` directory get applied to node_modules automatically for everyone without any additional steps.

Checkout this [pnpm video](https://www.youtube.com/watch?v=0GjLqRGRbcY)

---

Usecases:
- Making minor fixes in unmaintained libs
- Continue working on project as a team until lib author merges the PR or addes feature
- Tweak third party package without extra hassle of maintaining it

---

_Referenced in [astral-sh/uv#8366](../../astral-sh/uv/pulls/8366.md) on 2024-10-21 22:13_

---

_Comment by @aretrace on 2024-12-17 15:23_

✨ YES YES YES ✨
`uv patch <pkg_name>` will be insurmountably useful!


---

_Comment by @JonathanPlasse on 2025-01-03 13:26_

That is exactly what I am looking for to allow using [fake-bpy-module](https://github.com/nutti/fake-bpy-module) with custom properties for Blender add-on development.
The `patch` workflow is especially need here as the `fake-bpy-module` package is auto-generated and would need every person using it with custom properties to maintain a separate git repository mirroring it with their additional changes for every add-on project.
With the `patch` workflow, the add-on custom properties would not need to be in to separate repo.

---

_Referenced in [astral-sh/uv#9258](../../astral-sh/uv/issues/9258.md) on 2025-01-13 23:46_

---

_Comment by @Avasam on 2025-03-07 04:14_

Here's some prior art in the npm ecosystem: https://www.npmjs.com/package/patch-package

that package has saved my ass too many times. Also allowed us to enforce better type safety by patching unsafe types directly in the source !

---

_Comment by @jd-solanki on 2025-03-07 09:55_

Hi @charliermarsh We should update the label now from question to something else

---

_Comment by @simplegr33n on 2025-03-12 21:18_

would really appreciate ability to patch 3rd party packages as well, ideally via configuring in `pyproject.toml`

---

_Comment by @calebpitan on 2025-04-29 20:19_

I feared there was no way to patch packages with uv becuase I can say for sure I've seen every corner of the documentation and I don't remember ever seeing any such thing as "patch package"; now I run to the internet to find out if I can patch a package and my fear wins.

---

_Comment by @roniemartinez on 2025-05-23 21:32_

Had worked on a TS project recently where a 3rd-party lib was inactive and no releases have been done to fix multiple bugs. Luckily, I was able to use `yarn patch/patch-commit` which saved us from using an old buggy version of the library.

The workflow is:
1. Run `yarn patch <package-name>` which copies the library to a temp dir.
2. Edit the package with an editor (like vim).
3. Run `yarn patch-commit ...` which then generates a diff, comparing the original package and the package you edited.
4. Every time you run install, the patches are applied "post-install".

It would really be awesome if this is supported in `uv` as we are using our forks of inactive libraries (it's messy).

---

_Comment by @MattTheCuber on 2025-07-11 20:45_

This would be super useful for working with security vulnerabilities (#9189 reference).

For example, the current torch version has a vulnerability: https://github.com/advisories/GHSA-887c-mr87-cxwp. It was applied in this commit: https://github.com/pytorch/pytorch/commit/46fc5d8e360127361211cb237d5f9eef0223e567. To set this up, we would need to clone the repository, cherry-pick the commit, build wheels, then install. Having a way to configure this in our `pyproject.toml` would be amazing.

---

_Comment by @oliverzy on 2025-08-23 13:00_

This is only thing I miss. Please consider it

---

_Comment by @zhuoqun-chen on 2025-09-19 07:14_

would it be possible to install a package with its patch in the form of the same repo's unmerged pull request? It could be super convenient to install super old packages and make it compatible with modern pytorch bundles.

---

_Comment by @oliverzy on 2025-09-19 07:45_

https://github.com/nomyfan/patch-package-py

We developed this package to solve this problem, please help youself and let us know whether it helps

---
