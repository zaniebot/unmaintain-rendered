```yaml
number: 16627
title: Completed assignment.sh with data organization logic
type: pull_request
state: closed
author: MaryamAbedinnejad
labels: []
assignees: []
base: main
head: assignment
created_at: 2025-11-07T00:14:50Z
updated_at: 2025-11-07T02:55:23Z
url: https://github.com/astral-sh/uv/pull/16627
synced_at: 2026-01-10T06:28:12Z
```

# Completed assignment.sh with data organization logic

---

_Pull request opened by @MaryamAbedinnejad on 2025-11-07 00:14_

## Summary

Completed the `assignment.sh` script for the Shell/Git assignment. 
- Creates `newproject` directory with `analysis` and `output` subfolders.
- Downloads and unzips client data.
- Organizes `rawdata` into `data/raw` and processes logs into `data/processed`.
- Removes files containing "ipaddr".
- Creates `inventory.txt` listing all processed files.

## Test Plan

Tested locally by running:

```bash
bash assignment.sh


---

_Comment by @samypr100 on 2025-11-07 02:55_

Hello @MaryamAbedinnejad, I believe this PR is intended for another project and I'll be closing this as a result. Thanks!

---

_Closed by @samypr100 on 2025-11-07 02:55_

---
