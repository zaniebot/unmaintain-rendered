```yaml
number: 9121
title: Reorganize the project concept documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docs-collapsible-project
created_at: 2024-11-14T16:20:59Z
updated_at: 2024-11-19T19:52:16Z
url: https://github.com/astral-sh/uv/pull/9121
synced_at: 2026-01-12T16:08:40Z
```

# Reorganize the project concept documentation

---

_@zanieb_

- Adds a collapsible section for the project concept
- Splits the project concept document into several child documents.
- Moves the workspace and dependencies documents to under the project section
- Adds a mkdocs plugin for redirects, so links to the moved documents still work

I attempted to make the minimum required changes to the contents of the documents here. There is a lot of room for improvement on the content of each new child document. For review purposes, I want to do that work separately. I'd prefer if the review focused on this structure and idea rather than the content of the files.

I expect to do this to other documentation pages that would otherwise be very nested.

The project concept landing page and nav (collapsed by default) looks like this now:

<img width="1507" alt="Screenshot 2024-11-14 at 11 28 45 AM" src="https://github.com/user-attachments/assets/88288b09-8463-49d4-84ba-ee27144b62a5">


---

_Label `documentation` added by @zanieb on 2024-11-14 16:21_

---

_Marked ready for review by @zanieb on 2024-11-14 17:30_

---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-11-14 19:18_

---

_Review requested from @charliermarsh by @zanieb on 2024-11-14 21:21_

---

_Review requested from @dhruvmanila by @zanieb on 2024-11-14 21:21_

---

_@dhruvmanila reviewed on 2024-11-15 13:00_

---

_Review comment by @dhruvmanila on `mkdocs.template.yml`:7 on 2024-11-15 13:00_

I can't find any reference to this in mkdocs-material docs, is this a new feature?

---

_Comment by @dhruvmanila on 2024-11-15 13:56_

I think splitting larger pages into does make sense. We should enable [`navigation.path`](https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/?h=breadc#navigation-path) (navigation path breadcrumbs) so that it's easy to understand where the user is currently at. It might be problematic because of the "Introduction" page that exists currently but that can possibly be solved.

I'm still a big fan of having a third dimension to navigation aka tabs where the top level sections go but that's an unrelated discussion.

---

_Comment by @dhruvmanila on 2024-11-15 13:59_

One thing I noticed is that the footer seems to be a bit annoying when clicking on the "Projects" section because it occupies the entire screen width. I don't know if it's possible but limiting that only to the content region (and not the navigation and table of content) would be a good UX improvement IMO.

---

_@zanieb reviewed on 2024-11-15 15:47_

---

_Review comment by @zanieb on `mkdocs.template.yml`:7 on 2024-11-15 15:47_

lol, this isn't needed — I always just added it at the same time as additional nesting so I presumed it was turning it on. Will remove. Thanks!

---

_Comment by @zanieb on 2024-11-15 15:50_

> I'm still a big fan of having a third dimension to navigation aka tabs where the top level sections go but that's an unrelated discussion.

Agree, it just makes it a lot harder to incrementally add more sections because it's a weird experience to switch to tabs without creating and populating a lot of child pages.

> One thing I noticed is that the footer seems to be a bit annoying when clicking on the "Projects" section because it occupies the entire screen width. I don't know if it's possible but limiting that only to the content region (and not the navigation and table of content) would be a good UX improvement IMO.

I feel the same. In my larger reorg, I actually implemented this in an attempt to show more of the nav

<img width="1465" alt="Screenshot 2024-11-15 at 9 48 19 AM" src="https://github.com/user-attachments/assets/41491efb-37dd-4832-bb9e-bfced73cc470">

However, this requires JS to dynamically move the element by editing the DOM (you can't style it up into the content).

I think we either need to fork our own theme and change it there, or... idk. 

Edit: I had another idea that should solve this in the meantime — https://github.com/astral-sh/uv/pull/9153

---

_Comment by @zanieb on 2024-11-15 15:53_

> We should enable [navigation.path](https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/?h=breadc#navigation-path) (navigation path breadcrumbs) so that it's easy to understand where the user is currently at. It might be problematic because of the "Introduction" page that exists currently but that can possibly be solved.

This seems to have no effect locally for me, which is surprising. I agree it'd be nice to have — I think I tried turning it on previously but was annoyed by the "Introduction" page as you mentioned.

---

_Comment by @dhruvmanila on 2024-11-18 05:33_

> This seems to have no effect locally for me, which is surprising.

Oh interesting. The following diff seems to be showing the breadcrumbs for me locally:
```diff
diff --git a/mkdocs.template.yml b/mkdocs.template.yml
index b68729f48..d8c88a525 100644
--- a/mkdocs.template.yml
+++ b/mkdocs.template.yml
@@ -4,7 +4,7 @@ theme:
   logo: assets/logo-letter.svg
   favicon: assets/favicon.ico
   features:
-    - navigation.collapsible
+    - navigation.path
     - navigation.instant
     - navigation.instant.prefetch
     - navigation.instant.progress
```

But, it does include the "Introduction":
<img width="1715" alt="Screenshot 2024-11-18 at 11 00 26 AM" src="https://github.com/user-attachments/assets/e3efcc30-af02-4732-99c4-155306c2c4fb">

> In my larger reorg, I actually implemented this in an attempt to show more of the nav

This looks good. Is this a mockup or is the "reorg" a code change? I'm curious to look at the diff if it's available.

---

_@dhruvmanila reviewed on 2024-11-18 07:15_

---

_Review comment by @dhruvmanila on `mkdocs.template.yml`:67 on 2024-11-18 07:15_

If I understand correctly, the subsections within these documents has been pulled out into their own pages. Do we need redirects for them as well? Because, URLs like https://docs.astral.sh/uv/concepts/projects/#managing-dependencies won't work now.

---

_@zanieb reviewed on 2024-11-18 15:24_

---

_Review comment by @zanieb on `mkdocs.template.yml`:67 on 2024-11-18 15:24_

I'm not sure if the plugin can do redirects at the anchor level, I can try it though.

---

_@zanieb reviewed on 2024-11-18 20:02_

---

_Review comment by @zanieb on `mkdocs.template.yml`:67 on 2024-11-18 20:02_

Yeah it does not support this

WARNING -  redirects plugin: 'concepts/projects.md#managing-dependencies' is not a valid markdown file!

We can redirect _to_ an anchor but not from an anchor. I think this is fine for now, though of course not ideal.

---

_@zanieb reviewed on 2024-11-18 20:30_

---

_Review comment by @zanieb on `mkdocs.template.yml`:67 on 2024-11-18 20:30_

I added support in https://github.com/astral-sh/uv/pull/9212 though

---

_@dhruvmanila reviewed on 2024-11-19 03:00_

---

_Review comment by @dhruvmanila on `mkdocs.template.yml`:67 on 2024-11-19 03:00_

That looks good

---

_Merged by @zanieb on 2024-11-19 19:52_

---

_Closed by @zanieb on 2024-11-19 19:52_

---

_Branch deleted on 2024-11-19 19:52_

---
