```yaml
number: 13307
title: Respect XDG specification on Windows
type: issue
state: open
author: leni8ec
labels:
  - enhancement
assignees: []
created_at: 2025-05-06T00:14:14Z
updated_at: 2025-06-21T16:02:50Z
url: https://github.com/astral-sh/uv/issues/13307
synced_at: 2026-01-12T16:01:24Z
```

# Respect XDG specification on Windows

---

_@leni8ec_

### Summary

Now `uv` for some reason ignores XDG variables when installing in **windows** and needs to manually set `uv_*` environment variables.

It would be nice if windows also used  `XDG_*_Home` variables (for example as in [mise](https://github.com/jdx/mise)).


But it is interesting that `XDG_BIN_HOME` - still works in windows, although with an error.

![Image](https://github.com/user-attachments/assets/f7ab0e48-3f8b-4696-bd7f-48cc48096f5a)
![Image](https://github.com/user-attachments/assets/661ddbc9-748f-434f-a234-045d830bcd56)


### Related:
- https://docs.astral.sh/uv/reference/cli/#uv-tool-dir
- https://docs.astral.sh/uv/configuration/installer/#changing-the-install-path
- https://docs.astral.sh/uv/configuration/environment/#xdg_bin_home
- https://github.com/astral-sh/uv/issues/9985
- https://github.com/astral-sh/uv/issues/7008
- https://github.com/astral-sh/uv/issues/5746

> `XDG_DATA_HOME`, `XDG_CONFIG_HOME`, `XDG_CACHE_HOME`


### Example

`uv` uses the [XDG specification](https://specifications.freedesktop.org/basedir-spec/latest/) just as in macos/linux

---

_Label `enhancement` added by @leni8ec on 2025-05-06 00:14_

---

_Comment by @konstin on 2025-05-06 08:38_

We probably shouldn't read `XDG_BIN_HOME` on Windows for consistency, CC @zanieb 

https://github.com/astral-sh/uv/blob/797f1fbac0d5cc6820709d4703bbf3cd1569176c/crates/uv-dirs/src/lib.rs#L28-L33

I think it's more likely that we keep with the Windows native paths over using XDG, but either way we decide I think we should do it consistently.

---

_Comment by @zanieb on 2025-05-06 14:54_

There is no equivalent tool binary directory on Windows so we _are_ using the XDG location as done in `pipx`. I think `pipx` just hard-codes `%USERPROFILE%\.local\bin` though, but I'd prefer to consistently respect `XDG_BIN_HOME` — I think it'd be more confusing not to.

---

_Comment by @zanieb on 2025-05-06 14:55_

If the XDG_* variables are set and map clearly to the Windows native path concepts, I don't really see a compelling reason to ignore them on Windows really. Though, of course, we should use the native paths by default.

---

_Comment by @konstin on 2025-05-06 15:04_

Makes sense!

---

_Comment by @Gankra on 2025-05-17 09:29_

I agree that it would probably be good to just respect XDG if it *happens* to be set on windows -- it's pretty hard for those to be accidentally set!

---

_Comment by @danielniccoli on 2025-06-21 09:39_

> There is no equivalent tool binary directory on Windows

Yes, there is: `FOLDERID_UserProgramFiles`. I also wrote about that [here](https://github.com/astral-sh/uv/issues/7008#issuecomment-2721538187) and mentioned you.

Source: https://learn.microsoft.com/en-us/windows/win32/shell/knownfolderid

Furthermore, for anyone's interest:

> Applications should not create files or folders [in the user's profile folder]; they should put their data under the locations referred to by [ApplicationData](https://learn.microsoft.com/en-us/dotnet/api/system.environment.specialfolder?view=net-6.0#system-environment-specialfolder-applicationdata).

Source: https://learn.microsoft.com/en-us/dotnet/api/system.environment.specialfolder?view=net-6.0

---

_Comment by @zanieb on 2025-06-21 11:31_

> Yes, there is: FOLDERID_UserProgramFiles

Yes, I've seen your comment there and appreciate your suggestions. However, that's not a single directory shared across programs and expected to be present on the `PATH`. Rather the suggestion is to do something like `%LOCALAPPDATA%\Programs\uv\scripts` and add it to the `PATH`. We could do that, but it's not a direct equivalent to the `XDG_BIN_HOME`.

---

_Comment by @danielniccoli on 2025-06-21 16:02_

> However, that's not a single directory shared across programs and expected to be present on the PATH.

Yes, you are right that there is no directory equivalent to `~/.local/bin` on Windows. Reading the comment again, I think I did mistunderstand your intention a bit. 

---
