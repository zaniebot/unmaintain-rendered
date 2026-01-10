```yaml
number: 15053
title: "feat(audit): add audit command for Python package vulnerability scanning"
type: pull_request
state: closed
author: nyudenkov
labels: []
assignees: []
draft: true
base: main
head: feat/9189-uv-audit
created_at: 2025-08-04T06:54:11Z
updated_at: 2025-09-23T19:19:11Z
url: https://github.com/astral-sh/uv/pull/15053
synced_at: 2026-01-10T06:36:15Z
```

# feat(audit): add audit command for Python package vulnerability scanning

---

_Pull request opened by @nyudenkov on 2025-08-04 06:54_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->


## Summary

This PR implements the `uv audit` command as requested in issue #9189. The new command scans Python project dependencies for known security vulnerabilities offering multiple vulnerability data sources and several output formats.

### Multiple Vulnerability Data Sources
- **PyPA Advisory Database** (default) - ZIP download with local caching [https://github.com/pypa/advisory-database/archive/main.zip]
- **PyPI JSON API** - Direct queries to PyPI for real-time data  [https://pypi.org/pypi/{name}/{version}/json]
- **OSV.dev API** - Queries to Open Source Vulnerabilities database [https://api.osv.dev/v1]

### Flexible Output Formats
- **Human-readable**
- **JSON**
- **SARIF**

**Command Usage:**
```bash
# Basic vulnerability scanning
uv audit                           # scan current project

# Choose vulnerability data source
uv audit --source pypa-zip         # PyPA Advisory DB (default, fastest)
uv audit --source pypi             # PyPI JSON API
uv audit --source osv              # OSV.dev API

# Output formats for different use cases
uv audit --format json             # automation & CI/CD
uv audit --format sarif            # GitHub/GitLab integration
uv audit --output security-report.json  # save to file

# Advanced filtering and control
uv audit --severity critical,high  # only serious vulnerabilities
uv audit --ignore GHSA-xxxx-xxxx-xxxx CVE-2023-12345  # skip specific issues
uv audit --no-cache               # bypass local cache
```

## Test Plan

**Unit Tests:**
- 37 test cases covering all major functionality
- Cache management and TTL handling
- Output format generation (JSON, SARIF, human-readable)
- Error handling for missing files and network issues

**Integration Tests:**
- CLI command parsing and execution
- Multiple output format validation
- Cache behavior verification

**Manual Testing:**
- Tested against real Python projects with known vulnerabilities
- Verified output formats are properly structured
- Confirmed performance with large dependency trees
- Validated caching reduces subsequent scan times

**Edge Cases Covered:**
- Missing `uv.lock` and `pyproject.toml` files
- Network connectivity issues
- Malformed vulnerability data
- Empty projects and projects with no vulnerabilities
- Invalid command line arguments

Resolves #9189

---

_Converted to draft by @nyudenkov on 2025-08-04 07:06_

---

_Marked ready for review by @nyudenkov on 2025-08-04 14:38_

---

_Comment by @nyudenkov on 2025-08-04 15:17_

@zanieb, hi! Requesting someone to make a review and help with these failing windows tests :')

---

_Assigned to @zanieb by @zanieb on 2025-08-04 15:51_

---

_Converted to draft by @nyudenkov on 2025-08-05 19:02_

---

_Closed by @nyudenkov on 2025-08-06 21:49_

---

_Comment by @MattTheCuber on 2025-08-06 22:31_

Why close this PR?

---

_Comment by @nyudenkov on 2025-08-06 22:34_

@MattTheCuber currently I'm moving it to a standalone tool because rn, unfortunately, uv team can't review and approve this

---

_Comment by @MattTheCuber on 2025-08-06 22:38_

That is most unfortunate. I was excited to utilize this feature directly in UV.

---

_Comment by @nyudenkov on 2025-08-07 05:03_

@MattTheCuber 
https://github.com/nyudenkov/pysentry
Here it is :) 
Would be glad to hear feedback and suggestions on how to improve

---

_Branch deleted on 2025-09-23 19:19_

---
