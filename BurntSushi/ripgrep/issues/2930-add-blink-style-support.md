```yaml
number: 2930
title: "add `blink` style support"
type: issue
state: open
author: ClaireCJS
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-11-11T17:13:39Z
updated_at: 2024-11-11T17:58:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2930
synced_at: 2026-01-12T16:13:25Z
```

# add `blink` style support

---

_@ClaireCJS_

#### Describe your feature request

Enact GREP_COLOR variable configuration


Please describe the behavior you want and the motivation. Please also provide
examples of how ripgrep would be used if your feature request were added.

I currently have both these variables set:
```
GREP_COLOR=mt=42;5;185
GREP_COLORS=mt=42;5;185
```

It causes my matched grep text to be blinking white on green.... A deliberately annoying choice made to grab my attention. Not sure which one of the 2 is the one "doing" it; it depends on which year of grep release you use. Both is safest for me. And sometimes it requires --color=always to make it happen.   My grep is basically aliased to --color=always -i 

Been enjoying this for years if not decades, but today hit a wall with file encoding that just won't grep a simple word out of a simple log file unless i type it and pipe it to grep, which I don't like.  

So I discovered ripgrep.

It cuts through the file encoding and gives me my results just fine, but obviously not in the ridiculous color I want. I love its speed and simplicity, but I also love the colors I have been using for years/decades.


Here's a visual example of what I'm getting at:

![image](https://github.com/user-attachments/assets/ae0b184e-c547-42cd-859a-e5c0f2b8803f)



If you're not sure what to write here, then try imagining what the ideal
documentation of your new feature would look like in ripgrep's man page. Then
try to write it.

Probably just a copy of the gnu doc for the --color feature




---

_Comment by @BurntSushi on 2024-11-11 17:20_

No, definitely not. `GREP_COLOR` and `GREP_COLORS` is for grep, not for ripgrep. It's IMO not a good idea to be respecting other tool's environment variables.

Moreover, ripgrep very intentionally doesn't expose a way to set arbitrary ANSI escape codes. Instead, you specify the styling you want and ripgrep translates that to ANSI escapes.

I guess I'd be open to adding a "blink" style... Then you could still get the styling you want at least.

---

_Comment by @ClaireCJS on 2024-11-11 17:50_

> No, definitely not. `GREP_COLOR` and `GREP_COLORS` is for grep, not for ripgrep. It's IMO not a good idea to be respecting other tool's environment variables.
> 
> Moreover, ripgrep very intentionally doesn't expose a way to set arbitrary ANSI escape codes. Instead, you specify the styling you want and ripgrep translates that to ANSI escapes.
> 
> I guess I'd be open to adding a "blink" style... Then you could still get the styling you want at least.

That would indeed be nice. All the visual ANSI codes are implemented really well in Windows Terminal.  Italics, underline, double-underline would be great.

but blink would be greatest! :) Thanks!!

![image](https://github.com/user-attachments/assets/3f20ede1-9d61-484a-919e-5605c3635e5e)


---

_Comment by @BurntSushi on 2024-11-11 17:58_

Italics and underline are already supported by ripgrep.

I probably won't work on this myself, but PRs are welcome.

---

_Renamed from "Can we respect the GREP_COLOR and GREP_COLORS environment variables" to "add `blink` style support" by @BurntSushi on 2024-11-11 17:58_

---

_Label `enhancement` added by @BurntSushi on 2024-11-11 17:58_

---

_Label `help wanted` added by @BurntSushi on 2024-11-11 17:58_

---
