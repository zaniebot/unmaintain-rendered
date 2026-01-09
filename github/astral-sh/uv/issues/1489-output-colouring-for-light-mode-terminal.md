---
number: 1489
title: output colouring for light mode terminal
type: issue
state: closed
author: detlefla
labels:
  - enhancement
assignees: []
created_at: 2024-02-16T13:16:41Z
updated_at: 2024-02-17T07:59:21Z
url: https://github.com/astral-sh/uv/issues/1489
synced_at: 2026-01-07T13:12:16-06:00
---

# output colouring for light mode terminal

---

_Issue opened by @detlefla on 2024-02-16 13:16_

In the `uv pip sync` coloured output, package names are in a white font. In a black-on-white terminal they are invisible, and the output looks kind of surprising.

The `--color never` option would solve this; it would be helpful, though, if there was a light-mode colouring option.

The screenshot shows just a few lines of the delta listing:
![Screenshot_20240216_145013](https://github.com/astral-sh/uv/assets/13684630/f623b7b8-93f4-45d4-8e56-02b80076f839)


---

_Label `enhancement` added by @MichaReiser on 2024-02-16 13:24_

---

_Comment by @MichaReiser on 2024-02-16 13:25_

Thanks for reporting. Do you have a screenshot that you could atttach to the issue (You can CTRL+V or drag and drop the file into the description and it will upload it)

---

_Comment by @BurntSushi on 2024-02-16 13:29_

FWIW, I also use a black-on-white terminal and this is what I see:

![uv-pip-sync](https://github.com/astral-sh/uv/assets/456674/480e09ed-f966-4079-9333-4553e0510607)

I wonder if perhaps there is something amiss with your terminal color scheme? With that said, the grey color we use is perhaps not ideal.

---

_Comment by @detlefla on 2024-02-16 13:54_

I pasted the screenshot into the original comment, hope this is OK.

---

_Comment by @detlefla on 2024-02-16 13:56_

Forgot to mention that the terminal is a KDE konsole with its standard (light) colour scheme.

---

_Comment by @MichaReiser on 2024-02-16 14:13_

> I pasted the screenshot into the original comment, hope this is OK.

Thanks... Yeah that's uhm, not ideal

---

_Comment by @zanieb on 2024-02-16 14:34_

I was complaining that we were using `white` for these too — it's a problem we should choose a different light color.

---

_Comment by @zanieb on 2024-02-16 14:35_

In my GitHub Light High Contrast theme, okay but could be better

<img width="283" alt="Screenshot 2024-02-16 at 08 35 00" src="https://github.com/astral-sh/uv/assets/2586601/df165810-5044-44bd-84e0-7233f8a273d6">


---

_Comment by @henryiii on 2024-02-17 05:39_

Same on iterm. 
![Screenshot 2024-02-17 at 12 13 02 AM](https://github.com/astral-sh/uv/assets/4616906/f4d3cdc1-1923-44de-b40b-d2d2a075f18a)

You don't need to set white or black, the default (`\033[39;m`) is always the default forground color. For a black terminal, that will be white, and a white terminal, that's black, so it's always better than setting black or white. You can combine that with bold (`\033[39;1;m`), etc.

---

_Comment by @zanieb on 2024-02-17 05:43_

With just `bold()` https://github.com/astral-sh/uv/pull/1576

<img width="363" alt="Screenshot 2024-02-16 at 23 43 28" src="https://github.com/astral-sh/uv/assets/2586601/fb75d75f-8e21-4a25-bece-9e15c30bbf85">


---

_Referenced in [astral-sh/uv#1576](../../astral-sh/uv/pulls/1576.md) on 2024-02-17 05:44_

---

_Closed by @zanieb on 2024-02-17 07:59_

---
