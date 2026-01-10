```yaml
number: 17501
title: "ERA001: Allow Jira and gitlab references"
type: pull_request
state: open
author: maldoinc
labels:
  - rule
assignees: []
base: main
head: issue-references
created_at: 2025-04-20T19:11:42Z
updated_at: 2025-04-28T19:01:01Z
url: https://github.com/astral-sh/ruff/pull/17501
synced_at: 2026-01-10T19:03:00Z
```

# ERA001: Allow Jira and gitlab references

---

_Pull request opened by @maldoinc on 2025-04-20 19:11_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This extends the [ERA001](https://docs.astral.sh/ruff/rules/commented-out-code/) check to allow references to Jira issues and GitLab merge requests. Currently, only GitHub-style issue references are excluded from being flagged as commented-out code.

Previously, lines like the following were incorrectly flagged:

Jira issue reference
```
See: PROJ-1234
```

GitLab MR reference
```
See: !15
```

These are now correctly treated as comments, not code.


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

Added unit tests to verify the behavior.


---

_Comment by @ntBre on 2025-04-21 13:46_

Thanks for working on this! 

Does this correspond to an open issue? It's usually nice to discuss these things a bit before jumping into the implementation.

I can reproduce the Jira issue if there's a colon after `See`, but not otherwise: https://play.ruff.rs/cf52731c-e71a-4f6b-b736-1049d04b5133

so it's not really clear to me how big of an issue this is. There are several other heuristics used for determining code, not just the `HASH_NUMBER` regex, and in combination they seem to handle most of these cases.

If we decide we need to do something here still, another option is a user-configurable list of regular expressions. It sounds like that's what the [upstream linter](https://github.com/PyCQA/eradicate?tab=readme-ov-file#whitelisting) does, although I'm not sure if we have any other config options with user-facing regexes. cc @MichaReiser 

---

_Label `rule` added by @MichaReiser on 2025-04-22 17:01_

---

_Comment by @maldoinc on 2025-04-22 20:04_

There wasn't any discussion, more of a "scratch your own itch" situation. I used Ruff + Jira and this resulted in some positives. Since the github format was in, I figured why not extend it to jira. As you noted it is the `:` that makes the difference or not. 

---

_Comment by @MichaReiser on 2025-04-23 07:15_

IMO, it makes sense to have the rule being consistent with what we have for TD003. I'm a bit surprised to see that TD003 neither supports GitLab or JIRA comments, it seems.

https://github.com/astral-sh/ruff/blob/79a2c7eaa28daed1de520f35119ec0625b007188/crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs#L242-L248

But I'm okay allowlisting JIRA here, specifically `# See: PROJ-999` but I'm leaning towards adding the regex to the existing `ALLOWLIST` regex group

---

_Comment by @maldoinc on 2025-04-28 19:00_

There's already a bunch of different regex groups here but I'm not super opinionated on this. The question here is should we tie this with TD003 or keep it as is.

---
