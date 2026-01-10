```yaml
number: 14459
title: "Support `self update --output-format=json`"
type: pull_request
state: open
author: InSyncWithFoo
labels: []
assignees: []
base: main
head: self-update-json
created_at: 2025-07-04T10:06:06Z
updated_at: 2025-07-09T20:05:50Z
url: https://github.com/astral-sh/uv/pull/14459
synced_at: 2026-01-10T06:53:02Z
```

# Support `self update --output-format=json`

---

_Pull request opened by @InSyncWithFoo on 2025-07-04 10:06_

## Summary

Resolves #14455.

`self update --output-format=json` outputs the result in JSON. The outputs <em>should</em> look like these:

```jsonl
{"result":"offline"}
{"result":"externally-installed"}
{"result":"multiple-installations","current":"/home/some/bin","other":"/home/some/other/bin"}
{"result":"github-rate-limit-exceeded"}
{"result":"on-latest","version":"0.7.19"}
{"result":"would-update","from":"0.7.18","to":"latest"}
{"result":"would-update","from":"0.7.18","to":"0.7.19"}
{"result":"updated","from":null,"to":"0.7.19","tag":"0.7.19"}
{"result":"updated","from":"0.7.18","to":"0.7.19","tag":"0.7.19"}
```

## Test Plan

Unit tests.


---

_Converted to draft by @InSyncWithFoo on 2025-07-04 10:09_

---

_Comment by @InSyncWithFoo on 2025-07-04 11:30_

I'm not sure how to test the more important cases (`on-latest`, `would-update`, `updated`). Existing tests don't seem to cover them either.

---

_Marked ready for review by @InSyncWithFoo on 2025-07-04 12:34_

---
