```yaml
number: 19644
title: "Allow hiding the diagnostic severity in `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/hide-severity
created_at: 2025-07-30T17:42:25Z
updated_at: 2025-08-05T13:56:22Z
url: https://github.com/astral-sh/ruff/pull/19644
synced_at: 2026-01-12T15:56:44Z
```

# Allow hiding the diagnostic severity in `ruff_db`

---

_@ntBre_

## Summary

This PR is a spin-off from https://github.com/astral-sh/ruff/pull/19415. It enables replacing the severity and lint name in a ty-style diagnostic:

```
error[unused-import]: `os` imported but unused
```

with the noqa code and optional fix availability icon for a Ruff diagnostic:

```
F401 [*] `os` imported but unused
F821 Undefined name `a`
```

or nothing at all for a Ruff syntax error, where the previous `SyntaxError` prefix is replaced with the `invalid-syntax` lint name:

```
invalid-syntax: Expected one or more symbol names after import
```

Ruff adds the `SyntaxError` prefix to these messages manually.

Initially (d912458), I just passed a `hide_severity` flag through a bunch of calls to get it into `annotate-snippets`, but after looking at it again today, I think reusing the `None` severity/level gave a nicer result. As I note in a lengthy code comment, I think all of this code should be temporary and reverted when Ruff gets real severities, so hopefully it's okay if it feels a little hacky.

I think the main visible downside of this approach is that we can't style the asterisk in the fix availabilty icon in cyan, as in Ruff's current output. It's part of the message in this PR and any styling gets overwritten in `annotate-snippets`.

<img width="400" height="342" alt="image" src="https://github.com/user-attachments/assets/57542ec9-a81c-4a01-91c7-bd6d7ec99f99" />

Hmm, I guess reusing `Level::None` also means the `F401` isn't red anymore. Maybe my initial approach was better after all. In any case, the rest of the PR should be basically the same, it just depends how we want to toggle the severity.

## Test Plan

New `ruff_db` tests. These snapshots should be compared to the two tests just above them (`hide_severity_output` vs `output` and `hide_severity_syntax_errors` against `syntax_errors`).


---

_Label `internal` added by @ntBre on 2025-07-30 17:42_

---

_Label `diagnostics` added by @ntBre on 2025-07-30 17:42_

---

_Comment by @github-actions[bot] on 2025-07-30 17:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+29 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+29 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 12:1:8: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 21:1:11: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 21:1:19: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 21:1:50: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:11: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:16: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:22: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:24: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:27: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:34: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:48: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:49: invalid-syntax: Expected '}', found NonLogicalNewline
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:12:1: invalid-syntax: Unexpected indentation
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:13:3: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:13:4: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:13:5: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:13:6: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:14:1: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:14:2: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:4:7: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:5:14: invalid-syntax: Expected ':', found '{'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:25: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:46: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:47: invalid-syntax: Expected '}', found newline
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:9: invalid-syntax: Expected ',', found '{'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:7:13: invalid-syntax: Expected ':', found 'break'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:7:1: invalid-syntax: Unexpected indentation
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:8:28: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:9:18: invalid-syntax: Expected an expression
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| invalid-syntax: | 29 | 29 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+29 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+29 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 12:1:8: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 21:1:11: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 21:1:19: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 21:1:50: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:11: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:16: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:22: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:24: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:27: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:34: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:48: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:49: invalid-syntax: Expected '}', found NonLogicalNewline
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:12:1: invalid-syntax: Unexpected indentation
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:13:3: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:13:4: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:13:5: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:13:6: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:14:1: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:14:2: invalid-syntax: Expected a statement
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:4:7: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:5:14: invalid-syntax: Expected ':', found '{'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:25: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:46: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:47: invalid-syntax: Expected '}', found newline
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:9: invalid-syntax: Expected ',', found '{'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:7:13: invalid-syntax: Expected ':', found 'break'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:7:1: invalid-syntax: Unexpected indentation
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:8:28: invalid-syntax: Simple statements must be separated by newlines or semicolons
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:9:18: invalid-syntax: Expected an expression
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| invalid-syntax: | 29 | 29 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ntBre on 2025-07-30 18:57_

---

_Review requested from @carljm by @ntBre on 2025-07-30 18:57_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-30 18:57_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-30 18:57_

---

_Review requested from @sharkdp by @ntBre on 2025-07-30 18:57_

---

_Review requested from @dcreager by @ntBre on 2025-07-30 18:57_

---

_Review requested from @BurntSushi by @ntBre on 2025-07-30 18:57_

---

_Review request for @dcreager removed by @ntBre on 2025-07-30 18:57_

---

_Review request for @carljm removed by @ntBre on 2025-07-30 18:57_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-30 18:57_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-30 18:57_

---

_Comment by @MichaReiser on 2025-07-30 19:06_

I'll take a closer look tomorrow but my preference for `SyntaxError` is to not special case them and render `invalid-syntax: <message>`

---

_Comment by @ntBre on 2025-07-30 19:10_

I thought it would be a bit repetitive to render `invalid-syntax` in front of `SyntaxError`. I think it would disrupt the concise output to strip `SyntaxError: ` from the front of the actual message contents (unless we literally `strip_prefix` here, which I did consider in one early draft), but I agree with you otherwise.

---

_Comment by @MichaReiser on 2025-07-30 19:12_

Can't we remove the `SyntaxError:` prefix in whatever place we construct those diagnostics

---

_Comment by @ntBre on 2025-07-30 19:17_

Yes, but that will also change the `concise` diagnostics, where we wanted to preserve compatibility for now.

That's exactly what we should do as soon as we can break concise diagnostics for syntax errors, though.

---

_Comment by @ntBre on 2025-07-30 19:18_

I guess we could move the `SyntaxError` prefix into the concise rendering code for now.

Edit: after trying this, we'd actually have to move the prefix into _every_ output format except `full` to avoid changing their snapshots. (And except `gitlab`, which already uses `strip_prefix`)

---

_Comment by @MichaReiser on 2025-07-30 20:21_

> Edit: after trying this, we'd actually have to move the prefix into every output format except full to avoid changing their snapshots. (And except gitlab, which already uses strip_prefix)

I'm sort of fine if they change. They should all contain the lint id and the message, so that the prefix seems unnecessary

---

_@MichaReiser reviewed on 2025-07-31 06:23_

I now took a closer look. I'd prefer to avoid the invalid syntax special casing. An empty id feels hacky. I'd simply make invalid syntax an error like every other one (except that we use the `diagnostic.id` instead of the `secondary_code` as we should for all errors without a secondary code)

---

_Comment by @ntBre on 2025-07-31 13:25_

That totally makes sense to me, I was just worried about compatibility. Is this like what you had in mind?

```diff
-SyntaxError: Expected one or more symbol names after import
+invalid-syntax SyntaxError: Expected one or more symbol names after import
```

That's after unwrapping to the `diagnostic.id` instead of `String::default`. I'm planning also to remove the `SyntaxError:` prefix to give this:

```
invalid-syntax Expected one or more symbol names after import
```

I'd prefer having a colon there (`invalid-syntax:`), but I don't think there's an easy way to do that without also having a colon before a rule code. I think that also looks nice for a normal rule:

```
F821: Undefined name `a`
 --> undef.py:1:4      
