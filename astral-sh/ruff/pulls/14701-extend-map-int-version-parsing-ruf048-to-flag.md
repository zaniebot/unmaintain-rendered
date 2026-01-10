```yaml
number: 14701
title: "Extend `map-int-version-parsing (RUF048)` to flag `tuple(map(int, importlib.metadata.version(\"<package>\").split(\".\")))`"
type: pull_request
state: closed
author: harupy
labels: []
assignees: []
draft: true
base: main
head: RUF048-importlib-metadata-version
created_at: 2024-12-01T15:36:17Z
updated_at: 2024-12-03T13:59:29Z
url: https://github.com/astral-sh/ruff/pull/14701
synced_at: 2026-01-10T20:42:27Z
```

# Extend `map-int-version-parsing (RUF048)` to flag `tuple(map(int, importlib.metadata.version("<package>").split(".")))`

---

_Pull request opened by @harupy on 2024-12-01 15:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Extend `map-int-version-parsing (RUF048)` to flag `tuple(map(int, importlib.metadata.version("<package>").split(".")))`

## Test Plan

<!-- How was it tested? -->

New test case

## Code search

A quick [code search](https://github.com/search?q=%2Ftuple%5C%28map%5C%28int%2C+importlib%5C.metadata%5C.version%5C%28%5C%22%5Cw%2B%22%5C%29.split%5C%28%5B%22%27%5D%5C.%5B%22%27%5D%5C%29%5C%29%5C%29%2F+language%3APython&type=code) shows a few results.

---

_Closed by @harupy on 2024-12-03 13:59_

---
