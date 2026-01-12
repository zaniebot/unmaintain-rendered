```yaml
number: 3069
title: "Error: Command Failed"
type: issue
state: closed
author: user3059969594
labels: []
assignees: []
created_at: 2025-06-17T17:08:34Z
updated_at: 2025-06-17T18:51:59Z
url: https://github.com/BurntSushi/ripgrep/issues/3069
synced_at: 2026-01-12T16:13:25Z
```

# Error: Command Failed

---

_@user3059969594_

ripgrep works very well except that often when I search for long words (like email) or sequences of numbers I encounter an error which only tells me that the command failed, however it happens that I put a sequence of different numbers of equal length and there I do not have such an error for long words, here is an screen shot of the error im getting [image](https://github.com/user-attachments/assets/4a41d6ae-ec1e-480b-a129-ee0fc06b65d2)

---

_Closed by @BurntSushi on 2025-06-17 17:57_

---

_Reopened by @BurntSushi on 2025-06-17 17:57_

---

_Comment by @BurntSushi on 2025-06-17 17:58_

[Help me help you.](https://www.youtube.com/watch?v=l1B1_jQnlFk) Don't tell, [_show_](https://en.wikipedia.org/wiki/Minimal_reproducible_example). Here's [how to do it](https://stackoverflow.com/help/minimal-reproducible-example).

---

_Comment by @BurntSushi on 2025-06-17 17:58_

That error doesn't come from ripgrep.

---

_Comment by @user3059969594 on 2025-06-17 18:00_

> That error doesn't come from ripgrep.

Then from what here is my code for ripgrep execution: 
const searchCommand = `rg -i --threads 50 --no-filename '${safeKeyword}' "${dbPath}"`;

---

_Comment by @BurntSushi on 2025-06-17 18:02_

That isn't an MRE. If you don't know how to create an MRE, you might need to ask someone for help doing that. It shouldn't, for example, include any JavaScript code.

---

_Comment by @BurntSushi on 2025-06-17 18:24_

I'm not going to debug your JavaScript code. I specifically said your MRE shouldn't include JavaScript. And even so, that _still_ isn't an MRE, because I have no idea how to take that and turn it into the error you're seeing.

I'm sorry, but I do not have the bandwidth to help folks with your experience level. You'll need to seek help elsewhere.

---

_Closed by @BurntSushi on 2025-06-17 18:24_

---

_Closed by @BurntSushi on 2025-06-17 18:24_

---

_Comment by @user3059969594 on 2025-06-17 18:26_

> I'm not going to debug your JavaScript code. I specifically said your MRE shouldn't include JavaScript. And even so, that _still_ isn't an MRE, because I have no idea how to take that and turn it into the error you're seeing.
> 
> I'm sorry, but I do not have the bandwidth to help folks with your experience level. You'll need to seek help elsewhere.

Oh ok thank you for answering me then

---
