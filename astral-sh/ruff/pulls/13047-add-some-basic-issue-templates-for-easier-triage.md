```yaml
number: 13047
title: Add some basic Issue templates for easier triage
type: pull_request
state: closed
author: jvacek
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2024-08-22T08:33:20Z
updated_at: 2024-08-28T07:27:30Z
url: https://github.com/astral-sh/ruff/pull/13047
synced_at: 2026-01-12T15:55:42Z
```

# Add some basic Issue templates for easier triage

---

_@jvacek_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds templates for people to file bugs into. These will:
- Auto-assign some labels onto the issues
- Structure input a little more clearly

## Test Plan

N/A, this is a GitHub-specific change. You can check out what it looks like here: https://github.com/jvacek/ruff/issues/new/choose

---

_Comment by @jvacek on 2024-08-22 08:36_

The point is to make it easier to parse out what's going on from an issue, and effectively push it to the next step.

I'm very happy to hear out what your most common issues are like so I can structure this into something that would be more useful for you guys.

---

_Converted to draft by @jvacek on 2024-08-22 08:36_

---

_Comment by @jvacek on 2024-08-22 09:07_

Lot of CI running there for a non-code MR 🤔 

---

_Comment by @AlexWaygood on 2024-08-27 18:27_

Hey @jvacek, thanks so much for this -- really appreciate it! However, I'm going to close this for now as I think it's something we'd want to discuss as a team (possibly quite in depth!) before we changed it. The contributor experience people have on an open-source project is really hard to get right, and there's a lot of subtleties here, so I'd want us to make sure we had internal consensus on any changes before we made them.

Thanks again for the suggestion, though! We're hoping on putting into practice some improvements to our issue triage processes soon, and looking at ideas like this is really helpful

---

_Closed by @AlexWaygood on 2024-08-27 18:27_

---

_Comment by @jvacek on 2024-08-28 07:27_

Thanks for the nice message, understood, already had a brief chat in the discord about this.

FYI the example here is very over the top, but just wanted to show off the fields that make sense.

I'd propose keeping _only_ the version field, one markdown field for free text, and the optional field for keywords.

---
