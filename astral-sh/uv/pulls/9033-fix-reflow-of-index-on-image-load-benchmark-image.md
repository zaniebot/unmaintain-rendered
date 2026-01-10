```yaml
number: 9033
title: "Fix reflow of index on image load: benchmark image - #6264"
type: pull_request
state: open
author: jvestell
labels: []
assignees: []
base: main
head: Jerrod_6264
created_at: 2024-11-11T23:19:51Z
updated_at: 2025-09-26T07:27:41Z
url: https://github.com/astral-sh/uv/pull/9033
synced_at: 2026-01-10T06:36:15Z
```

# Fix reflow of index on image load: benchmark image - #6264

---

_Pull request opened by @jvestell on 2024-11-11 23:19_

This PR sets a fixed height and width for the benchmark image and description text underneath it. 

The purpose of the change is to prevent page reflow and layout shifts on the docs page.

Both light mode and dark mode were tested. The benchmark image is slightly larger with the change. I thought this increase in size helps draw attention to the bolded uv result shown in the benchmark diagram. I'll draw the size back down to the original if reviewers don't like it. 

closes #6264 

![aaaaaScreenshot 2024-11-11 151909](https://github.com/user-attachments/assets/506c2aa4-5c6f-489a-b831-8ad3d13a96bd)




---

_@zanieb reviewed on 2024-11-11 23:33_

---

_Review comment by @zanieb on `docs/stylesheets/extra.css`:92 on 2024-11-11 23:33_

Shouldn't these characteristics be set just for this specific image? Is there a reason you decided to implement it in the general css?

---

_Comment by @zanieb on 2024-11-11 23:33_

Welcome to the project!

---

_@jvestell reviewed on 2024-11-11 23:58_

---

_Review comment by @jvestell on `docs/stylesheets/extra.css`:92 on 2024-11-11 23:58_

That's a good suggestion. Initially I wanted to leave the index file unmodified, but I like your suggestion. Here it is.

---

_Comment by @jvestell on 2024-11-12 00:00_

Thanks for the hospitality! 

---

_Comment by @jvestell on 2024-11-14 01:06_

@zanieb Friendly reminder: new commit ready for review :)

---

_Comment by @Arkenstone999 on 2025-09-25 14:05_

hi, is this open pr satisfies the requirements from astral ? If not may i work on it ?  Regards


---

_@MichaReiser reviewed on 2025-09-26 07:27_

---

_Review comment by @MichaReiser on `docs/stylesheets/extra.css`:92 on 2025-09-26 07:27_

I agree, making this an inline style seems easier. It should also be sufficient to set `height: 107px` (using the HTML attribute doesn't work for reasons?). All the other style changes weren't necessary when I tested this in Firefox.

107 is the height from the SVG image. Using the exact height removes the need for setting width.

---
