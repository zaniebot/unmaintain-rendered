```yaml
number: 7053
title: "docs: separate project from configuration settings"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/separate-project-from-configuration-settings
created_at: 2024-09-04T22:08:27Z
updated_at: 2024-09-14T08:05:02Z
url: https://github.com/astral-sh/uv/pull/7053
synced_at: 2026-01-10T12:53:38Z
```

# docs: separate project from configuration settings

---

_Pull request opened by @mkniewallner on 2024-09-04 22:08_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Part of https://github.com/astral-sh/uv/issues/7007.

Settings documentation reference currently doesn't separate "project metadata" and "configuration" options, implying that it's possible to set things like `dev-dependencies` in `uv.toml` while it's not. This is an attempt at better separating those options, by having 2 different sections:
- `Project metadata`, that holds configuration that can only be set in `pyproject.toml`
- `Configuration`, that holds configuration that can be set both in `pyproject.toml` and `uv.toml`

Here are some screenshots to show what this looks like (note that I don't have code highlighting in the right navigation, which makes them clunky, as first item is always bigger because of the missing "span" -- I think that's because it's an `mkdocs-material` insider feature, since I have the same thing on `main` branch):

- Right side navigation:

<img width="241" alt="Screenshot 2024-09-05 at 01 19 50" src="https://github.com/user-attachments/assets/012f64a4-8d34-4e34-a506-8d02dc1fbf98">

<img width="223" alt="Screenshot 2024-09-05 at 01 20 01" src="https://github.com/user-attachments/assets/0b0fb71d-c9c3-4ee3-8f6e-cf35180b1a99">

- An option from "Project metadata" section that only applies to `pyproject.toml`:

<img width="788" alt="Screenshot 2024-09-05 at 01 20 11" src="https://github.com/user-attachments/assets/64349fbb-8623-4b81-a475-d6ff38c658f1">

- An option from "Configuration" section that applies both to `pyproject.toml` and `uv.toml`:

<img width="787" alt="Screenshot 2024-09-05 at 01 20 33" src="https://github.com/user-attachments/assets/732e43d3-cc64-4f5a-8929-23a5555d4c53">

## Test Plan

Local run of the documentation.

---

_Marked ready for review by @mkniewallner on 2024-09-04 23:25_

---

_Comment by @mkniewallner on 2024-09-04 23:31_

Not sure about the naming btw, went with "Project metadata" and "Configuration" but this might not be ideal. We might also consider adding a small description at the top of "Project metadata" section, to make it even more explicit why it's only possible to set those specific options in `pyproject.toml`.

---

_Comment by @charliermarsh on 2024-09-10 13:35_

One oddity here that I need to resolve is that in the pip interface (`uv pip`), we _will_ respect those settings in `uv.toml` (at least, `constraints-dependencies` etc., but not `dev-dependencies`).

---

_Comment by @mkniewallner on 2024-09-10 20:49_

> One oddity here that I need to resolve is that in the pip interface (`uv pip`), we _will_ respect those settings in `uv.toml` (at least, `constraints-dependencies` etc., but not `dev-dependencies`).

Until we find a way to properly reflect that in the documentation, wouldn't it still be preferable to move forward with the changes? I feel that this is less problematic to only show `pyproject.toml` for options where it's also possible to use `uv.toml` in `pip` interface than showing `uv.toml` where it doesn't work for either both resolvers, or the universal one.

Worst case, obviously far from great, but if too complex for now, maybe we can "hack" around the issue by adding specific note on options where `uv.toml` only works for `pip` interface, e.g. either:
- adding a note like "If using `pip` interface, it's also possible to set the option in `uv.toml` like so: <example>"
- using `uv.toml (pip interface only)` in the file header
- in the example, adding a comment "# This will only work for `pip` interface"

---

_Comment by @charliermarsh on 2024-09-10 21:31_

> Until we find a way to properly reflect that in the documentation, wouldn't it still be preferable to move forward with the changes?

I agree, it doesn't need to be blocking.

---

_Comment by @charliermarsh on 2024-09-10 21:39_

What's the motivation for increasing the header size for the options? Is it because there are now two levels of nesting ("Project" -> "Workspace" -> "Members")? I wonder if we should use `workspace.members` there (as an example).

---

_Comment by @mkniewallner on 2024-09-10 21:55_

> What's the motivation for increasing the header size for the options? Is it because there are now two levels of nesting ("Project" -> "Workspace" -> "Members")? I wonder if we should use `workspace.members` there (as an example).

That's mostly the reason yes. Basically today we hack a bit things to have `pip` and `workspace` at the same level as `Global`, but now that we have 2 main categories ("Project" and "Configuration"), we want to add one more header level.

This is not really easy to see without digging into the HTML, but this is what we currently have:
<img width="270" alt="Screenshot 2024-09-10 at 23 49 20" src="https://github.com/user-attachments/assets/39599a2d-ba29-4fa1-8663-25eec7de582c">
<img width="198" alt="Screenshot 2024-09-10 at 23 49 51" src="https://github.com/user-attachments/assets/8999b29c-e26e-490e-bb6e-b8e02c8e25ac">
<img width="252" alt="Screenshot 2024-09-10 at 23 50 23" src="https://github.com/user-attachments/assets/6b588e12-e58a-4ddc-949b-8f3a0591d501">

Now we'll have:
<img width="293" alt="Screenshot 2024-09-10 at 23 50 41" src="https://github.com/user-attachments/assets/e2e2f3fd-0933-4a47-bc1b-280d835c6f83">
<img width="256" alt="Screenshot 2024-09-10 at 23 50 58" src="https://github.com/user-attachments/assets/259835a2-c476-47ef-a153-08c3e806e85a">

---

_Merged by @charliermarsh on 2024-09-14 00:57_

---

_Closed by @charliermarsh on 2024-09-14 00:57_

---

_Label `documentation` added by @charliermarsh on 2024-09-14 00:57_

---

_Branch deleted on 2024-09-14 08:05_

---
