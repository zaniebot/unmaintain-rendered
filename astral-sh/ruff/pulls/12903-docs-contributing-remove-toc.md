```yaml
number: 12903
title: "docs(contributing): remove TOC"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/hide-contributing-toc-in-mkdocs
created_at: 2024-08-15T10:47:18Z
updated_at: 2024-08-19T09:38:09Z
url: https://github.com/astral-sh/ruff/pull/12903
synced_at: 2026-01-10T21:38:32Z
```

# docs(contributing): remove TOC

---

_Pull request opened by @mkniewallner on 2024-08-15 10:47_

## Summary

Update: Ended up completely removing TOC, because of https://github.com/astral-sh/ruff/pull/12903#issuecomment-2292687490.

`docs/contributing.md` is sourced from `CONTRIBUTING.md`, where a manual TOC is generated (so that GitHub users can have a TOC as well). In the `mkdocs` documentation, this is redundant with the navigation bar (https://docs.astral.sh/ruff/contributing/).

I did not find an `mkdocs`-specific way of hiding content, so since we have an extra CSS that only applies to `mkdocs`, used a div with a specific id to hide the section in `extra.css`.

Also removed the usage of `[!NOTE]`, since it's GitHub specific, so it's displayed as is in `mkdocs` (https://docs.astral.sh/ruff/contributing/#benchmarking-and-profiling).

## Test Plan

Local run of the documentation.

---

_Marked ready for review by @mkniewallner on 2024-08-15 10:51_

---

_Comment by @dhruvmanila on 2024-08-16 03:53_

I think I'd just remove the TOC from `CONTRIBUTING.md` as GitHub provides it as well:
 
<img width="1728" alt="Screenshot 2024-08-16 at 09 22 37" src="https://github.com/user-attachments/assets/76878744-c934-4f3f-9273-d4d7441f7910">


---

_Comment by @mkniewallner on 2024-08-16 18:11_

> I think I'd just remove the TOC from `CONTRIBUTING.md` as GitHub provides it as well:
> <img alt="Screenshot 2024-08-16 at 09 22 37" width="1728" src="https://private-user-images.githubusercontent.com/67177269/358462571-76878744-c934-4f3f-9273-d4d7441f7910.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjM4MzIxMDgsIm5iZiI6MTcyMzgzMTgwOCwicGF0aCI6Ii82NzE3NzI2OS8zNTg0NjI1NzEtNzY4Nzg3NDQtYzkzNC00ZjNmLTkyNzMtZDRkNzQ0MWY3OTEwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA4MTYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwODE2VDE4MTAwOFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTQ5MWNkMmU2MDY4YmE4NjJjNGY4OWFkNmJiZWMxZTQwZDExOTFkMGJlZmVmMmY5YjM2NGRmZDhhNTYwZjU0MDEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.Ij-OXRSgJEhcW_htwZD5QDQLj342NZVagMppZkGIpFg">

Oh nice catch, I didn't know that GitHub does that!

---

_Renamed from "docs(contributing): hide TOC in `mkdocs`" to "docs(contributing): remove TOC" by @mkniewallner on 2024-08-16 18:11_

---

_Comment by @MichaReiser on 2024-08-19 09:35_

Thanks

---

_Label `documentation` added by @MichaReiser on 2024-08-19 09:35_

---

_Merged by @MichaReiser on 2024-08-19 09:38_

---

_Closed by @MichaReiser on 2024-08-19 09:38_

---
