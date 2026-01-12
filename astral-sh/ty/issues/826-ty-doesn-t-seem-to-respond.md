```yaml
number: 826
title: "ty doesn't seem to respond"
type: issue
state: closed
author: uriahf
labels:
  - question
assignees: []
created_at: 2025-07-15T13:36:14Z
updated_at: 2025-07-15T17:15:20Z
url: https://github.com/astral-sh/ty/issues/826
synced_at: 2026-01-12T15:54:24Z
```

# ty doesn't seem to respond

---

_@uriahf_

### Summary

Hi all, I'm really excited about ty but unfortunately ty doesn't respond.

I've tried to run `ty check` but I get nothing.

<img width="535" height="47" alt="Image" src="https://github.com/user-attachments/assets/eba4f976-6d16-4174-a04c-de0658e55855" />

Any ideas?

Thanks!


### Version

0.0.1a12

---

_Comment by @sharkdp on 2025-07-15 14:01_

Thank you for reporting this.

How did you install `ty`?

What happens if you run `ty --version` or `ty check --verbose`?

---

_Label `question` added by @MichaReiser on 2025-07-15 14:02_

---

_Comment by @uriahf on 2025-07-15 14:13_

I tried both

uv add --dev ty
and
uv tool install ty@latest

both
ty --version and ty check --verbose 
do not respond

---

_Comment by @sharkdp on 2025-07-15 14:34_

Can you please check what `ty` corresponds to in your PS session? I'm not familar with PS, but I guess `Get-Command ty` would be equivalent to `which ty` on Unix.

Also, if you installed it with uv, could you try to run it via `uvx ty` to see if that works?

---

_Comment by @uriahf on 2025-07-15 15:44_

I get a weird version 0.0.0.0 output:

<img width="1000" height="148" alt="Image" src="https://github.com/user-attachments/assets/dedd16d3-52f3-4186-b310-04700b94cc72" />

uvx ty isn't working neither unfortunately:

<img width="513" height="40" alt="Image" src="https://github.com/user-attachments/assets/693509bc-596c-4fb6-b240-338f16ffdf9b" />

---

_Comment by @MichaReiser on 2025-07-15 16:13_

Hmm, that's weird. Running `Get-Command` gives me the same output:

```
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Application     ty.exe                                             0.0.0.0    c:\users\micha\.local\bin\ty.exe
```

I assume running any other command with uv works fine. E.g do you see any output when running `uvx ruff check` ?

---

_Comment by @uriahf on 2025-07-15 16:15_

uvx ruff check works well (I run regularly uv run ruff check)

<img width="548" height="70" alt="Image" src="https://github.com/user-attachments/assets/14019220-60ee-4e3e-a740-ffc8cdb1f304" />

---

_Comment by @MichaReiser on 2025-07-15 16:23_

That's so strange. `uvx ty` doesn't really do much. All it does is print the help text and exit. 

Does it work if you run the same command in `cmd.exe`? 

---

_Comment by @uriahf on 2025-07-15 16:42_

Hmmm... I get this message

<img width="726" height="266" alt="Image" src="https://github.com/user-attachments/assets/5e54bd45-1980-4627-bace-d326b1d26f4b" />


---

_Comment by @zanieb on 2025-07-15 16:46_

It sounds like you're missing the mscv runtime, see https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170

---

_Comment by @MichaReiser on 2025-07-15 16:47_

@Gankra aren't we statically linking the MSVC?

---

_Comment by @uriahf on 2025-07-15 16:57_

It works now! 
Thank you so much!

@zanieb  @MichaReiser 

---

_Closed by @uriahf on 2025-07-15 16:57_

---

_Comment by @zanieb on 2025-07-15 17:15_

Do we propagate https://github.com/astral-sh/ruff/blob/638186afbd571c1a618bbcf356dd0f50785370ca/.cargo/config.toml#L5-L10 to the ty build?

---
