```yaml
number: 16398
title: "Add JSON output for the `tree` command"
type: pull_request
state: open
author: belmarca-bia
labels: []
assignees: []
base: main
head: mabelanger/tree-json
created_at: 2025-10-21T20:17:59Z
updated_at: 2025-10-24T14:56:18Z
url: https://github.com/astral-sh/uv/pull/16398
synced_at: 2026-01-12T16:12:14Z
```

# Add JSON output for the `tree` command

---

_@belmarca-bia_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds support for outputting JSON using the `tree` command. It refactored the visitor code in order to call abstract procedures instead of hardcoding the textual representation.

## Test Plan

The tests (`cargo test`) passed.

---

_Comment by @konstin on 2025-10-24 13:18_

Can you share the JSON format you chose for a real project, how does it compare to `uv pip list --format json`?

It seems that you change the iteration based code to a much more complex visitor based one. What motivated this change?

---

_Comment by @belmarca-bia on 2025-10-24 14:56_

This does something different from `uv pip list`, which just lists dependencies. It only adds a JSON tree emitter in addition to the default textual tree emitter. I'm interested in the dependency tree and not the the dependencies themselves. This would be used to taint packages/libraries to know which tests to run during CI.

I don't particularly care about the actual key names or such details for now, nor do I care about the actual implementation identifiers, etc. Just testing the waters to see if that's the kind of functionality that could be added to `uv`.

As for the bigger complexity, the advantage is `uv` can now serialize the dependency graph in multiple formats much more easily. The textual one is not great to pass as a input to another program, for example! The current code and the new code are actually functionally similar, they both run depth-first except the new code is now an actual visitor instead of just using the name `visitor`. The old method is kept in case for now but would have to go as it's basically hard-coding the serialization implementation.

```diff
-            for (visited_index, visited_line) in self.visit(*dep, visited, path).iter().enumerate()
-            {
-                let prefix = if visited_index == 0 {
-                    prefix_top
-                } else {
-                    prefix_rest
-                };
-                lines.push(format!("{prefix}{visited_line}"));
-            }
+            self.visit_with_formatter(*dep, formatter, visited, path, child_position);
```

Note that the current pattern could (should) be abstracted a little bit more as it is not limited to formatting and serializing. This same pattern could allow `uv` to perform any computation on the graph, basically.

Thanks for taking a look!

---
