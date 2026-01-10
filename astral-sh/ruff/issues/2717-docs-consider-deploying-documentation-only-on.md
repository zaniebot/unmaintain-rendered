```yaml
number: 2717
title: "docs: consider deploying documentation only on tagged releases"
type: issue
state: closed
author: bastimeyer
labels:
  - release
assignees: []
created_at: 2023-02-10T16:03:09Z
updated_at: 2023-02-11T03:14:51Z
url: https://github.com/astral-sh/ruff/issues/2717
synced_at: 2026-01-10T11:09:45Z
```

# docs: consider deploying documentation only on tagged releases

---

_Issue opened by @bastimeyer on 2023-02-10 16:03_

Hi, I've just migrated one of our projects from flake8 to ruff over the past few days and I've been slowly adding more of the available rules to the project's config since then.

While browsing the docs, I had a look at the `PYI` rules of the `flake8-pyi` re-implementation:
https://beta.ruff.rs/docs/rules/#flake8-pyi-pyi

Even though I was using the latest version of ruff, namely `0.0.244` released on 2023-02-08, adding `PYI` resulted in an error:
```
$ GIT_PAGER=cat git diff -U0
diff --git a/pyproject.toml b/pyproject.toml
index 1dbef5be..cc7e4b8b 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -82,0 +83,2 @@ select = [
+  # flake8-pyi
+  "PYI",

$ ruff --version
ruff 0.0.244

$ ruff .
error: TOML parse error at line 84, column 3
   |
84 |   "PYI",
   |   ^^^^^
Unknown rule selector `PYI`
```

That confused me, so I had a look at the issue tracker and commit log and found that the first one of these rules (#848) was just implemented this morning and it hasn't made it into a new release yet:
- 125615af1209de6c3be226b10b8f55a5eccd5aea
- 67e58a024aad929447d74898f12a48d1150d8fb8

From looking at the `docs.yml` CI config, new docs builds get deployed on each push to the `main` branch when the project's `README.md` gets updated:
https://github.com/charliermarsh/ruff/blame/cda2ff0b180d311a24cf545c0339ba8977437db3/.github/workflows/docs.yaml

I understand that this project is currently moving pretty fast and that there are lots of changes and new releases. However, deploying docs only on tagged releases would avoid confusions like this and would solve inconsistencies between what's documented and what's actually currently available when installing ruff from a tagged release (pre-built wheels, etc). Since you're having your docs organized in the project's readme and considering the development pace, splitting up docs builds between "stable" tags (quoted, because still pre 1.0.0) and the "unstable" main branch is currently probably not needed, and anyone who's looking for the latest stuff can have a look at the readme instead of the published docs.

Thanks for listening.

---

_Label `release` added by @charliermarsh on 2023-02-10 16:48_

---

_Comment by @charliermarsh on 2023-02-10 16:48_

This makes sense! The only downside is that the README and the docs will be out-of-sync, but... I guess that's ok, and unavoidable.

---

_Closed by @charliermarsh on 2023-02-11 03:14_

---
