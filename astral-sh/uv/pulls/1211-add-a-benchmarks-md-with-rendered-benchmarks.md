```yaml
number: 1211
title: "Add a `BENCHMARKS.md` with rendered benchmarks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/bench
created_at: 2024-01-31T18:59:11Z
updated_at: 2024-01-31T20:19:36Z
url: https://github.com/astral-sh/uv/pull/1211
synced_at: 2026-01-12T16:04:30Z
```

# Add a `BENCHMARKS.md` with rendered benchmarks

---

_@charliermarsh_

As a precursor to the release, I want to include a structured document with detailed benchmarks.

Closes https://github.com/astral-sh/puffin/issues/1210.

---

_Label `documentation` added by @charliermarsh on 2024-01-31 18:59_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-31 19:12_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-31 19:12_

---

_Review requested from @konstin by @charliermarsh on 2024-01-31 19:12_

---

_Marked ready for review by @charliermarsh on 2024-01-31 19:12_

---

_@charliermarsh reviewed on 2024-01-31 19:13_

---

_Review comment by @charliermarsh on `Cargo.toml`:90 on 2024-01-31 19:13_

A lot of new deps but these are only in `puffin-dev`. Perhaps we should move `puffin-dev` dependencies out of here, actually, to avoid that problem.

---

_@charliermarsh reviewed on 2024-01-31 19:16_

---

_Review comment by @charliermarsh on `Cargo.toml`:90 on 2024-01-31 19:16_

Actually, I'm just gonna use these directly in `puffin-dev` and not treat them as workspace deps.

---

_Review comment by @BurntSushi on `BENCHMARKS.md`:11 on 2024-01-31 19:23_

I feel like the two sentences here are disconnected. If I think about it, I think you're saying that you picked Trio as an example that is not _just_ realistic, but also has a set of dependencies that demonstrates the differences between tools. That is, it isn't a benchmark that is tool agnostic.

---

_Review comment by @BurntSushi on `BENCHMARKS.md`:76 on 2024-01-31 19:25_

I think I would include the setup steps for running `python -m scripts.bench` here.

---

_Review comment by @BurntSushi on `BENCHMARKS.md`:76 on 2024-01-31 19:26_

Oh I see, you mention them below. Hmmm okay. I'd be tempted to at least include the `virtualenv` commands maybe?

---

_@BurntSushi approved on 2024-01-31 19:27_

Nice! Short and sweet and :fire: 

---

_@charliermarsh reviewed on 2024-01-31 19:58_

---

_Review comment by @charliermarsh on `BENCHMARKS.md`:76 on 2024-01-31 19:58_

Done!

---

_Merged by @charliermarsh on 2024-01-31 20:11_

---

_Closed by @charliermarsh on 2024-01-31 20:11_

---

_Branch deleted on 2024-01-31 20:11_

---

_@konstin reviewed on 2024-01-31 20:19_

---

_Review comment by @konstin on `BENCHMARKS.md`:3 on 2024-01-31 20:19_

This should name the used cpu

---
