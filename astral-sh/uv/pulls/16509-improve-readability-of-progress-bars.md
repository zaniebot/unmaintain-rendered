```yaml
number: 16509
title: Improve readability of progress bars
type: pull_request
state: merged
author: cbrnr
labels:
  - enhancement
assignees: []
merged: true
base: main
head: progressbars
created_at: 2025-10-30T08:46:48Z
updated_at: 2025-11-03T17:13:10Z
url: https://github.com/astral-sh/uv/pull/16509
synced_at: 2026-01-12T16:12:17Z
```

# Improve readability of progress bars

---

_@cbrnr_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Improve readability of progress bars by drawing the right portions in dimmed black instead of dimmed green. This makes it much easier to distinguish from the left (finished) part, which is drawn in green.

Fixes #6908.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Tested locally, looks as follows:

<img width="1100" height="731" alt="white" src="https://github.com/user-attachments/assets/2a396e96-27ef-41ed-8b03-a0de2061af12" />
<img width="1100" height="731" alt="black" src="https://github.com/user-attachments/assets/85d03a3a-a1dc-4389-9e51-fd486e60e067" />

---

_Review requested from @zanieb by @konstin on 2025-10-30 10:33_

---

_Label `enhancement` added by @konstin on 2025-10-30 10:33_

---

_Comment by @zanieb on 2025-10-30 14:04_

Funny, this doesn't change anything with my terminal color scheme :)

---

_@zanieb approved on 2025-10-30 14:04_

---

_Merged by @zanieb on 2025-10-30 14:16_

---

_Closed by @zanieb on 2025-10-30 14:16_

---

_Branch deleted on 2025-10-30 14:27_

---

_Comment by @eliegoudout on 2025-10-31 10:18_

Isn't it more likely to have users with dimm black terminal (Who would have worse readability then) than users with dimm Green terminal?
Just a thought.

---

_Comment by @cbrnr on 2025-10-31 12:33_

Readability is also improved with dark background, see my screenshot. The point is that you see the actual progress, not so much the unfinished bars to the right.

---

_Comment by @eliegoudout on 2025-10-31 13:13_

> Readability is also improved with dark background, see my screenshot.

The dark mode is not universal and many people have different "dark mode" terminal color. My point is that it may be that some users may have the exact color you used as a background color, making them lose readability. Of course, people may also have "dark green" background (in which case, the previous colors would be worse), but my point is that the latter is less likely than the former, isn't it?

My point only stands if the internals define a hardcoded color though, I don't know if it's the case?


---

_Comment by @eliegoudout on 2025-10-31 13:17_

Just so you know, here's the situation locally on my side on a PowrShell (I don't really use that, but people may)

##### Previous version
<img width="584" height="55" alt="image" src="https://github.com/user-attachments/assets/4f69440f-fcc5-40be-8667-566cdec4a988" />

##### Your improved version
<img width="584" height="103" alt="image" src="https://github.com/user-attachments/assets/10bf7137-bfc7-4afa-b26b-2a4a781c034a" />

----

As you can see, previous version was bad, and your version is better, but still shows complete black for incomplete part. In this case, it's still a win, but if the former version was working well (with an actual dark green), then your update would be a regression on my side.

---

_Comment by @cbrnr on 2025-10-31 14:19_

@eliegoudout don't you think this is clearly better? This change is intentional, you can actually see the progress, whereas previously, I can see only one single green bar. I get it that on some terminals with black background, dimmed black might be pure black, but since it is clear where the 100% mark is, you can still see the progress (because the green color wasn't changed at all, it is just better visible now). Or do you have to see the unfinished (remaining) part of the bar (and if so why)?

---

_Comment by @cbrnr on 2025-10-31 14:20_

I think this also heavily depends on the terminal you are using. Which one is it?

---

_Comment by @eliegoudout on 2025-11-03 15:29_

> don't you think this is clearly better?

First of all, yes! On the screens I provided, the new version is clearly better. But I tink this is only because my console does not show the correct colors for the first version (dimmed green), no?

I would have liked to see the rendering for your console before your PR.

My point is simply that previously, since the colors were _green vs dimmed green_, there was almost no risk of a user having the dimmed green badly displayed. However, with the new version (_green vs dimmed **black**_), there is a greater risk or users having a "conflict of colors" with their own console background.

Honestly, I think it's just splitting hairs at this point, but I was simply sharing my (very mild) concern :)

----

> I think this also heavily depends on the terminal you are using. Which one is it?

The default (i think) Windows PowerShell terminal

---

_Comment by @cbrnr on 2025-11-03 15:47_

No worries, I think it is good to discuss any regressions!

> I would have liked to see the rendering for your console before your PR.

I posted it [here](https://github.com/astral-sh/uv/issues/6908#issuecomment-3460651875). This was the intended look, and it was really hard to distinguish the two green colors.

> My point is simply that previously, since the colors were _green vs dimmed green_, there was almost no risk of a user having the dimmed green badly displayed. However, with the new version (_green vs dimmed **black**_), there is a greater risk or users having a "conflict of colors" with their own console background.

True, but even if a dark (dimmed black) background makes the right portion of the bar invisible now, the progress is still clearly visible. So even this special case is an improvement vs. the previous green/dimmed green IMO.

> The default (i think) Windows PowerShell terminal

It looks like special (Unicode?) characters are not working either for you. I recommend Windows Terminal (and not Command Prompt aka cmd.exe) on Windows; this should fix both the colors and the Unicode issues.

---

_Comment by @eliegoudout on 2025-11-03 17:11_

Thanks for your answer! Indeed, the previous renders your showed are pretty bad! And you're right that even if the incomplete part is invisible, it's still better than the terrible rendering from before.

I mean, we could argue for ages about the perfect color(s), but at least, seing the previous rendering, I agree that there is no regression at all.

All the best!


---
