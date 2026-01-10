```yaml
number: 21502
title: "[ty] Add hyperlinks to rule codes in CLI"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: micha/error-links
created_at: 2025-11-17T18:41:57Z
updated_at: 2025-11-18T15:37:01Z
url: https://github.com/astral-sh/ruff/pull/21502
synced_at: 2026-01-10T16:53:56Z
```

# [ty] Add hyperlinks to rule codes in CLI

---

_Pull request opened by @MichaReiser on 2025-11-17 18:41_

## Summary

This PR adds hyperlink rendering to our diagnostics so that users can jump to the rule documentation by clicking on the diagnostic id.

My first implementation added an optional `url_resolver: &dyn Fn(&DiagnosticId) -> Option<String>` argument to the full and concise rendering. The main idea was to avoid constructing the URLs unnecessarily when the output format doesn't need the URL. 

However, I then realised that the LSP always uses the URL and we have other output formats (ruff only for now) that construct the URL too, namely the JSON and rdjson output formats. The overhead also seems negligible compared to that we always construct the full diagnostic, even when only using the `concise` output format. That's why I opted to store the URL as an optional field on the diagnostic instead. Not only did it simplify the implementation, it also reduced some code duplication.

The main advantage of this approach is that it requires some amount of code duplication to set the URL on the diagnostic. 

It should be very easy to leverage this functionality in Ruff too (@ntBre if you're interested).


## Test Plan
https://github.com/user-attachments/assets/9855ca70-f92d-4299-b847-360be8ea1f89



---

_Label `ty` added by @MichaReiser on 2025-11-17 18:41_

---

_Label `diagnostics` added by @MichaReiser on 2025-11-17 18:41_

---

_@MichaReiser reviewed on 2025-11-17 18:42_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/snippet.rs`:16 on 2025-11-17 18:42_

This type is inspired by the new annotate snippet that also has an `Id` type

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 18:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-17 18:53_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @MichaReiser on 2025-11-18 08:27_

---

_Review requested from @carljm by @MichaReiser on 2025-11-18 08:27_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-18 08:27_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-18 08:27_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-18 08:27_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-11-18 08:27_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-18 08:27_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-18 08:27_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-18 08:27_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-18 08:35_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 08:41_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://micha-error-links.ecosystem-663.pages.dev/diff)** ([timing results](https://micha-error-links.ecosystem-663.pages.dev/timing))




---

_Renamed from "[ty] Render links for rule codes" to "[ty] Add hyperlinks to rule codes in CLI" by @MichaReiser on 2025-11-18 08:44_

---

_Comment by @AlexWaygood on 2025-11-18 08:50_

Interesting that mypy_primer timed out but ecosystem-analyzer didn't find any changes to report (primer timeouts usually occur because it took too long trying to compute the diff between the two runs)

---

_Comment by @MichaReiser on 2025-11-18 09:15_

mypy primer timing out makes sense, given that it forces color output (every diagnostic has now a different output)

---

_Comment by @MichaReiser on 2025-11-18 09:34_

There's an argument that it's hard to notice that the id is clickable. We could decide to underline the id or add the URL as a note (similar to what we do with the rule status). This would have the benefit that we could render the URL as a string and it's up to the CLI to add the hyperlink. The downside of this is that it only works for the `full` output and not concise.

I'm also not a 100% sure if we need to restrict the hyperlink rendering to more cases, e.g. by using https://docs.rs/supports-hyperlinks/latest/src/supports_hyperlinks/lib.rs.html#12-53

---

_@BurntSushi approved on 2025-11-18 13:44_

Love it! Nice work. This all LGTM. I agree that the overhead here is likely negligible, and so preferring API simplicity is the right call.

> There's an argument that it's hard to notice that the id is clickable. We could decide to underline the id or add the URL as a note (similar to what we do with the rule status). This would have the benefit that we could render the URL as a string and it's up to the CLI to add the hyperlink. The downside of this is that it only works for the full output and not concise.

I like the underline.

> I'm also not a 100% sure if we need to restrict the hyperlink rendering to more cases, e.g. by using https://docs.rs/supports-hyperlinks/latest/src/supports_hyperlinks/lib.rs.html#12-53

I'd be fine with doing this reactively, i.e., in response to user feedback.

We could also choose to add a note containing the actual URL if and only if we decide not to render the hyperlink (based on `supports-hyperlink`).

But I'm fine shipping this as-is and iterating. :-)

---

_Comment by @AlexWaygood on 2025-11-18 13:59_

CMD+clicking on the rule code works on this branch for me in a terminal inside VSCode, but not in the Terminal app on my macbook -- any idea why? (Is this just a case of "Alex, start using iTerm2 or something instead of the default Terminal app that your macbook came with"?)

Adding the full URL as a note might resolve this, since I'm definitely able to CMD+click on full URL in a terminal (I do this all the time to open local playground deploys in a browser straight from my terminal, obviously 😄)

---

_Comment by @MichaReiser on 2025-11-18 15:06_

> CMD+clicking on the rule code works on this branch for me in a terminal inside VSCode, but not in the Terminal app on my macbook -- any idea why? (Is this just a case of "Alex, start using iTerm2 or something instead of the default Terminal app that your macbook came with"?)

I couldn't find a reference but yes, this is the behavior of terminals supporting the ANSI escape but not supporting the hyperlink feature. 

---

_Comment by @MichaReiser on 2025-11-18 15:36_

I actually like the current behavior where Ghostty only renders the underline when I hit Cmd and hover the id. This makes it behave the same as other clickable links

---

_Merged by @MichaReiser on 2025-11-18 15:37_

---

_Closed by @MichaReiser on 2025-11-18 15:37_

---

_Branch deleted on 2025-11-18 15:37_

---
