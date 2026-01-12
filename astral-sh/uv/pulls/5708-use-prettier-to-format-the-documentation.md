```yaml
number: 5708
title: Use prettier to format the documentation
type: pull_request
state: merged
author: konstin
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: konsti/format-docs
created_at: 2024-08-01T19:28:56Z
updated_at: 2024-08-02T13:58:34Z
url: https://github.com/astral-sh/uv/pull/5708
synced_at: 2026-01-12T16:06:58Z
```

# Use prettier to format the documentation

---

_@konstin_

To enforce the 100 character line limit in markdown files introduced in https://github.com/astral-sh/uv/pull/5635, and to automate the formatting of markdown files, i've added prettier and formatted our markdown files with it.

I've excluded the changelog and the generated references documentation from this for having too many changes, but we can also include them.

I'm not particular on which style we use. My main motivations are (major) not having to reflow markdown files myself anymore and (minor) consistence between all markdown files. I've chosen prettier for similar reason as we chose black, it's a single good style that's automated and shared in the community. I do prefer prettier's style of not breaking inside of a link name though.

This PR is in two parts, the first adds prettier to CI and documents using it, while the second actually formats the docs. When merge conflicts arise, we can drop the last commit and regenerate it with `npx prettier --prose-wrap always --write BENCHMARKS.md CONTRIBUTING.md README.md STYLE.md docs/*.md docs/concepts/**/*.md docs/guides/**/*.md docs/pip/**/*.md`.

---

_Label `documentation` added by @konstin on 2024-08-01 19:28_

---

_Review requested from @BurntSushi by @konstin on 2024-08-01 19:28_

---

_Review requested from @charliermarsh by @konstin on 2024-08-01 19:28_

---

_Review requested from @zanieb by @konstin on 2024-08-01 19:28_

---

_@zanieb reviewed on 2024-08-01 19:31_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:91 on 2024-08-01 19:31_

Why not `docs/**/*.md`? Because we need to exclude generated files? This seems brittle.

---

_Label `internal` added by @zanieb on 2024-08-01 19:32_

---

_@konstin reviewed on 2024-08-01 19:37_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:91 on 2024-08-01 19:37_

Yeah, i went with the conservative option here. I have touched neither the reference docs nor the changelog, so i'll follow your lead for them.

---

_Comment by @konstin on 2024-08-01 19:41_

For the jetbrains IDE users, you can configure prettier to run for `*.md` on save instead of the default formatter. Don't forget to make sure that node is also configured.

![image](https://github.com/user-attachments/assets/98cfb06c-9c96-492e-bd60-481670066b4c)


---

_@zanieb reviewed on 2024-08-01 19:56_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:91 on 2024-08-01 19:56_

Does it support exclusions? Can we encode this in a repo-level config file?

---

_@konstin reviewed on 2024-08-01 20:29_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:91 on 2024-08-01 20:29_

I added exclusion comments. If we have more setting prettier we can switch to one of their configuration files.

I'd like to format the generated files too, but the diff is already large enough.

---

_@BurntSushi approved on 2024-08-02 11:34_

SGTM

---

_Merged by @zanieb on 2024-08-02 13:58_

---

_Closed by @zanieb on 2024-08-02 13:58_

---

_Branch deleted on 2024-08-02 13:58_

---
