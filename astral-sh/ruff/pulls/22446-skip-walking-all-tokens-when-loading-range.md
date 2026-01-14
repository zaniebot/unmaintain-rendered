```yaml
number: 22446
title: Skip walking all tokens when loading range suppressions
type: pull_request
state: open
author: amyreese
labels:
  - performance
  - preview
assignees: []
base: main
head: amy/suppression-perf-2
created_at: 2026-01-07T22:39:17Z
updated_at: 2026-01-14T08:48:07Z
url: https://github.com/astral-sh/ruff/pull/22446
synced_at: 2026-01-14T09:35:09Z
```

# Skip walking all tokens when loading range suppressions

---

_@amyreese_

Adapted from https://github.com/astral-sh/ruff/pull/21441#pullrequestreview-3503773083

issue #22087


---

_Label `internal` added by @amyreese on 2026-01-07 22:39_

---

_Label `performance` added by @amyreese on 2026-01-07 22:39_

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 22:49_


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

_Comment by @amyreese on 2026-01-08 01:20_

The original codspeed run I think showed a 1% perf *regression* on a few benchmarks, but I also don't think we have any codspeed jobs that would actually exercise this functionality yet since the range suppressions aren't likely to be found in the wild yet.

---

_Comment by @MichaReiser on 2026-01-08 08:04_

We'll probably need to temporarily remove the preview gating to see if we mitigated the perf regression that we've seen on your first PR.

---

_Comment by @amyreese on 2026-01-08 18:09_

> We'll probably need to temporarily remove the preview gating to see if we mitigated the perf regression that we've seen on your first PR.

Looks like a [1% perf hit](https://codspeed.io/astral-sh/ruff/branches/amy%2Fsuppression-perf-2?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Adefault_rules%3A%3Abenchmark_default_rules%3A%3Alinter%2Fdefault-rules%255Blarge%2Fdataset.py%255D&runnerMode=Instrumentation&sectionId=benchmark-comparison-section-comparison-passed) when ungated on `large/dataset.py`, compared to [2% perf hit](https://codspeed.io/astral-sh/ruff/branches/amy%2Fperf-enable-suppressions?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Adefault_rules%3A%3Abenchmark_default_rules%3A%3Alinter%2Fdefault-rules%255Blarge%2Fdataset.py%255D&runnerMode=Instrumentation&sectionId=benchmark-comparison-section-comparison-passed) without this PR when ungated on the same benchmark in #22461.

---

_Marked ready for review by @amyreese on 2026-01-09 19:38_

---

_Review requested from @MichaReiser by @amyreese on 2026-01-09 19:38_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:402 on 2026-01-10 09:16_

We could avoid doing two binary search by introducing a `tokens.split_at(offset)` method:

```rust
let (before_tokens, after_tokens) = tokens.split_at(suppression.range.start());

let last_indent = before_tokens.iter().rfind(...);
let tokens = after_tokens
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:399 on 2026-01-10 09:18_

I think we should clear indents after each iteration. We use it more as a scratch pad and we only use the same instance to avoid allocating multiple vecs`

```suggestion
        'comments: while let Some(suppression) = suppressions.peek() {
        		indents.clear();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:455 on 2026-01-10 09:39_

Nit: You could use let chains here

Change the tokens loop to:

```rust
let mut tokens = tokens.after(suppression.range.start()).iter();
while let Some(token) = tokens.next() {
	...
```

```suggestion
                        if indent == current_indent
                            && let Some(non_trivia) = tokens.clone().find(|t| !t.kind().is_trivia())
                            && matches!(non_trivia.kind(), TokenKind::Dedent | TokenKind::Indent)
```

Cloning `tokens` here is fine because cloning a slice iterator is cheap (it's only two pointers, one pointing to the start and one to the end of the slice)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/preview.rs`:298 on 2026-01-10 09:39_

TODO: Revert

---

_@MichaReiser approved on 2026-01-10 09:40_

Nice, thank you. 

I think it's worthwhile to introduce a `tokens.split_at` method to avoid repeating the same binary search twice only to get after and before

---

_Label `internal` removed by @MichaReiser on 2026-01-10 09:41_

---

_Label `preview` added by @MichaReiser on 2026-01-10 09:42_

---

_@amyreese reviewed on 2026-01-13 23:28_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:402 on 2026-01-13 23:28_

I tried building `split_at` and using it when loading tokens, but somehow it changes the logic, and I'm not seeing why. Running the tests results in snapshot changes that implies leading comments before indents are no longer getting loaded/matched correct. But I also don't fully understand how your logic here is working in this implementation, so I'm hoping I'm missing something. Any suggestion would be appreciated.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:425 on 2026-01-14 08:20_

Can we resolve this comment?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:449 on 2026-01-14 08:41_

You need to change this to `after`

---

_@MichaReiser reviewed on 2026-01-14 08:41_

---

_@MichaReiser reviewed on 2026-01-14 08:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/token/tokens.rs`:203 on 2026-01-14 08:42_

```suggestion
        let (before, after) = self.raw.split_at(partition_point);
```

---

_@MichaReiser reviewed on 2026-01-14 08:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/token/tokens.rs`:195 on 2026-01-14 08:48_

I think it's worth pointing out that `after` is different than calling `.after` (at least I think so).

**`.after`**

Finds the first token that ends after `offset`

```
           
| 1 |  | 2 |     | 3|
           - offset
                 ^^^^ .after
^^^^^ split_at before
       ^^^^^^^^^^^^^^ split_at after        
```

This is because `.after` finds the first token that ends after `offset` whereas `split_at` finds the first token that starts at or before `offset`

We could change `split_at` to skip over tokens that fall directly on `offset` (e.g. a `Dedent` token with zero width). For now, I think it's fine to call this difference out in the comment

---
