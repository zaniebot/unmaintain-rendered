```yaml
number: 6909
title: Update playground README.md
type: pull_request
state: merged
author: cnpryer
labels:
  - documentation
  - playground
assignees: []
merged: true
base: main
head: playground
created_at: 2023-08-27T01:04:40Z
updated_at: 2023-08-27T11:10:36Z
url: https://github.com/astral-sh/ruff/pull/6909
synced_at: 2026-01-12T15:55:22Z
```

# Update playground README.md

---

_@cnpryer_

This PR makes a few small changes to the playground's README.md.

1. I needed to install [`wasm-pack`](https://rustwasm.github.io/wasm-pack/) -- which wasn't mentioned in the README.md.
2. The same command is mentioned for both release and debug builds (`npm run build:wasm`). Maybe I'm misunderstanding this, but I changed the debug command to `npm run dev:wasm`.
3. Some wording to tie the changes together.

---

_Comment by @codspeed-hq[bot] on 2023-08-27 01:12_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cnpryer:playground)

### Merging #6909 will **not alter performance**

<sub>Comparing <code>cnpryer:playground</code> (ed1e586) with <code>main</code> (eae59cf)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_@MichaReiser approved on 2023-08-27 08:54_

Thanks

---

_Merged by @MichaReiser on 2023-08-27 08:54_

---

_Closed by @MichaReiser on 2023-08-27 08:54_

---

_Label `documentation` added by @MichaReiser on 2023-08-27 08:54_

---

_Label `playground` added by @MichaReiser on 2023-08-27 08:54_

---

_Branch deleted on 2023-08-27 11:10_

---
