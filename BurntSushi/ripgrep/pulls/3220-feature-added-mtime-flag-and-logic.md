```yaml
number: 3220
title: "[FEATURE] Added --mtime flag and logic"
type: pull_request
state: closed
author: gege42o
labels: []
assignees: []
base: master
head: feature/mtime_and_ctime
created_at: 2025-11-16T10:25:10Z
updated_at: 2025-11-16T12:34:30Z
url: https://github.com/BurntSushi/ripgrep/pull/3220
synced_at: 2026-01-12T18:23:15Z
```

# [FEATURE] Added --mtime flag and logic

---

_@gege42o_

## Feature Description

The `--mtime` flag allows users to filter files based on their modification time before searching them. 
### Syntax

The flag accepts three formats, matching the behavior of `find -mtime`:

| Format | Description | Example |
|--------|-------------|---------|
| `--mtime N` | Files modified exactly N days ago | `--mtime 3` |
| `--mtime -N` | Files modified less than N days ago (within last N days) | `--mtime -7` |
| `--mtime +N` | Files modified more than N days ago (older than N days) | `--mtime +30` |

### Behavior Characteristics

1. **Fractional Day Handling**: When calculating days ago, fractional parts are **truncated** (not rounded)
   - A file modified 1.9 days ago is considered "1 day old"
   - `--mtime +1` requires at least 2 full days

2. **24-Hour Periods**: Each day is exactly 24×60×60 = 86,400 seconds

3. **Filter-Only**: This flag only affects which files are searched, not the output format

4. **Metadata Required**: Files without accessible metadata are skipped with debug logging

---

### Data Flow

1. **Parsing Phase**: User provides `--mtime VALUE`
   - Value is parsed to determine format (`+`, `-`, or plain number)
   - Converted to `MtimeFilter` enum variant
   - Stored in `LowArgs.mtime`

2. **Configuration Phase**: Arguments converted from low to high level
   - `LowArgs.mtime` → `HiArgs.mtime`
   - No transformation, direct copy

3. **Execution Phase**: Files are filtered during traversal
   - `HiArgs.mtime` → `HaystackBuilder.mtime_filter`
   - For each file encountered:
     - Get file metadata
     - Extract modification time
     - Compare against filter
     - Include/exclude file

---

_Converted to draft by @gege42o on 2025-11-16 10:28_

---

_Marked ready for review by @gege42o on 2025-11-16 11:00_

---

_Comment by @gege42o on 2025-11-16 11:36_

It appears that the feature works and is not flagged by rust-fmt. The errors are related to the help message but I don't know exactly where it is wrong. A little help would be appreciated and if needed I will provide further explanations about the code, idea and execution.

---

_Comment by @BurntSushi on 2025-11-16 12:10_

If someone other than me is going to add this to ripgrep, then it needs design first before going to a PR. See #3214.

There are a number of issues with the approach here. Like being limited to days. Or always assuming 1 day is 24 hours (some are longer, some are shorter). And I'm not a fan of the name "mtime."

---

_Closed by @BurntSushi on 2025-11-16 12:10_

---

_Comment by @gege42o on 2025-11-16 12:34_

I understand, I saw the same issue before i started designing this. Wanted to do a 1:1 copy to find's --mtime that works only for days because thats what the person who wrote the original issue wanted. What names would be suitable for this option? Maybe I can try again while using jiff.

---