```

but not so much with the fix indicator:

```
F401: [*] `os` imported but unused
 --> fib.py:1:8                  
```

Eh, maybe that's not so bad. What do you think?

---

_Comment by @MichaReiser on 2025-07-31 13:40_

I do like the colon! But I don't think we should make the change now as it breaks the concise output format. I think it's fine without a colon and we can create an issue to improve this later

---

_Comment by @ntBre on 2025-07-31 16:24_

Okay I think I've implemented what we discussed here and on Discord for the `full` and `concise` formats. In short, we just replaced the manual `SyntaxError:` prefix with `invalid-syntax:`, which reuses the `LintId`. All of the snapshots in b8aa43f515 fall into this category.

The `junit`, `pylint`, and `rdjson` formats already included the `LintId` for syntax errors, so I similarly accepted those without further changes in 0dc0616cf7.

The other output formats did not fall back on the `LintId`, so I went through each of them in separate commits.
- azure: 0b625a1f6c
- github: a1488feea5
- grouped: 7cec0e02db
- sarif: 199af70fae
- json and json-lines: 1cdb26ce78

The `github` format seems a bit redundant because we print `invalid-syntax` twice, but that matches what we do for noqa codes too:

```diff
 ::error title=Ruff (F821),file=[TMP]/input.py,line=2,col=5,endLine=2,endColumn=6::input.py:2:5: F821 Undefined name `y`
-::error title=Ruff,file=[TMP]/input.py,line=3,col=1,endLine=3,endColumn=6::input.py:3:1: SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+::error title=Ruff (invalid-syntax),file=[TMP]/input.py,line=3,col=1,endLine=3,endColumn=6::input.py:3:1: invalid-syntax: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
```

The `sarif` changes are slightly complicated by the `unique_rules` collection, which excludes syntax errors. I added the new enum to avoid needing to change the definition of `SarifRule::from` for `SecondaryCode`.

Initially it felt like a syntax error should also be a unique `SarifRule`, but it doesn't have most of the other fields like `linter`, `summary`, or `url`, and `name` and `code` would be the same.

---

_Comment by @ntBre on 2025-07-31 16:49_

I added a similar change for WASM as part of fixing that test failure. Instead of this:
<img width="813" height="256" alt="image" src="https://github.com/user-attachments/assets/44d6f9ad-75b0-4bf2-b301-9a8d69336f30" />

syntax errors now render as this in the playground:

<img width="817" height="258" alt="image" src="https://github.com/user-attachments/assets/e044226c-96de-43f5-8a3c-b8a6a1dc7202" />


---

_Review comment by @MichaReiser on `crates/ruff/tests/snapshots/lint__output_format_github.snap`:21 on 2025-07-31 16:55_

I agree, this feels odd but it is consistent to how we handle the other rules. I'm fine leaving this as is (seems like a separate change)

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:227 on 2025-07-31 16:56_

I think this comment is now outdated

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/concise.rs`:174 on 2025-07-31 16:58_

