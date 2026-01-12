```yaml
number: 19921
title: "[Formatter] Add `function-arg-extra-indent` option"
type: pull_request
state: closed
author: jsh9
labels: []
assignees: []
base: main
head: jsh9-function-arg-extra-indent
created_at: 2025-08-14T22:42:00Z
updated_at: 2025-08-15T06:47:35Z
url: https://github.com/astral-sh/ruff/pull/19921
synced_at: 2026-01-12T15:56:50Z
```

# [Formatter] Add `function-arg-extra-indent` option

---

_@jsh9_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

(Implements https://github.com/astral-sh/ruff/issues/8360)

This PR introduces a new formatter option, `function-arg-extra-indent`, which enables extra indentation (8 spaces instead of 4) for function arguments that span multiple lines.

This option can be configured via the CLI or configuration files, and is intended to improve readability in accordance with PEP-8 recommendations.  This option defaults to "disabled", **which means guarantees backward compatibility**.

The implementation includes updates to the formatter internals, configuration schema, CLI, documentation, and adds relevant tests and fixtures to verify the new behavior.

### _Additional context:_

This feature is probably one of the most controversial topics in `Black`'s issue board.  Many people are frustrated that `Black` doesn't add such a feature.  Discussions on both sides are filled with emotions, most notably this one: 
- https://github.com/psf/black/issues/1178

Here are some additional feature requests:

- https://github.com/grantjenks/blue/issues/89
- https://github.com/grantjenks/blue/issues/90
- https://github.com/psf/black/issues/1127
- https://github.com/psf/black/issues/2117

A similar feature request was posted on Ruff's issue board: https://github.com/astral-sh/ruff/issues/8360.

## Test Plan

<!-- How was it tested? -->

- Added and updated unit and integration tests to cover both enabled and disabled states of the `function-arg-extra-indent` option.
- Verified output via new and existing test fixtures.
- Confirmed correct CLI and config file parsing, and checked documentation updates for accuracy.

---

_Review requested from @MichaReiser by @jsh9 on 2025-08-14 22:42_

---

_Comment by @MichaReiser on 2025-08-15 06:31_

Thank you for working on this. It's great to see that you're excited about this feature. Unfortunately, I don't see a direct path for us to merge this PR because it doesn't make progress towards answering the fundamental questions raised in https://github.com/astral-sh/ruff/issues/8360#issuecomment-1891659341 and I currently don't have the capacity to make progress on those decisions myself. That's why I'll close this PR because it isn't actionable at the moment. 

I still believe the PR is valuable in the future because it demonstrates that this feature can be implemented with minimal complexity. 

Thanks again and I'm sorry for not a more positive outcome.

---

_Closed by @MichaReiser on 2025-08-15 06:31_

---

_Comment by @jsh9 on 2025-08-15 06:47_

Hi @MichaReiser , I addressed [your comment](https://github.com/astral-sh/ruff/issues/8360#issuecomment-1891659341) in another comment here: https://github.com/astral-sh/ruff/issues/8360#issuecomment-2509132055

Did you happen to have a chance to take a look? Thanks!

---
