```yaml
number: 13053
title: Fix several occurrences of the phrase “This options”
type: pull_request
state: merged
author: musicinmybrain
labels:
  - documentation
assignees: []
merged: true
base: main
head: this-options
created_at: 2025-04-22T12:22:08Z
updated_at: 2025-04-22T13:20:01Z
url: https://github.com/astral-sh/uv/pull/13053
synced_at: 2026-01-10T11:10:40Z
```

# Fix several occurrences of the phrase “This options”

---

_Pull request opened by @musicinmybrain on 2025-04-22 12:22_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes several occurrences of the minor typo “This options” for “This option.”
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Since this is just a typo fix in documentation and comment strings, no particular testing was conducted.

## Notes

The typo fixes in `crates/uv-cli/src/lib.rs` would affect `docs/reference/cli.md`. I assumed you might want to just re-generate the reference documention, but fixing it up manually would look like:

```diff
diff --git a/docs/reference/cli.md b/docs/reference/cli.md
index 338fa0ff9..8851ca2c0 100644
--- a/docs/reference/cli.md
+++ b/docs/reference/cli.md
@@ -355,7 +355,7 @@ uv run [OPTIONS] [COMMAND]
 
 </dd><dt id="uv-run--no-group"><a href="#uv-run--no-group"><code>--no-group</code></a> <i>no-group</i></dt><dd><p>Disable the specified dependency group.</p>
 
-<p>This options always takes precedence over default groups, <code>--all-groups</code>, and <code>--group</code>.</p>
+<p>This option always takes precedence over default groups, <code>--all-groups</code>, and <code>--group</code>.</p>
 
 <p>May be provided multiple times.</p>
 
@@ -1757,7 +1757,7 @@ uv sync [OPTIONS]
 
 </dd><dt id="uv-sync--no-group"><a href="#uv-sync--no-group"><code>--no-group</code></a> <i>no-group</i></dt><dd><p>Disable the specified dependency group.</p>
 
-<p>This options always takes precedence over default groups, <code>--all-groups</code>, and <code>--group</code>.</p>
+<p>This option always takes precedence over default groups, <code>--all-groups</code>, and <code>--group</code>.</p>
 
 <p>May be provided multiple times.</p>
 
@@ -2492,7 +2492,7 @@ uv export [OPTIONS]
 
 </dd><dt id="uv-export--no-group"><a href="#uv-export--no-group"><code>--no-group</code></a> <i>no-group</i></dt><dd><p>Disable the specified dependency group.</p>
 
-<p>This options always takes precedence over default groups, <code>--all-groups</code>, and <code>--group</code>.</p>
+<p>This option always takes precedence over default groups, <code>--all-groups</code>, and <code>--group</code>.</p>
 
 <p>May be provided multiple times.</p>
 
@@ -2855,7 +2855,7 @@ uv tree [OPTIONS]
 
 </dd><dt id="uv-tree--no-group"><a href="#uv-tree--no-group"><code>--no-group</code></a> <i>no-group</i></dt><dd><p>Disable the specified dependency group.</p>
 
-<p>This options always takes precedence over default groups, <code>--all-groups</code>, and <code>--group</code>.</p>
+<p>This option always takes precedence over default groups, <code>--all-groups</code>, and <code>--group</code>.</p>
 
 <p>May be provided multiple times.</p>
 
```


---

_Comment by @musicinmybrain on 2025-04-22 12:39_

> The typo fixes in `crates/uv-cli/src/lib.rs` would affect `docs/reference/cli.md`. I assumed you might want to just re-generate the reference documention …

I learned from the CI output that this looks like `cargo dev generate-cli-reference`, so I did that, and I’ll push the result in a second commit. It looks just like the manual diff above.

---

_@konstin approved on 2025-04-22 12:59_

Thanks!

---

_Label `documentation` added by @konstin on 2025-04-22 12:59_

---

_Merged by @charliermarsh on 2025-04-22 13:20_

---

_Closed by @charliermarsh on 2025-04-22 13:20_

---
