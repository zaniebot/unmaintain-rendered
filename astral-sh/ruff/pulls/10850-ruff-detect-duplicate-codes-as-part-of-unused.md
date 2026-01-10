```yaml
number: 10850
title: "[`ruff`] Detect duplicate codes as part of `unused-noqa` (`RUF100`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
assignees: []
merged: true
base: main
head: ruf100-duplicate-codes
created_at: 2024-04-09T23:00:04Z
updated_at: 2024-04-27T17:10:02Z
url: https://github.com/astral-sh/ruff/pull/10850
synced_at: 2026-01-10T22:37:01Z
```

# [`ruff`] Detect duplicate codes as part of `unused-noqa` (`RUF100`)

---

_Pull request opened by @augustelalande on 2024-04-09 23:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement duplicate code detection as part of `RUF100`, mirroring the behavior of `flake8-noqa` (`NQA005`) mentioned in #850. The idea to merge the rule into `RUF100` was suggested by @MichaReiser https://github.com/astral-sh/ruff/pull/10325#issuecomment-2025535444.

## Test Plan

Test cases were added to the fixture.

---

_Comment by @github-actions[bot] on 2024-04-09 23:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @augustelalande on 2024-04-09 23:33_

Given the violations in the ecosystem check (`# noqa: PGH001, S307`), there is a question of whether duplicates should be detected based on rule (i.e. `PGH001` and `S307` are the same rule) or code (i.e. `PGH001` and `S307` are different codes).

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-10 04:23_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-04-10 04:23_

---

_Label `rule` added by @charliermarsh on 2024-04-10 04:24_

---

_Comment by @charliermarsh on 2024-04-10 04:28_

Hmm yeah... I think it would make sense to match based on _rule_, but it's confusing that the diagnostic lists the redirected code (and so, presumedly, the one that should stay).

I'd say let's do the duplicate detection based on code for now. Seems simpler.


---

_Comment by @augustelalande on 2024-04-10 15:53_

Ok I modified it to use the original code. I also had to modify the list of valid codes, to also use the original code for the fix to work. This has the added effect that redirects will no longer be updated if another rule is violated. But this might be less confusing behavior for users since codes will no longer be updated to new codes without explanation.

If we want to detect redirects and lint against them, then the user should probably be notified that they are using a redirect.

---

_Comment by @charliermarsh on 2024-04-10 15:58_

Could we add a separate "reason" for redirected codes?

---

_Comment by @augustelalande on 2024-04-10 15:59_

Ya I was just thinking about that. Do you want me to do it in this PR, or a separate one?

---

_Comment by @charliermarsh on 2024-04-10 16:00_

I'm cool with either, whichever is easier for you.

---

_Comment by @augustelalande on 2024-04-10 16:00_

Ok I will add it to this one

---

_Comment by @augustelalande on 2024-04-10 16:28_

Should redirects be a seperate rule? Technically `RUF100` is `unused-noqa` which isn't strictly true of redirects.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:95 on 2024-04-11 06:21_

Nit: you can use `at` to avoid having to offset the end by `codes_end`
```suggestion
                            TextRange::at(
                                TextSize::try_from(codes_end).unwrap(),
                                code.text_len(),
                            )
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:192 on 2024-04-11 06:39_

I think it's fine to use two vectors here but we could also use a single vector if we want. However, I think it's important that we don't leak the fact that `codes` and `code_ranges` must contain exactly the same number of elements (which it currently does because the rule implementation uses `zip`).

I would change the implementation to 

```rust
impl<'a> Codes<'a> {
    /// The codes that are ignored by the `noqa` directive.
    pub(crate) fn names(&self) -> impl Iterator<Item=&'a str> + '_ {
        self.codes.iter().map(|(code, _)| *code)
    }

    pub(crate) fn iter(&self) -> std::slice::Iter<(&str, TextRange)> {
        self.codes.iter()
    }

    /// Returns `true` if the string list of `codes` includes `code` (or an alias
    /// thereof).
    pub(crate) fn includes(&self, needle: Rule) -> bool {
        let needle = needle.noqa_code();

        self.names()
            .any(|code| needle == get_redirect_target(code).unwrap_or(code))
    }

    pub(crate) fn ranges(&self) -> impl Iterator<Item=TextRange> + '_ {
        self.codes.iter().map(|&(_, range)| range)
    }
}
```

and then only use these methods in the calling code. It's then up to the implementation to decide if it wants to use a `Vec` storing a tuple for each code or two vectors (and I leave that decision to you). 



(Note I moved the `includes` function to `Codes`. That means we should delete the `noqa::includes` function and instead use `codes.includes(rule)`)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/redirected_noqa.rs`:66 on 2024-04-11 06:41_

Nit
```suggestion
            if let Some(redirected) = get_redirect_target(original_code) {
                let mut diagnostic = Diagnostic::new(
                    RedirectedNOQA {
                        original: (*original_code).to_string(),
                        target: redirected.to_string(),
                    },
                    *code_range,
                );

                diagnostic.set_fix(Fix::safe_edit(Edit::range_replacement(
                    code.to_string(),
                    *code_range,
                )));
                diagnostics.push(diagnostic);
            }
        }
```

---

_@MichaReiser approved on 2024-04-11 06:41_

---

_@augustelalande reviewed on 2024-04-11 17:24_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/noqa.rs`:95 on 2024-04-11 17:24_

Nice

---

_@augustelalande reviewed on 2024-04-11 17:27_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/ruff/rules/redirected_noqa.rs`:66 on 2024-04-11 17:27_

Clean 👍 

---

_@augustelalande reviewed on 2024-04-11 18:22_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/noqa.rs`:192 on 2024-04-11 18:22_

Makes sense. Done.

---

_Renamed from "[`ruff`] Detect duplicate codes as part of `unused-noqa` (`RUF100`)" to "[`ruff`] Implement `redirected-noqa` (`RUF101`), also detect duplicate codes as part of `unused-noqa` (`RUF100`)" by @augustelalande on 2024-04-11 18:24_

---

_Comment by @augustelalande on 2024-04-20 01:44_

I'm gonna split this to help merge it

---

_Converted to draft by @augustelalande on 2024-04-20 01:44_

---

_Renamed from "[`ruff`] Implement `redirected-noqa` (`RUF101`), also detect duplicate codes as part of `unused-noqa` (`RUF100`)" to "[`ruff`] Detect duplicate codes as part of `unused-noqa` (`RUF100`)" by @augustelalande on 2024-04-27 04:00_

---

_Marked ready for review by @augustelalande on 2024-04-27 04:12_

---

_Comment by @augustelalande on 2024-04-27 04:13_

@charliermarsh hopefully this should be an easy merge now 😉 

---

_@charliermarsh approved on 2024-04-27 12:18_

Great, thank you!

---

_Merged by @charliermarsh on 2024-04-27 12:33_

---

_Closed by @charliermarsh on 2024-04-27 12:33_

---

_Branch deleted on 2024-04-27 17:10_

---
