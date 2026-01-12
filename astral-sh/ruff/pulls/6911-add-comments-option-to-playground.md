```yaml
number: 6911
title: Add comments option to playground
type: pull_request
state: merged
author: cnpryer
labels:
  - playground
assignees: []
merged: true
base: main
head: playground-comments
created_at: 2023-08-27T02:17:20Z
updated_at: 2023-08-28T12:31:07Z
url: https://github.com/astral-sh/ruff/pull/6911
synced_at: 2026-01-12T02:45:38Z
```

# Add comments option to playground

---

_Pull request opened by @cnpryer on 2023-08-27 02:17_

Closes #6509

This PR adds a comments option to the playground by implementing a public `pretty_comments` function in `ruff_python_formatter`.

![image](https://github.com/astral-sh/ruff/assets/14341145/aa3d6702-d088-41db-904e-2c75a25f0ec3)


TODO:
- [x] Public `print_comments` or similar function [(comment)](https://github.com/astral-sh/ruff/issues/6509#issuecomment-1694546839)


---

_@cnpryer reviewed on 2023-08-27 02:17_

---

_Review comment by @cnpryer on `playground/src/Editor/Icons.tsx`:130 on 2023-08-27 02:17_

I have to look into SVG codes

---

_@cnpryer reviewed on 2023-08-27 02:23_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/debug.rs`:40 on 2023-08-27 02:23_

Ignore this

---

_@cnpryer reviewed on 2023-08-27 02:23_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/mod.rs`:285 on 2023-08-27 02:23_

Ignore this

---

_@cnpryer reviewed on 2023-08-27 02:23_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/mod.rs`:466 on 2023-08-27 02:23_

Ignore this

---

_@cnpryer reviewed on 2023-08-27 02:23_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/context.rs`:43 on 2023-08-27 02:23_

Ignore this

---

_@cnpryer reviewed on 2023-08-27 02:24_

---

_Review comment by @cnpryer on `crates/ruff_wasm/Cargo.toml`:25 on 2023-08-27 02:24_

Ignore this

---

_@cnpryer reviewed on 2023-08-27 02:24_

---

_Review comment by @cnpryer on `crates/ruff_wasm/src/lib.rs`:25 on 2023-08-27 02:24_

Ignore this

---

_Comment by @codspeed-hq[bot] on 2023-08-27 02:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cnpryer:playground-comments)

### Merging #6911 will **not alter performance**

<sub>Comparing <code>cnpryer:playground-comments</code> (bf1083b) with <code>main</code> (eae59cf)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-08-27 02:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add comments to playground" to "Add comments option to playground" by @cnpryer on 2023-08-27 20:48_

---

_@cnpryer reviewed on 2023-08-27 22:36_

---

_Review comment by @cnpryer on `playground/src/Editor/setupMonaco.tsx`:625 on 2023-08-27 22:36_

I tried looking into highlighting for \`code snippets\`, but couldn't figure it out with a first-pass.

---

_@cnpryer reviewed on 2023-08-27 22:37_

---

_Review comment by @cnpryer on `playground/src/Editor/Icons.tsx`:130 on 2023-08-27 22:37_

IIUC it's just a type of encoding? I'll revisit if it's preferred.

---

_Marked ready for review by @cnpryer on 2023-08-27 22:38_

---

_@cnpryer reviewed on 2023-08-27 22:38_

---

_Review comment by @cnpryer on `crates/ruff_wasm/src/lib.rs`:269 on 2023-08-27 22:38_

Left this here intentionally

---

_Review comment by @cnpryer on `playground/src/Editor/SecondaryPanel.tsx`:70 on 2023-08-27 22:39_

Could probably call this "Formatted Comments"

---

_@cnpryer reviewed on 2023-08-27 22:39_

---

_@cnpryer reviewed on 2023-08-27 22:48_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/lib.rs`:182 on 2023-08-27 22:48_

Addresses
![image](https://github.com/astral-sh/ruff/assets/14341145/1ebf9505-d38f-44ce-9e04-4798fdc2c338)


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:182 on 2023-08-28 06:49_

Do I understand it correctly, that the empty `{}` is the expected output? Because that works without any special casing

---

_Review comment by @MichaReiser on `playground/src/Editor/setupMonaco.tsx`:625 on 2023-08-28 06:58_

That could be challenging, because you would need to define the embedded python language. 

---

_@MichaReiser approved on 2023-08-28 07:12_

This is so cool. Love it 

---

_Label `playground` added by @MichaReiser on 2023-08-28 07:13_

---

_Merged by @MichaReiser on 2023-08-28 07:26_

---

_Closed by @MichaReiser on 2023-08-28 07:26_

---

_Branch deleted on 2023-08-28 12:02_

---

_@cnpryer reviewed on 2023-08-28 12:11_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/lib.rs`:182 on 2023-08-28 12:11_

That'd be the expected output as-is (with no condition on empty comments). It was a nit of mine so I decided to print nothing if comments were 'empty'.

---

_Comment by @cnpryer on 2023-08-28 12:12_

Oooo nice `ParsedModule` refactor

---

_Comment by @MichaReiser on 2023-08-28 12:31_

This is now deployed to https://play.ruff.rs

---
