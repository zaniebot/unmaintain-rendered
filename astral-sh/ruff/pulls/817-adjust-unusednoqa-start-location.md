```yaml
number: 817
title: "Adjust `UnusedNOQA` start location"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: adjust-M001-range
created_at: 2022-11-19T11:29:40Z
updated_at: 2022-11-19T14:40:56Z
url: https://github.com/astral-sh/ruff/pull/817
synced_at: 2026-01-12T15:55:05Z
```

# Adjust `UnusedNOQA` start location

---

_@harupy_

This PR adjusts the start location of `UnusedNOQA` to make it look less confusing.

## Before

<img width="548" alt="image" src="https://user-images.githubusercontent.com/17039389/202848331-f4186673-a180-440a-87e9-3a8dc8ac4e64.png">

It looks like this line contains a single violation but contains two.

## After

<img width="548" alt="image" src="https://user-images.githubusercontent.com/17039389/202848431-fe2b4c8d-8930-449f-ba6f-798ec14941a7.png">

It's clear that this line contains two violations.

---

_Renamed from "Adjust M001 range" to "Adjust `UnusedNOQA` range" by @harupy on 2022-11-19 11:30_

---

_Renamed from "Adjust `UnusedNOQA` range" to "Adjust `UnusedNOQA` start location" by @harupy on 2022-11-19 11:30_

---

_Merged by @charliermarsh on 2022-11-19 14:30_

---

_Closed by @charliermarsh on 2022-11-19 14:30_

---

_Branch deleted on 2022-11-19 14:40_

---
