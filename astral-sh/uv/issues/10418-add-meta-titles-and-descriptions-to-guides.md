```yaml
number: 10418
title: Add meta titles and descriptions to guides
type: issue
state: closed
author: charliermarsh
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-01-08T23:21:51Z
updated_at: 2025-01-09T22:39:02Z
url: https://github.com/astral-sh/uv/issues/10418
synced_at: 2026-01-10T04:27:58Z
```

# Add meta titles and descriptions to guides

---

_Issue opened by @charliermarsh on 2025-01-08 23:21_

We should probably do this everywhere, but the guides would be a good start.

---

_Label `documentation` added by @charliermarsh on 2025-01-08 23:21_

---

_Comment by @zanieb on 2025-01-09 00:20_

It'd be helpful if you gave some references to the guide the ideal content here.

---

_Comment by @charliermarsh on 2025-01-09 00:31_

There's an example here:

https://github.com/astral-sh/uv/pull/10411/files#diff-fa2c2b580b11a0f5315fce2c294eccfe1a8aeb681514e102c824708c79ab5fa0R1-R6


---

_Comment by @charliermarsh on 2025-01-09 00:33_

More examples:

```diff
diff --git a/docs/guides/integration/fastapi.md b/docs/guides/integration/fastapi.md
index 654b2ed14..6693f46d3 100644
--- a/docs/guides/integration/fastapi.md
+++ b/docs/guides/integration/fastapi.md
@@ -1,3 +1,10 @@
+---
+title: Using uv with FastAPI
+description:
+  A guide to using uv with FastAPI to manage Python dependencies, run applications, and deploy with
+  Docker.
+---
+
 # Using uv with FastAPI

 [FastAPI](https://github.com/fastapi/fastapi) is a modern, high-performance Python web framework.
diff --git a/docs/guides/integration/jupyter.md b/docs/guides/integration/jupyter.md
index 358a090d4..f1292a581 100644
--- a/docs/guides/integration/jupyter.md
+++ b/docs/guides/integration/jupyter.md
@@ -1,3 +1,10 @@
+---
+title: Using uv with Jupyter
+description:
+  A complete guide to using uv with Jupyter notebooks for interactive computing, data analysis, and
+  visualization, including kernel management and virtual environment integration.
+---
+
 # Using uv with Jupyter

 The [Jupyter](https://jupyter.org/) notebook is a popular tool for interactive computing, data
diff --git a/docs/guides/integration/pre-commit.md b/docs/guides/integration/pre-commit.md
index 6d963f8a4..e60c17854 100644
--- a/docs/guides/integration/pre-commit.md
+++ b/docs/guides/integration/pre-commit.md
@@ -1,3 +1,10 @@
+---
+title: Using uv with pre-commit
+description:
+  A guide to using uv with pre-commit to automatically update lock files, export requirements, and
+  compile requirements files.
+---
+
 # Using uv in pre-commit

 An official pre-commit hook is provided at
```

---

_Label `help wanted` added by @zanieb on 2025-01-09 01:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-09 01:19_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-01-09 01:21_

---

_Comment by @charliermarsh on 2025-01-09 01:21_

I PR'd those here: https://github.com/astral-sh/uv/pull/10421

---

_Comment by @FishAlchemist on 2025-01-09 06:31_

I'd like to know why you decided to add meta titles. Who is the target audience for these?

-----
LLMs should be pretty good at summarizing. I think having them create meta titles for each document and then just tweaking them a bit should be enough.

---

_Comment by @charliermarsh on 2025-01-09 14:16_

This is primarily for SEO. Adding a meta-description can help with ranking, and they appear in the search engine results pages (SERPs).

I did generate the above with prompting + some manual tweaks.

---

_Comment by @charliermarsh on 2025-01-09 21:38_

I'll wrap these up.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-09 21:38_

---

_Closed by @charliermarsh on 2025-01-09 22:39_

---

_Closed by @charliermarsh on 2025-01-09 22:39_

---
