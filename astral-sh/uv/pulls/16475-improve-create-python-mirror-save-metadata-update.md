```yaml
number: 16475
title: "Improve create-python-mirror - Save metadata & Update URLs"
type: pull_request
state: open
author: plucia-mitre
labels: []
assignees: []
base: main
head: plucia-mitre/create-python-mirror-improvements
created_at: 2025-10-28T02:12:02Z
updated_at: 2025-10-29T21:02:16Z
url: https://github.com/astral-sh/uv/pull/16475
synced_at: 2026-01-12T16:12:16Z
```

# Improve create-python-mirror - Save metadata & Update URLs

---

_@plucia-mitre_

## Summary

As mentioned in #12838, improve the `create-python-mirror.py` script to (optionally) save filtered metadata with (optionally) updated URLs.

This adds the following arguments:

* `--save-metadata [filepath]` - Indicates metadata should be saved. If a file path is given after this argument the metadata is saved to the given file. If no path is given, the metadata is saved to the `--target` argument value (or it's default value) with the filename `download-metadata.json`. 
* `--mirror-url <url>` - Updates URLs in the saved metadata to the given URL.

## Test Plan

Some ad-hoc testing has been performed. I'm not sure there's a more formal test procedure for this script.


---

_Comment by @plucia-mitre on 2025-10-29 21:02_

Tagging @MeitarR since you created #12838 and I wanted to make sure this crossed your inbox. 😄 


---
