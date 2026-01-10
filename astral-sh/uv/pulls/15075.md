```yaml
number: 15075
title: Update badge logo SVG
type: pull_request
state: open
author: Onyx-Nostalgia
labels: []
assignees: []
base: main
head: fix/logo-badge
created_at: 2025-08-05T05:39:33Z
updated_at: 2025-08-20T12:31:06Z
url: https://github.com/astral-sh/uv/pull/15075
synced_at: 2026-01-10T06:44:33Z
```

# Update badge logo SVG

---

_Pull request opened by @Onyx-Nostalgia on 2025-08-05 05:39_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
I am using UV and would like to include a UV badge in my README. I found that the current logo does not match the one in your official docs (https://docs.astral.sh/uv/).

I have replaced the SVG logo you are using ([/docs/assets/logo-letter.svg](https://github.com/astral-sh/uv/blob/main/docs/assets/logo-letter.svg)).

This type of replacement will allow many users who call your official badge to not have to change anything.

## Test Plan

**Before**
- Normal
```
[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)
```
[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)
- With Style (e.g. `&style=for-the-badge`)
```
[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json&style=for-the-badge)](https://github.com/astral-sh/uv)
```
[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json&style=for-the-badge)](https://github.com/astral-sh/uv)

**After**
- Normal
```
[![uv](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2FOnyx-Nostalgia%2Fuv%2Frefs%2Fheads%2Ffix%2Flogo-badge%2Fassets%2Fbadge%2Fv0.json)](https://github.com/astral-sh/uv)
```
[![uv](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2FOnyx-Nostalgia%2Fuv%2Frefs%2Fheads%2Ffix%2Flogo-badge%2Fassets%2Fbadge%2Fv0.json)](https://github.com/astral-sh/uv)
- With Style (e.g. `&style=for-the-badge`)
```
[![uv](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2FOnyx-Nostalgia%2Fuv%2Frefs%2Fheads%2Ffix%2Flogo-badge%2Fassets%2Fbadge%2Fv0.json&style=for-the-badge)](https://github.com/astral-sh/uv)
```
[![uv](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2FOnyx-Nostalgia%2Fuv%2Frefs%2Fheads%2Ffix%2Flogo-badge%2Fassets%2Fbadge%2Fv0.json&style=for-the-badge)](https://github.com/astral-sh/uv)

Note: I use a URL from the repo I forked from you (https://github.com/Onyx-Nostalgia/uv/blob/fix/logo-badge/assets/badge/v0.json) to show you an example of the result you will get if you merge this PR. And of course, after the merge, you will also see the changes in the badge you use in your own README.

---

_Comment by @zanieb on 2025-08-05 19:35_

I don't think we want to change this to the letter logo, e.g., we're not using the "R" for the Ruff badge. I don't think we have an appropriate alternate svg here. Except maybe the lightning bolt in uv's color?

---

_Comment by @Onyx-Nostalgia on 2025-08-06 08:23_

> I don't think we have an appropriate alternate svg here

😅 I apologize for the confusion. I initially assumed that the logo-letter I saw in your web documents was the current logo that you didn't want to change. I also mistakenly thought you might have just forgotten to update the logo on the badge, so I changed it to maintain consistency.

> Except maybe the lightning bolt in uv's color?

If you do decide to change the UV logo in the future, may I offer a personal opinion? I believe that having distinct logos for each of your products could be beneficial. It would help highlight the unique identity of each product and prevent confusion with your other tools.

Just to clarify and perhaps help me provide better feedback on the logo: is the name "UV" something you intend to keep permanently? And is there a story or a reason behind the name (e.g., an acronym, a reference to UV light, etc.)?

---

_Comment by @my1e5 on 2025-08-20 12:31_

> I don't think we want to change this to the letter logo, e.g., we're not using the "R" for the Ruff badge. 

@zanieb I've noticed that ty is using "T" for its badge

<img width="352" height="121" alt="image" src="https://github.com/user-attachments/assets/4eefdca3-8d49-4e65-a9a4-b6e94f21d078" />

Relevant PR: 
- https://github.com/astral-sh/ty/pull/897

---
