```yaml
number: 10355
title: "formatter: Two iterations required to converge on specific comment + slice case"
type: issue
state: closed
author: aneeshusa
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-03-12T06:56:01Z
updated_at: 2024-03-20T17:12:11Z
url: https://github.com/astral-sh/ruff/issues/10355
synced_at: 2026-01-12T15:54:50Z
```

# formatter: Two iterations required to converge on specific comment + slice case

---

_@aneeshusa_

Tested on ruff v0.3.2 and `main` (dacec7377c436fc85e380f7cc69a007824fa08bc). No ruff config.
Minimized from a file at $WORK; this cropped up during one of my final rebases to switch to ruff.
Not a blocker since the final formatting is stable.
black 24.2.0 accepts both the original and final style;
it rewrites the intermediate style to the final style.

Input:
```
repro(
    "some long string that takes up some space"
)[  # some long comment also taking up space
    0
]
```

1st run of ruff gives:
```
repro(
    "some long string that takes up some space"
)[0]  # some long comment also taking up space
```

2nd and beyond runs give:
```
repro("some long string that takes up some space")[
    0
]  # some long comment also taking up space
```


<details>
<summary>Script used</summary>

```
#!/usr/bin/env bash

# no errexit
set -o nounset
set -o pipefail

tmp_dir="$(mktemp -d)"
cleanup() {
    rm -rf "${tmp_dir}"
}
trap cleanup EXIT INT QUIT TERM

cat >"${tmp_dir}/repro.py" <<EOF
repro(
    "some long string that takes up some space"
)[  # some long comment also taking up space
    0
]
EOF

cd ~/code/ruff
git checkout "${1}"
ruff=(cargo run --quiet --package=ruff --)

echo -e '\nStarting hash of file'
(cd "${tmp_dir}" && sha256sum "./repro.py")
cat "${tmp_dir}/repro.py"

echo -e '\nFirst run'
"${ruff[@]}" format --isolated --target-version py310 --no-cache "${tmp_dir}/repro.py"
(cd "${tmp_dir}" && sha256sum "./repro.py")
cat "${tmp_dir}/repro.py"

echo -e '\nSecond run'
"${ruff[@]}" format --isolated --target-version py310 --no-cache "${tmp_dir}/repro.py"
(cd "${tmp_dir}" && sha256sum "./repro.py")
cat "${tmp_dir}/repro.py"

echo -e '\nThird run'
"${ruff[@]}" format --isolated --target-version py310 --no-cache "${tmp_dir}/repro.py"
(cd "${tmp_dir}" && sha256sum "./repro.py")
```
</details>

Didn't see any specific issues for this non-single-step-convergence, let me know if I missed one.

---

_Label `bug` added by @AlexWaygood on 2024-03-12 07:33_

---

_Label `formatter` added by @AlexWaygood on 2024-03-12 07:33_

---

_Comment by @charliermarsh on 2024-03-12 14:29_

Thanks!

---

_Assigned to @MichaReiser by @MichaReiser on 2024-03-18 11:20_

---

_Comment by @MichaReiser on 2024-03-20 16:53_

Thanks for the excellent write up. It made it super easy to reproduce and fix (including test cases)

---

_Closed by @MichaReiser on 2024-03-20 17:12_

---
