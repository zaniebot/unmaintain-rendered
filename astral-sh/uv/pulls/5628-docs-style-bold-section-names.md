```yaml
number: 5628
title: "docs(style): Bold section names"
type: pull_request
state: closed
author: eth3lbert
labels:
  - documentation
  - preview
assignees: []
base: main
head: doc-bold-section-names
created_at: 2024-07-30T18:55:12Z
updated_at: 2024-08-19T23:53:20Z
url: https://github.com/astral-sh/uv/pull/5628
synced_at: 2026-01-10T13:09:50Z
```

# docs(style): Bold section names

---

_Pull request opened by @eth3lbert on 2024-07-30 18:55_

## Summary

Closes #5614 .

## Test Plan

Run `mkdocs serve -f mkdocs.public.yml` to see the changes.

---

_Comment by @eth3lbert on 2024-07-30 18:57_

Some screenshots:

<img width="262" alt="image" src="https://github.com/user-attachments/assets/8010075a-e211-4236-b304-a0347b540966">
<img width="287" alt="image" src="https://github.com/user-attachments/assets/b926ab9a-3334-43f2-89a8-64387ef8eb8c">

Noted: The RHS is only used to demonstrate that the area is being modified.


---

_Comment by @zanieb on 2024-07-30 19:04_

Nice! Thank you. Should we make the color match the other bolded headings? e.g.

```diff
diff --git a/docs/stylesheets/extra.css b/docs/stylesheets/extra.css
index 82b4ca86..9b3a14e0 100644
--- a/docs/stylesheets/extra.css
+++ b/docs/stylesheets/extra.css
@@ -131,4 +131,5 @@ See: https://github.com/astral-sh/uv/issues/5614 */
 .md-nav__link[for^="__nav_"] .md-ellipsis,
 .md-nav__container .md-ellipsis {
   font-weight: bold;
+  color: var(--md-default-fg-color--light);
 }

---

_Label `documentation` added by @zanieb on 2024-07-30 19:05_

---

_Label `preview` added by @zanieb on 2024-07-30 19:05_

---

_Comment by @eth3lbert on 2024-07-30 19:13_

> Should we make the color match the other bolded headings?

Hmm, this will make the active one no different from others. Maybe it's not a good idea. :sweat_smile:


---

_Comment by @zanieb on 2024-07-30 19:41_

Presumably we could use `.md-nav__link--active` to retain the active color change, right?

Alternatively we could explore a less heavy bold font-weight but the default family (at least on my platform) doesn't support it.

---

_Comment by @eth3lbert on 2024-07-30 19:41_

Also, if you think it's too bold, you could change the value to something(500, 600, ...) that larger than 400 (normal). Currently, it's 700 (bold).

---

_Comment by @zanieb on 2024-07-30 19:48_

There's also 

```diff
diff --git a/mkdocs.template.yml b/mkdocs.template.yml
index 26d01d0d..14c01bcf 100644
--- a/mkdocs.template.yml
+++ b/mkdocs.template.yml
@@ -8,6 +8,7 @@ theme:
     - navigation.instant.prefetch
     - navigation.instant.progress
     - navigation.expand
+    - navigation.sections
     - navigation.indexes
     - navigation.tracking
     - content.code.annotate
```

I'm not sure if that's better or not?

---

_Comment by @eth3lbert on 2024-07-30 19:56_

> ```diff
> - navigation.sections
> ```

<img width="210" alt="image" src="https://github.com/user-attachments/assets/d5b5fa0e-5ef0-460e-8be7-0eb6c86d3327">

This lack of indentation.

And style preference that varies from person to person. I suppose we could let others choose.

---

_Comment by @zanieb on 2024-07-30 20:05_

Yeah I'm not sure what's best. @charliermarsh and @dhruvmanila curious for your thoughts.

---

_Comment by @dhruvmanila on 2024-07-31 05:33_

I think `navigation.sections` looks good as we're already using `navigation.expand`. So, `navigation.sections` would provide both the bold section names _and_ expanded navigation. It does have a limitation that one cannot close any specific section but I doubt that readers would close all sections except for the one they're focused in. I do like the indentation but couldn't find any reference for it in the mkdocs-material documentation.

tldr, my opinion is to replace `navigation.expand` with `navigation.sections`.

---

_Comment by @eth3lbert on 2024-07-31 05:47_

> I do like the indentation but couldn't find any reference for it in the mkdocs-material documentation.

I think this can be achieved by the css.

---

_Comment by @zanieb on 2024-08-19 22:48_

Superseded by #5674 

---

_Closed by @zanieb on 2024-08-19 22:48_

---

_Comment by @zanieb on 2024-08-19 22:48_

Thanks for exploring this though

---

_Branch deleted on 2024-08-19 23:53_

---
