---
number: 17006
title: "Add `--isolated-lock` flag to `uv sync`"
type: pull_request
state: open
author: terror
labels: []
assignees: []
base: main
head: isolated-lock-sync
created_at: 2025-12-05T22:36:26Z
updated_at: 2025-12-06T00:20:59Z
url: https://github.com/astral-sh/uv/pull/17006
synced_at: 2026-01-07T13:12:19-06:00
---

# Add `--isolated-lock` flag to `uv sync`

---

_Pull request opened by @terror on 2025-12-05 22:36_

Resolves https://github.com/astral-sh/uv/issues/17000

This diff adds a new `--isolated-lock` flag to `uv sync` that performs a full resolution and installs packages into the project environment, but does not persist changes to the `uv.lock` file.

As seen in the issues, this can be useful for testing alternate resolution strategies (e.g., `--resolution=lowest-direct`) in CI or tools like `nox` without modifying the committed lockfile.

---

_Comment by @zanieb on 2025-12-06 00:20_

Note we'll need consensus on the flag still, that was just an example.

---
