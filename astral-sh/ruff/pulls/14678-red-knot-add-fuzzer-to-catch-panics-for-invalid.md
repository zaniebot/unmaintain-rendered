```yaml
number: 14678
title: "[red-knot] Add fuzzer to catch panics for invalid syntax"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/red-knot-fuzzer
created_at: 2024-11-29T13:32:20Z
updated_at: 2024-12-04T09:08:13Z
url: https://github.com/astral-sh/ruff/pull/14678
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Add fuzzer to catch panics for invalid syntax

---

_Pull request opened by @dhruvmanila on 2024-11-29 13:32_

## Summary

This PR adds a fuzzer harness for red knot that runs the type checker on source code that contains invalid syntax.

Additionally, this PR also updates the `init-fuzzer.sh` script to increase the corpus size to:
* Include various crates that includes Python source code
* Use the 3.13 CPython source code

And, remove any non-Python files from the final corpus so that when the fuzzer tries to minify the corpus, it doesn't produce files that only contains documentation content as that's just noise.

## Test Plan

Run `./fuzz/init-fuzzer.sh`, say no to the large dataset.
Run the fuzzer with `cargo +night fuzz run red_knot_check_invalid_syntax -- -timeout=5`


---

_Label `red-knot` added by @dhruvmanila on 2024-11-29 13:32_

---

_Comment by @github-actions[bot] on 2024-12-03 06:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @dhruvmanila on 2024-12-03 11:02_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-03 11:58_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-03 11:58_

---

_@sharkdp reviewed on 2024-12-03 13:20_

---

_Review comment by @sharkdp on `fuzz/init-fuzzer.sh`:32 on 2024-12-03 13:20_

Should we also keep stub files?

Using
```suggestion
    find . -type f -not \( -name "*.py" -or -name "*.pyi" \) -delete
```
or
```suggestion
    find . -type f -not -regex '.*\.pyi?' -delete
```


---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/red_knot_check_invalid_syntax.rs`:129 on 2024-12-03 13:37_

Constructing the `db` can be fairly expensive. It might be worth to cache the db instance to make the fuzzer faster. 

---

_@MichaReiser approved on 2024-12-03 13:38_

---

_@dhruvmanila reviewed on 2024-12-04 08:11_

---

_Review comment by @dhruvmanila on `fuzz/init-fuzzer.sh`:32 on 2024-12-04 08:11_

We could but at the end we would still use a `.py` file even if the content is coming from a stub file. The reason is that the fuzzer gives us the raw bytes of the source code that it generates.

Hmm, maybe we should run red knot once on a `.py` file and then on a `.pyi` file with the same content as we do on the corpus tests.

---

_Merged by @dhruvmanila on 2024-12-04 09:06_

---

_Closed by @dhruvmanila on 2024-12-04 09:06_

---

_Branch deleted on 2024-12-04 09:07_

---
