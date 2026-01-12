```yaml
number: 1195
title: Implement SIM118 (key in dict) of flake8-simplify
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: flake8-simplify-sim118
created_at: 2022-12-11T12:53:40Z
updated_at: 2022-12-27T05:42:59Z
url: https://github.com/astral-sh/ruff/pull/1195
synced_at: 2026-01-12T05:36:31Z
```

# Implement SIM118 (key in dict) of flake8-simplify

---

_Pull request opened by @squiddy on 2022-12-11 12:53_

Refs: #998

---

_Comment by @charliermarsh on 2022-12-11 15:05_

Awesome, and thanks for handling all the new-plugin boilerplate. I'd love to automate some of that, I have a hacky Bash script but...

---

_Merged by @charliermarsh on 2022-12-11 15:05_

---

_Closed by @charliermarsh on 2022-12-11 15:05_

---

_Comment by @squiddy on 2022-12-11 15:07_

> I'd love to automate some of that, I have a hacky Bash script but...

Haha, I was thinking the same. Mind sharing your bash script? I'm in the process of cleaning up some existing code to align all of it better.

---

_Comment by @charliermarsh on 2022-12-11 15:09_

Hahah I mean, I'll share it, but it's embarrassing and incomplete! It really just creates a few files and directories, and then added the sequence of comments to `checks.rs` in a way that could be cleaned up by `cargo fmt`. But it's probably not even worth looking at sadly.

```sh
set -euxo pipefail

NAME=$1

mkdir -p src/$1
mkdir -p resources/test/fixtures/$1
touch src/$1/mod.rs

sed -i "" "s/mod flake8_print;/mod flake8_print; mod flake8_return;/g" src/lib.rs
sed -i "" "s|// flake8-print|// flake8-return\n// flake8-print|g" src/checks.rs
```

---

_Comment by @squiddy on 2022-12-11 15:22_

Thanks, I'll try to put something together.

---

_Branch deleted on 2022-12-27 05:42_

---
