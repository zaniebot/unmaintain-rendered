---
number: 4080
title: "[FEATURE REQUEST] restrict pause usage"
type: issue
state: closed
author: Malix-Labs
labels: []
assignees: []
created_at: 2024-06-05T23:55:54Z
updated_at: 2024-06-06T00:07:03Z
url: https://github.com/astral-sh/uv/issues/4080
synced_at: 2026-01-07T13:12:17-06:00
---

# [FEATURE REQUEST] restrict pause usage

---

_Issue opened by @Malix-Labs on 2024-06-05 23:55_

## Problem

Some commands, such as `uv pip sync` has an ending pause (wait for an input)

Afaik, currently, the only way to remove it is to use it with the `-q, --quiet` flag
However, this flag will run it without any log message, including the ending pause

The convention about pause usage is when an user input is **required for the business logic** (i.e. confirmation)
In the case of `uv pip sync`, user input is not one of its requirements

In the case of automation (i.e. scripts, cicd...), neither a pause or no-logging is desired

## Solution

Use pauses more wisely
Possibly not having them by default except for destructive actions?
A `--pause` / `--confirm` / `--prompt` / `--input` flag would be enough to make them back for potential desire to pause

## Alternative

A `--no-pause` / `--skip-pause` / `--continue` / `-y, --yes` flag
To make commands be able to be loggable and continuous

## Workaround

Use commands with the `-q, --quiet` flag (drawback: no log message)

---

_Comment by @Malix-Labs on 2024-06-06 00:07_

I am confused as I do not seem to be running in pause for `uv pip sync` alone.
It might have been in a previous script function

---

_Closed by @Malix-Labs on 2024-06-06 00:07_

---
