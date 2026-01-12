```yaml
number: 12370
title: Add --force option to uv-publish to force upload
type: pull_request
state: open
author: marc-planard
labels: []
assignees: []
base: main
head: main
created_at: 2025-03-21T16:49:52Z
updated_at: 2025-05-13T07:24:45Z
url: https://github.com/astral-sh/uv/pull/12370
synced_at: 2026-01-12T16:10:14Z
```

# Add --force option to uv-publish to force upload

---

_@marc-planard_

## Summary

uv publish currently fails with the error message "error: Local file and index file do not match ... " if an attempt is made to publish a file with same name with a differing hash.
In some situation we want to be able to override this, this change adds  a `--force` option to uv publish to by-pass this and try the upload anyway.

(discussion in https://github.com/astral-sh/uv/issues/12369 )

## Test Plan

```bash
$ uv publish --index my-nexus
[...]
error: Local file and index file do not match for $FILE_NAME. If you know what you\'re doing to can still upload this file using the option `--force`. Local: sha256=xxx, Remote: sha256=yyy

$ uv publish --force --index my-nexus
[...]
Uploading $FILE_NAME ($SIZEKiB)
```


---

_Comment by @mertkarakilic on 2025-05-12 17:59_

When is this change expected to be merged. It is urgently required for one of our projects, thanks :) 

---

_Comment by @marc-planard on 2025-05-13 07:24_

hello @mertkarakilic it seems it's stalled as there's no consensus to merge it. The corresponding issue includes a workaround : https://github.com/astral-sh/uv/issues/12369#issuecomment-2786601451 

---
