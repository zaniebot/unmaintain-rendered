---
number: 12500
title: Add MCP tools
type: issue
state: open
author: alok
labels:
  - enhancement
assignees: []
created_at: 2025-03-27T00:24:05Z
updated_at: 2025-08-19T20:00:15Z
url: https://github.com/astral-sh/uv/issues/12500
synced_at: 2026-01-10T01:25:20Z
---

# Add MCP tools

---

_Issue opened by @alok on 2025-03-27 00:24_

### Summary

Git/github and many others have MCP tool servers. just for github:


```
create_or_update_file
search_repositories
create_repository
get_file_contents
push_files
create_issue
create_pull_request
fork_repository
create_branch
list_commits
list_issues
update_issue
add_issue_comment
search_code
search_issues
search_users
get_issue
get_pull_request
list_pull_requests
create_pull_request_review
merge_pull_request
get_pull_request_files
get_pull_request_status
update_pull_request_branch
get_pull_request_comments
get_pull_request_reviews
```

I've found models tend to go to lower level commands like uv pip install etc, and natively binding common uv commands could help with that

### Example

_No response_

---

_Label `enhancement` added by @alok on 2025-03-27 00:24_

---

_Referenced in [prefix-dev/pixi#3830](../../prefix-dev/pixi/issues/3830.md) on 2025-05-26 13:53_

---

_Comment by @rudy-lath-vizio on 2025-08-19 19:34_

+1

---
