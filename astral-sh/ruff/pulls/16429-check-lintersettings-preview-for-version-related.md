```yaml
number: 16429
title: "Check `LinterSettings::preview` for version-related syntax errors"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/preview-syntax-errors
created_at: 2025-02-28T03:23:28Z
updated_at: 2025-02-28T17:45:52Z
url: https://github.com/astral-sh/ruff/pull/16429
synced_at: 2026-01-12T15:55:55Z
```

# Check `LinterSettings::preview` for version-related syntax errors

---

_@ntBre_

Summary
--
Fixes part of the report in https://github.com/astral-sh/ruff/issues/16417#issuecomment-2689600543 by checking `LinterSettings::preview` in both the language server and in the playground. That now covers all calls of `Parsed::unsupported_syntax_errors`.

Test Plan
--
Manual tests in both VS Code and the playground. I also added `preview = true` to the WASM test I added earlier.


---

_Label `bug` added by @ntBre on 2025-02-28 03:23_

---

_Comment by @github-actions[bot] on 2025-02-28 03:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-02-28 04:25_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:180 on 2025-02-28 04:25_

Should we move this into `check_path` as that seems like the common denominator for the server, WASM, and the command-line?

---

_@dhruvmanila reviewed on 2025-02-28 04:26_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:180 on 2025-02-28 04:26_

Hmm, that might not be possible in the server because it needs to know whether the user has enabled `showSyntaxErrors` or not

---

_@dhruvmanila approved on 2025-02-28 04:29_

Thank you for taking this on quickly and fixing it.

---

_@ntBre reviewed on 2025-02-28 04:44_

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:180 on 2025-02-28 04:44_

Ah I think you're right. I kept putting it right next to the message/diagnostic generation code, but I think we could just truncate the vec inside of `check_path` because it's called right before each of these. I'll just have to add a new `Parsed` method because the field itself is private.

I don't think we can move this exact code unless we return an additional `unsupported_syntax_errors` from `check_path`.

---

_Comment by @ntBre on 2025-02-28 04:46_

Thanks for the quick review! I'd also like to get https://github.com/astral-sh/ruff/pull/16425 merged and then we could probably do a release if you want. That should cover all of the (known) bugs with the version-related errors.

I'm going to take a look at your suggestion in the morning, though. It's getting pretty late

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:180 on 2025-02-28 05:12_

> Hmm, that might not be possible in the server because it needs to know whether the user has enabled `showSyntaxErrors` or not

I think we could pass a `ShowSyntaxErrors` enum similar to `flags::Noqa` to `check_path` which in combination with the `settings.preview.is_enabled()` check be used to filter out syntax error diagnostics. But, `ShowSyntaxErrors` would need to be defined in `ruff_linter` as it can't be in `ruff_server` because otherwise it would create circular dependency.

---

_@dhruvmanila reviewed on 2025-02-28 05:12_

---

_@MichaReiser reviewed on 2025-02-28 07:25_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:180 on 2025-02-28 07:25_

I like the idea of unifying the handling if it doesn't introduce a lot of complexity but I think we can ship the bug fix as is. That gives us more time to explore the refactor and allows us to ship the bugfix sooner.

---

_@MichaReiser approved on 2025-02-28 07:25_

---

_Merged by @MichaReiser on 2025-02-28 08:58_

---

_Closed by @MichaReiser on 2025-02-28 08:58_

---

_Branch deleted on 2025-02-28 08:58_

---

_@ntBre reviewed on 2025-02-28 17:40_

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:180 on 2025-02-28 17:40_

I think I might just leave this alone for now if it's okay with you both. I looked into it briefly today and realized that my `parsed.unsupported_syntax_errors.truncate()` idea would require a `&mut Parsed`, which I didn't really want to add. There are four of these calls, which is annoying, but we'll also get to simply delete them all once the feature is out of preview.

Another temporary option could be passing a `preview: bool` (or enum) argument to the `unsupported_syntax_errors` method to avoid duplicating the explicit `if`s.

---

_@dhruvmanila reviewed on 2025-02-28 17:45_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:180 on 2025-02-28 17:45_

Thank you for looking into it. I think it's fine to leave it as is.

---
