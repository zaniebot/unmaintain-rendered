```yaml
number: 11642
title: "Add an \"edit this page\" button to the docs"
type: pull_request
state: open
author: mgoin
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-02-19T21:30:59Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11642
synced_at: 2026-01-10T11:10:38Z
```

# Add an "edit this page" button to the docs

---

_Pull request opened by @mgoin on 2025-02-19 21:30_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

It's useful to be able to immediately get to the source of a page on the docs to contribute edits, so this PR aims to add this as a button on each page.

## Test Plan

Will test in the PR build

---

_Comment by @zanieb on 2025-02-19 21:38_

Thanks for contributing! You can test the documentation locally with `uvx --with-requirements docs/requirements.txt -- mkdocs serve -f mkdocs.public.yml`

Looks like this renders as

<img width="877" alt="Screenshot 2025-02-19 at 3 37 03 PM" src="https://github.com/user-attachments/assets/0a5ace07-b0b9-4732-894d-4ee18fe7492d" />

The branch name is wrong though

<img width="877" alt="Screenshot 2025-02-19 at 3 37 24 PM" src="https://github.com/user-attachments/assets/66bb7354-13dd-4b7b-b568-109eb90fc41f" />

I'm not entirely sure we want this, but an open to discussion. cc @charliermarsh 


---

_Comment by @mgoin on 2025-02-19 22:09_

Thanks so much for the prompt testing instructions! With the addition of `edit_uri: blob/main/docs/`, it seems to redirect to the right place

<img width="1389" alt="Screenshot 2025-02-19 at 5 08 38 PM" src="https://github.com/user-attachments/assets/7ab85bc7-fdbe-4b69-8040-524aa5d6a598" />

Clicking on the edit button on this page takes you to https://github.com/astral-sh/uv/blob/main/docs/index.md

---
