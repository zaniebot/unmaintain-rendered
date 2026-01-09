---
number: 10183
title: setup-uv is broken for uv version 0.5.12
type: issue
state: closed
author: ekzhu
labels: []
assignees: []
created_at: 2024-12-26T22:56:16Z
updated_at: 2024-12-27T16:39:48Z
url: https://github.com/astral-sh/uv/issues/10183
synced_at: 2026-01-07T13:12:18-06:00
---

# setup-uv is broken for uv version 0.5.12

---

_Issue opened by @ekzhu on 2024-12-26 22:56_

See the failing CI build: https://github.com/microsoft/autogen/actions/runs/12508932257/job/34897740078

It works locally with uv version 0.5.12. 

Previous version 0.5.11 also works on setup-uv@v5 in CI. 

---

_Referenced in [microsoft/autogen#4820](../../microsoft/autogen/pulls/4820.md) on 2024-12-26 22:56_

---

_Comment by @charliermarsh on 2024-12-26 22:58_

Thanks, I'll take a look.

---

_Comment by @charliermarsh on 2024-12-26 23:13_

Ah ok, this is a result of a change I made in this release that modifies how we write metadata for "projects that don't have a `[project]` table", and it looks like you're using that in `autogen/python`. We've considered those legacy for a while, so I thought it was fine to ship it in a patch release. I'm sorry for the disruption. You can run `uv lock` to update the lockfile. It should attempt to preserve as many versions as possible.

---

_Comment by @ekzhu on 2024-12-26 23:32_

Thanks for the quick response. I ran `uv lock` but it did not fix it. 

We would like to update from the legacy usage. So we should include `[project]` in our `autogen/python/project.toml` file correct? 

---

_Comment by @charliermarsh on 2024-12-26 23:38_

Yeah, this would be the most up-to-date configuration:

```diff
diff --git a/python/pyproject.toml b/python/pyproject.toml
index 5829a09a..af13101d 100644
--- a/python/pyproject.toml
+++ b/python/pyproject.toml
@@ -1,8 +1,10 @@
-[tool.uv.workspace]
-members = ["packages/*"]
+[project]
+name = "autogen"
+version = "0.0.1"
+dependencies = []

-[tool.uv]
-dev-dependencies = [
+[dependency-groups]
+dev = [
     "pyright==1.1.389",
     "mypy==1.13.0",
     "ruff==0.4.8",
@@ -22,6 +24,9 @@ dev-dependencies = [
     "tomli",
 ]

+[tool.uv.workspace]
+members = ["packages/*"]
+
 [tool.uv.sources]
 autogen-core = { workspace = true }
 autogen-ext = { workspace = true }
```

`[dependency-groups]` was recently standardized, so you can now use that in lieu of `[tool.uv.dev-dependencies]`, but it's not a required change -- just optional.

---

_Comment by @charliermarsh on 2024-12-26 23:38_

(Running `uvx uv@0.5.12 lock` followed by `uvx uv@0.5.12 lock --locked` does succeed for me, though.)

---

_Comment by @jackgerrits on 2024-12-26 23:55_

Thanks so much for explaining how we can fix our setup. Definitely didn't intend to be out of date and are very happy to move forward

---

_Comment by @charliermarsh on 2024-12-27 13:41_

Oh, no worries! Happy to help.

---

_Closed by @charliermarsh on 2024-12-27 13:41_

---

_Referenced in [microsoft/autogen#4830](../../microsoft/autogen/pulls/4830.md) on 2024-12-27 15:22_

---

_Comment by @jackgerrits on 2024-12-27 15:35_

@charliermarsh After upgrading our workspace `pyproject.toml`, it seems like a `uv sync` in the root workspace directory is no longer installing all workspace packages into the created virtual environment. Is this an expected change?

~~Is there a way to cause the `uv sync` to install all workspace packages?~~ Found it: `--all-packages`

I confirmed before and after changing the workspace definition per [this](https://github.com/astral-sh/uv/issues/10183#issuecomment-2563171567) comment using both uv 5.11 and 5.13.

Is it because the workspace is now a "project", and it is only installing that "project" and no longer transitively all projects in the workspace?

---

_Comment by @jackgerrits on 2024-12-27 15:38_

It seems I now need to pass `--all-packages` to get all packages installed. We're we relying on some implicit weirdness before to get all the packages installed?

This definitely feels reasonable, but I know some of our users are going to get tripped up by it

---

_Comment by @charliermarsh on 2024-12-27 16:01_

Yeah that's right -- if you use a `pyproject.toml` without a `[project]` table, then the default behavior is to install all members. But once it's a package, the default is to install that package's dependencies.

So, you have a few options:

1. Add all the packages in `packages/` as dependencies.

```diff
diff --git a/python/pyproject.toml b/python/pyproject.toml
index 5829a09a..6e774066 100644
--- a/python/pyproject.toml
+++ b/python/pyproject.toml
@@ -1,3 +1,17 @@
+[project]
+name = "autogen"
+version = "0.0.1"
+dependnecies = [
+    "agbench",
+    "autogen-agentchat",
+    "autogen-core",
+    "autogen-ext",
+    "autogen-magentic-one",
+    "autogen-studio",
+    "autogen-test-utils",
+    "component-schema-gen",
+]
+
 [tool.uv.workspace]
 members = ["packages/*"]
 
@@ -23,10 +37,14 @@ dev-dependencies = [
 ]
 
 [tool.uv.sources]
+agbench = { workspace = true }
+autogen-agentchat = { workspace = true }
 autogen-core = { workspace = true }
 autogen-ext = { workspace = true }
-autogen-agentchat = { workspace = true }
+autogen-magentic-one = { workspace = true }
+autogen-studio = { workspace = true }
 autogen-test-utils = { workspace = true }
+component-schema-gen = { workspace = true }
 
 [tool.ruff]
 line-length = 120
```

2. Pass `--all-packages`.

3. Skip adding `[project]` for now. We'll probably make these non-`[project]` projects more first-class in the future, since PEP 735 explicitly added support for them. So, it's ok if you want to leave it as-is. It's mostly that we've historically discouraged them, and they have some odd behaviors right now that we want to fix up over time.

---

_Comment by @jackgerrits on 2024-12-27 16:11_

Thanks so much for the explanation all makes complete sense to me. I'd prefer to be working with what's best supported by uv and since this workspace package is solely for dev work convenience (which is a big deal. uv's workspace support is a massive feature IMO) its not a big deal at all to change if we need to. 

Option 1 seems the best for our use case at the moment. Thanks again for the support here!

---

_Comment by @charliermarsh on 2024-12-27 16:17_

No worries, I'm always happy to help especially when people are so friendly. Ask any time.

---

_Comment by @jackgerrits on 2024-12-27 16:27_

We really appreciate your super fast and helpful responses! Seriously love the work you and the team do.

---

_Comment by @jackgerrits on 2024-12-27 16:36_

One more question. I have a dev dependency group in both the root workspace package and several of the packages in the workspace. It seems if I use `--all-packages` then the dev dependency group is installed for each package in workspace. But, 
 if I go the route of [`1.`](https://github.com/astral-sh/uv/issues/10183#issuecomment-2563831350) above, then the dev dependency group of transitive packages is not installed. Understandable, since now they are simple dependencies and you wouldn't want dev deps installed in this instance normally. However, is there a way to define the root workspace package to also cause all workspace package dev dependencies to be installed too? 

If not, I think I'll just collapse all dev dependencies into the root package rather than splitting them per package.

---
