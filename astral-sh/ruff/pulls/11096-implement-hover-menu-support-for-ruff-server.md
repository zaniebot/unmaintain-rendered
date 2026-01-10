```yaml
number: 11096
title: "Implement hover menu support for ruff-server; Issue #10595"
type: pull_request
state: merged
author: nolanking90
labels:
  - server
assignees: []
merged: true
base: main
head: main
created_at: 2024-04-23T00:36:43Z
updated_at: 2024-04-23T20:23:53Z
url: https://github.com/astral-sh/ruff/pull/11096
synced_at: 2026-01-10T22:37:01Z
```

# Implement hover menu support for ruff-server; Issue #10595

---

_Pull request opened by @nolanking90 on 2024-04-23 00:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add support for hover menu to ruff_server, as requested in [10595](https://github.com/astral-sh/ruff/issues/10595).
Majority of new code is in hover.rs.
I reused the regex from ruff-lsp's implementation. Also reused the format_rule_text function from ruff/src/commands/rule.rs
Added capability registration in server.rs, and added the handler to api.rs.

## Test Plan

Tested in NVIM v0.10.0-dev-2582+g2a8cef6bd, configured with lspconfig using the default options (other than cmd pointing to my test build, with options "server" and "--preview"). OS: Ubuntu 24.04, kernel 6.8.0-22. 


---

_Comment by @codspeed-hq[bot] on 2024-04-23 00:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/nolanking90:main)

### Merging #11096 will **not alter performance**

<sub>Comparing <code>nolanking90:main</code> (8c3130e) with <code>main</code> (455d22c)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Label `server` added by @snowsignal on 2024-04-23 00:43_

---

_Review comment by @snowsignal on `Cargo.lock`:350 on 2024-04-23 00:51_

The lockfile changes seem unrelated to this PR - can you revert them?

---

_Comment by @github-actions[bot] on 2024-04-23 00:54_

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

_Review requested from @snowsignal by @charliermarsh on 2024-04-23 01:35_

---

_Assigned to @snowsignal by @charliermarsh on 2024-04-23 01:35_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/hover.rs`:21 on 2024-04-23 02:19_

You can borrow the URL here:
```suggestion
        std::borrow::Cow::Borrowed(&params.text_document_position_params.text_document.uri)
```

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/hover.rs`:36 on 2024-04-23 02:38_

I recommend removing `Result` from the return type and wrapping it in `Ok(...)` inside of `Hover::run_with_snapshot`. This would also let you remove the `allow(clippy::unnecessary_wraps)`.

```suggestion
pub(crate) fn hover(
    snapshot: &DocumentSnapshot,
    position: &types::TextDocumentPositionParams,
) -> Option<types::Hover> {
```

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/hover.rs`:40 on 2024-04-23 02:47_

The document already has a line index on it, so you can utilize it without having to allocate a new string.
```suggestion
    let document = snapshot.document();
    let line_number: usize = position
        .position
        .line
        .try_into()
        .expect("line number should fit within a usize");
    let line_range = document.index().line_range(
        OneIndexed::from_zero_indexed(line_number),
        document.contents(),
    );
    let line = &document.contents()[line_range];
```

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/hover.rs`:68 on 2024-04-23 03:26_

I think we could simplify things here by using a second regex, similar to [`ruff-lsp`'s implementation](https://github.com/astral-sh/ruff-lsp/blob/905bf3620581f2c9efd6692aca199ddc9b8d9373/ruff_lsp/server.py#L774-L798). Also, we should favor early returns over `.unwrap()`ing, just to be safe.

(This suggestion assumes that the function return value was changed to `Option<types::Hover>`)

```suggestion
    let noqa_regex = Regex::new(r"(?i:# (?:(?:ruff|flake8): )?(?P<noqa>noqa))(?::\s?(?P<codes>([A-Z]+[0-9]+(?:[,\s]+)?)+))?").unwrap();
    let noqa_captures = noqa_regex.captures(line)?;
    let codes_match = noqa_captures.name("codes")?;
    let codes_start = codes_match.start();
    let code_regex = Regex::new(r"[A-Z]+[0-9]+").unwrap();
    let cursor: usize = position
        .position
        .character
        .try_into()
        .expect("column number should fit within a usize");
    let word = code_regex.find_iter(codes_match.as_str()).find(|code| {
        cursor > (code.start() + codes_start) && cursor <= (code.end() + codes_start)
    })?;
```

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/hover.rs`:116 on 2024-04-23 03:27_

My fault for not explaining this better, but we should still include this line in the output, just in a modified form since this isn't the CLI:
```suggestion
    if rule.is_preview() || rule.is_nursery() {
        output.push_str(r"This rule is in preview and is not stable.");
        output.push('\n');
        output.push('\n');
    }
```

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/hover.rs`:127 on 2024-04-23 03:41_

Nit: I think we can tweak the message slightly to be more descriptive of the issue, and also log this problem.
```suggestion
    if let Some(explanation) = rule.explanation() {
        output.push_str(explanation.trim());
    } else {
        tracing::warn!("Rule {} does not have an explanation", rule.noqa_code());
        output.push_str("An issue occurred: an explanation for this rule was not found.");
    }
```

---

_@snowsignal requested changes on 2024-04-23 04:02_

Thank you for getting this done! You have the honor of being the first outside contributor for `ruff server` 😄 

There are a few things I'd like to improve before we merge this:

1. **Error resiliency** - there's a lot of `.unwrap()`s in here that could potentially panic in a real-world scenario. When I was testing this PR, I noticed that hovering over an incomplete `noqa` comment would cause the server to panic (for example: `# noqa: RUF`). I've made some suggestions that use early returns in the place of potentially panicking functions like `.unwrap()`.
1. **Code complexity** - sections of this implementation are actually more complicated than they need to be. I've left suggestions on how we can simplify them.

Once again, I appreciate you taking the time to implement this! This is going to be a nifty feature.

---

_Review comment by @nolanking90 on `Cargo.lock`:350 on 2024-04-23 12:56_

I reverted the lock file and pushed a commit, but now it's failing the check about the cargo.lock file. I'm not sure what's going on here (I assume I'm doing something wrong in git). Will return to this after I finish the other fixes. 

---

_@nolanking90 reviewed on 2024-04-23 12:56_

---

_Review requested from @snowsignal by @nolanking90 on 2024-04-23 18:01_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/hover.rs`:57 on 2024-04-23 19:59_

Nit:
```suggestion
        cursor >= (code.start() + codes_start) && cursor < (code.end() + codes_start)
```

---

_@snowsignal approved on 2024-04-23 20:03_

Excellent! This seems to be working great on VS Code. Good catch with the comparison operators! I have one small nit, but otherwise you are free to merge 👍 

---

_Comment by @snowsignal on 2024-04-23 20:07_

My bad, I actually have to run the merge. Thanks for bearing with me 🤦‍♀️ 

---

_Merged by @snowsignal on 2024-04-23 20:16_

---

_Closed by @snowsignal on 2024-04-23 20:16_

---