This is much better

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/full.rs`:47 on 2025-07-31 16:58_

Same, this improves ty's syntax error diagnostics by a lot

---

_@MichaReiser reviewed on 2025-07-31 17:03_

The snapshot changes look good to me. I'll review the rendering changes tomorrow

---

_Comment by @ntBre on 2025-07-31 20:08_

I think the ecosystem check makes sense now. I could make the regex look for either `SyntaxError` or `invalid-syntax` temporarily to get +0/-0, but I think this will be fine after this PR is merged.

---

_Review comment by @MichaReiser on `python/ruff-ecosystem/ruff_ecosystem/check.py`:48 on 2025-08-01 07:32_

We could probably change this to `[a-z\-]+` (I didn't test this pattern) to future prove (e.g. `io-error:`)

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:217 on 2025-08-01 07:34_

Nice comment! This helped me a lot to understand what's going on. Thanks for writing it

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:963 on 2025-08-01 07:35_

Can this be private?

---

_@MichaReiser approved on 2025-08-01 07:37_

Thank you! 

---

_Comment by @ntBre on 2025-08-01 15:45_

Thanks for the review! I fixed your comments and also added a small commit (1ae8695248) fixing the styling issue I pointed out in the summary (`Severity::None` meant the error code was unformatted). Not sure why I didn't think of it before, but this also fits into the TODO about severities, so I added an additional note to the comment to be sure we catch it.

With that, the diagnostics look quite similar in color to Ruff's current `full` output:

<img width="707" height="426" alt="image" src="https://github.com/user-attachments/assets/c1ce2f76-b29f-449f-93dc-55eadd9dbb92" />

It's still a bit of a shame about the missing cyan in the fixability marker, but I don't think there's a good way to fix that unless we add an `is_fixable` flag to the `Annotation` type in `annotate-snippets`. I guess that's not such a big deal, though, if it sounds good to you?

(I temporarily switched `--output-format concise` to trigger the new `full` rendering in `myruff`, only locally!)

---

_Comment by @MichaReiser on 2025-08-01 15:54_

> It's still a bit of a shame about the missing cyan in the fixability marker, but I don't think there's a good way to fix that unless we add an is_fixable flag to the Annotation type in annotate-snippets

At this point it will be very hard to every use the upstream crate again because I don't think we can convince them to add this as a feature. But who knows, maybe the have something much more flexible. 

I think I'd be okay with adding it

---

_Comment by @ntBre on 2025-08-01 16:43_

I got something working, but it's a little more invasive than I hoped. I didn't realize there were at least two distinct `Annotation` types in `annotate-snippets`, so I had to pass it through `snippet::Message` instead of `snippet::Annotation` like I was expecting. `snippet::Annotation` never gets converted directly into a `display_list::Annotation`, as far as I can tell. The `Message` header is rendered separately from its `snippets`, which contain the `snippet::Annotation`s, so the header rendering doesn't have access to the `snippet::Annotation`s.

The asterisk is also bold, but I figured that was close enough, in lieu of plumbing another style through `annotate-snippets`.

The top output is again from this branch.

<img width="537" height="408" alt="image" src="https://github.com/user-attachments/assets/35dd125b-2fdb-4741-9e28-badfd3db7607" />


---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:260 on 2025-08-01 20:43_

Do we need to gate this behind hide_severity or could we always render it after the severity/and id (unconditionally) if `is_fixable` is true

---

_@MichaReiser approved on 2025-08-01 20:44_

---

_@ntBre reviewed on 2025-08-01 20:54_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:260 on 2025-08-01 20:54_

Oh good point. I don't think we strictly need to gate it, but it would look something like this if we had a fixable ty-style diagnostic right now:

```
error[invalid-assignment][*]: Object of type `Literal[1]` is not assignable to `str`
```

I guess that looks pretty good, and it should be unreachable currently, in any case. I'll move it down!

---

_Merged by @ntBre on 2025-08-05 13:56_

---

_Closed by @ntBre on 2025-08-05 13:56_

---

_Branch deleted on 2025-08-05 13:56_

---
