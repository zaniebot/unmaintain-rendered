```yaml
number: 9610
title: "Build backend: Add `--list` option"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-list-cli
created_at: 2024-12-03T16:32:26Z
updated_at: 2024-12-04T12:50:15Z
url: https://github.com/astral-sh/uv/pull/9610
synced_at: 2026-01-12T16:08:53Z
```

# Build backend: Add `--list` option

---

_@konstin_

Add the `uv build --list`, a "subcommand" to list the files that would be included when building a distribution. It does not build the distribution, except when a source dist is required for source dist -> wheel. This is an important debugging tool for the include and exclude options: Did i actually include the files I wanted, or am i shipping a broken distribution? Are there any temporary files I still need to exclude?

Cargo offers this as `cargo package --list`.

`--list` is preview-exclusive, since it requires the fast path, which I also put into preview.

Examples:

![image](https://github.com/user-attachments/assets/55e3f169-3051-4217-987d-0cb01ae5050e)

![image](https://github.com/user-attachments/assets/1da75245-358d-4bee-9199-f720089f0a70)

![image](https://github.com/user-attachments/assets/4d97a893-552e-43a1-9c22-78fc67f1e9f5)

I'll fix the error handling in a follow-up.

Tagging as enhancement because it changes the stable output slightly (two lines instead of one).

CC @charliermarsh for uv-wide consistency in the stdout/stderr handling.

---

_Label `preview` added by @konstin on 2024-12-03 16:32_

---

_Review requested from @BurntSushi by @konstin on 2024-12-03 16:32_

---

_Review requested from @charliermarsh by @konstin on 2024-12-03 16:32_

---

_Label `preview` removed by @konstin on 2024-12-03 16:45_

---

_Label `enhancement` added by @konstin on 2024-12-03 16:45_

---

_Label `preview` added by @konstin on 2024-12-03 16:45_

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:2181 on 2024-12-03 17:09_

Should you mention in the docs here that this conflicts with `--force-pep517`? I might even spare a sentence or two saying why that conflict exists. (I'm guessing that in the fast path, it's easier to have a side channel asking the build backend to emit extra output? I haven't read the rest of the PR yet.)

---

_@BurntSushi approved on 2024-12-03 17:11_

I think this makes sense to me. Although I am not sure I have enough knowledge to understand why `--list` requires the fast path. Will there be other options that also require the fast path?

(It occurs to me that "fast path" may be a misnomer, or at least, selling itself short. Because it isn't _just_ a fast path but a required mode in order to access additional functionality.)

---

_@konstin reviewed on 2024-12-03 19:02_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2181 on 2024-12-03 19:02_

I added an explanation about how this doesn't go through PEP 517.

---

_Comment by @konstin on 2024-12-03 20:10_

> Although I am not sure I have enough knowledge to understand why --list requires the fast path. Will there be other options that also require the fast path?

Potentially - We may grow more uv build backend only features in the future that don't work through the limited set of PEP 517 hooks. "direct build" is a better term now than fast path. We need to explain to users that uv build can be both the PEP 517 generic source dist and wheel builder (`python -m build`, `pip wheel`, etc.) and the CLI command to the uv build system (`poetry build`, `hatch build`, `maturin build`). The latter has more options than the former (`maturin build` is a good example), while the former is the "official" way, there is some agreement that `python -m build` is the "official" way to build. By default, uv will be the latter for the uv build backend, but we allow switching to the former with `--force-pep517`, since this is what your package's users will experience.

---

_Comment by @charliermarsh on 2024-12-03 20:24_

I think it's correct for the output to go to `stderr` in general, and then `stdout` with `--list`.

---

_Merged by @konstin on 2024-12-04 08:52_

---

_Closed by @konstin on 2024-12-04 08:52_

---

_Branch deleted on 2024-12-04 08:52_

---

_Comment by @BurntSushi on 2024-12-04 12:50_

Yeah I like "direct build" a lot better. :-) Otherwise makes sense, thank you for explaining!

---
