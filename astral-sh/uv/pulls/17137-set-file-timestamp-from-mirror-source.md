```yaml
number: 17137
title: Set file timestamp from mirror source
type: pull_request
state: open
author: keestux
labels: []
assignees: []
base: main
head: feat/timestamp-files-in-mirror
created_at: 2025-12-15T22:09:38Z
updated_at: 2025-12-16T18:20:34Z
url: https://github.com/astral-sh/uv/pull/17137
synced_at: 2026-01-10T05:49:14Z
```

# Set file timestamp from mirror source

---

_Pull request opened by @keestux on 2025-12-15 22:09_

The create-python-mirror.py script will now set the timestamp of the downloaded files.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Set the timestamp of the downloaded files as found on the originating server.

## Test Plan

Basically run the mirror script and afterwards check the timestamp of newly
downloaded files. The timestamps will be different from the current time.
Files that were already present (with correct checksum) will not be changed.


---

_Comment by @konstin on 2025-12-16 09:08_

Why do we need the file timestamp for it if we already have the SHA check?

---

_Comment by @keestux on 2025-12-16 09:27_

> Why do we need the file timestamp for it if we already have the SHA check?

It's a convenience. I'm copying the downloaded files to some other location with `rsync -ia ...`, the output will be more informative if unchanged files are skipped. Also, if I want to clean up old files I need useful timestamps.



---

_Comment by @konstin on 2025-12-16 10:11_

I wouldn't manually backdate the timestamps of files, since it can break e.g. the rsync workflow you mentioned, where rsync would then skip those files even though they were actually updated. Since the file doesn't change after the first download, rsync shouldn't re-copy it. 

Regarding cleaning up files, it seems that the timestamp is the wrong measure for this (especially backdated ones), and should rather be based on what files the deployment needs.

---
