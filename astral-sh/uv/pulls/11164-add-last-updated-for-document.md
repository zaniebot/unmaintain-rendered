```yaml
number: 11164
title: "Add ``last updated`` for document"
type: pull_request
state: merged
author: FishAlchemist
labels: []
assignees: []
merged: true
base: main
head: add_last_updated
created_at: 2025-02-02T11:09:02Z
updated_at: 2025-06-17T06:22:55Z
url: https://github.com/astral-sh/uv/pull/11164
synced_at: 2026-01-10T11:10:34Z
```

# Add ``last updated`` for document

---

_Pull request opened by @FishAlchemist on 2025-02-02 11:09_

## Summary
![image](https://github.com/user-attachments/assets/75431f9f-debe-435d-a02e-d216be7a3a01)
![image](https://github.com/user-attachments/assets/2d1b895e-4878-410e-90ff-ff8e932cbf24)
Display the last document update time, excluding any automatically generated parts of the document, while ensuring that Google can accurately read and recognize the webpage's time.

Note that I do not have permission to update ``requirements-insiders.txt``


Google time info
* https://developers.google.com/search/blog/2019/03/help-google-search-know-best-date-for
* https://developers.google.com/search/docs/appearance/structured-data/article#amp

Similar https://github.com/astral-sh/uv/pull/11162
Closes #11148
## Test Plan
uvx --with-requirements docs/requirements.txt -- mkdocs serve -f mkdocs.public.yml --strict
![image](https://github.com/user-attachments/assets/6e8cd609-2e60-489c-97cc-fb28aa3204e0)
The correct format is actually ``2024-08-08T22:01:08Z``, but Google Search happens to be lenient and accepts this format.
![image](https://github.com/user-attachments/assets/2ec8ce98-49ea-403b-bbd2-3d0d5630a562)



---

_Comment by @FishAlchemist on 2025-02-02 11:12_

I just stepped out, planning to set up Google time and then open a PR when I got back. Unexpectedly, someone beat me to it. See which version you want to use, I just don't want my time to be wasted, so I'm throwing it up for reference.
https://github.com/astral-sh/uv/pull/11162

---

_Comment by @FishAlchemist on 2025-02-02 12:03_

By the way, the website does not currently support rich results, so even if you set a time, it will not take effect.
https://search.google.com/test/rich-results/result?id=oQWO7tBph2rXt81M4uuAKg&hl=en

---

_Renamed from "Add last updated for doc" to "Add ``last updated`` for document" by @FishAlchemist on 2025-02-02 12:03_

---

_@FishAlchemist reviewed on 2025-02-02 12:12_

---

_Review comment by @FishAlchemist on `mkdocs.public.yml`:6 on 2025-02-02 12:12_

Actually, I don't want to specifically target UTC(Default UTC), but unfortunately, ``git_revision_date_localized_raw_iso_datetime`` doesn't include the timezone offset. So, if you want to change the timezone, you also need to modify the JSON-LD information for Google Search to recognize it.

https://github.com/timvink/mkdocs-git-revision-date-localized-plugin/blob/v1.3.0/src/mkdocs_git_revision_date_localized_plugin/dates.py#L42-L45
```py
"iso_date": loc_revision_date.strftime("%Y-%m-%d"),
"iso_datetime": loc_revision_date.strftime("%Y-%m-%d %H:%M:%S"),
```

---

_@FishAlchemist reviewed on 2025-02-02 12:54_

---

_Review comment by @FishAlchemist on `.github/workflows/publish-docs.yml`:29 on 2025-02-02 12:54_

https://github.com/timvink/mkdocs-git-revision-date-localized-plugin?tab=readme-ov-file#note-when-using-build-systems-like-github-actions

---

_Review requested from @zanieb by @konstin on 2025-02-02 13:09_

---

_@charliermarsh reviewed on 2025-02-03 21:15_

---

_Review comment by @charliermarsh on `mkdocs.public.yml`:10 on 2025-02-03 21:15_

Why omit these?

---

_@zanieb reviewed on 2025-02-03 21:17_

---

_Review comment by @zanieb on `mkdocs.public.yml`:10 on 2025-02-03 21:17_

Perhaps because they're generated files? Still seems nice to include the updated time.

---

_@FishAlchemist reviewed on 2025-02-04 02:58_

---

_Review comment by @FishAlchemist on `mkdocs.public.yml`:10 on 2025-02-04 02:58_

Since they are generated from code, I think their changes should actually follow the version, so let's exclude them for now. However, it's okay to keep them if you want them to be displayed.

---

_@zanieb reviewed on 2025-02-04 03:07_

---

_Review comment by @zanieb on `mkdocs.public.yml`:10 on 2025-02-04 03:07_

For a reader though, the date updated is still relevant.

---

_@FishAlchemist reviewed on 2025-02-04 03:10_

---

_Review comment by @FishAlchemist on `mkdocs.public.yml`:10 on 2025-02-04 03:10_

@zanieb Okay, so I just revoked the exclusion.

---

_@charliermarsh approved on 2025-02-04 03:27_

Thanks!

---

_Merged by @charliermarsh on 2025-02-04 03:28_

---

_Closed by @charliermarsh on 2025-02-04 03:28_

---

_Branch deleted on 2025-06-17 06:22_

---
