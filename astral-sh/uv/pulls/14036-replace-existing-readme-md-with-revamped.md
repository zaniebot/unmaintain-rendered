```yaml
number: 14036
title: "Replace existing `README.md` with revamped, feature‑rich version"
type: pull_request
state: closed
author: tripathiji1312
labels: []
assignees: []
base: main
head: main
created_at: 2025-06-14T05:30:09Z
updated_at: 2025-06-16T01:45:58Z
url: https://github.com/astral-sh/uv/pull/14036
synced_at: 2026-01-12T16:10:59Z
```

# Replace existing `README.md` with revamped, feature‑rich version

---

_@tripathiji1312_


**Summary**
This PR replaces the current minimalist `README.md` with a fully redesigned, badge‑driven landing page that:

* Introduces the `uv ⚡` branding and logo
* Surfaces install instructions (standalone scripts, package managers, self‑update)
* Highlights performance benchmarks with visual assets
* Showcases core features (Projects, Scripts, Tools, Python versions, pip interface) in rich, iconized tables and code samples
* Details platform support matrix
* Displays live project stats (stars, forks, issues, PRs, last commit)
* Consolidates a “Contributing” section with badge links
* Adds FAQ, Acknowledgements, License, and “Made by Astral” callouts
* Maintains consistency of links, shields.io badge styling, and accessibility

---

**Diff**

```diff
--- a/README.md
+++ b/README.md
@@ -1,358 +1,1 @@
-# uv
-
-[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)
-… (entire old content) …
+# uv ⚡
+
+<div align="center">
+
+![uv Logo](https://img.shields.io/badge/⚡-UV-orange?style=for-the-badge&labelColor=black&color=orange)
+
+**An extremely fast Python package and project manager, written in Rust.**
+
+… (full new content as provided) …
+
+*"The fastest way to manage Python projects and dependencies"*
```

---

**Checklist**

* [x] Old `README.md` content fully removed
* [x] New hero section, badges, and logo render correctly on GitHub
* [x] Code blocks and links verified
* [x] Performance image “dark”/“light” mode switching functions
* [x] “Contributing” badges link to `CONTRIBUTING.md`, “Good First Issues”, and “Help Wanted” pages
* [x] License section updated and dual‑license badges display
* [x] “Made by Astral” footer displays with appropriate links

---

*Enhances contributor onboarding and project discoverability.*


---

_Comment by @charliermarsh on 2025-06-16 01:45_

I think we're happy with the README as-is and not looking to do a big overhaul here.

---

_Closed by @charliermarsh on 2025-06-16 01:45_

---
