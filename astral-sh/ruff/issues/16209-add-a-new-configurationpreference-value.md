```yaml
number: 16209
title: "Add a new `configurationPreference` value `filesystemOnly`"
type: issue
state: open
author: dhruvmanila
labels:
  - needs-decision
  - server
assignees: []
created_at: 2025-02-17T11:01:46Z
updated_at: 2025-04-04T14:43:17Z
url: https://github.com/astral-sh/ruff/issues/16209
synced_at: 2026-01-10T11:09:57Z
```

# Add a new `configurationPreference` value `filesystemOnly`

---

_Issue opened by @dhruvmanila on 2025-02-17 11:01_

### Description

> Thanks for implementing the [ruff.configurationPreference](https://docs.astral.sh/ruff/editors/settings/#configurationpreference) setting. I wanted to raise one behavior that I found confusing:
> 
> If some ruff settings are not set in the `.toml` file, the extension will use preferences from the editor, not the default values. I assume this is what "takes priority" mean?
> 
> This results in undesirable behaviors, including inconsistencies in linting and formatting. For example, if line length is not defined in the `.toml` file but defined in `settings.json`, running `ruff format` in the terminal will apply the default of 88 while vscode's format will apply the `settings.json` length.
> 
> Maybe there is an easy workaround I missed? If not, would it be possible to have a "filesystemOnly" option that would use the editor's preferences only when no `.toml` is present? Alternatively, could we have "hard" vs "soft" priorities for both "editorFirst" and "filesystemFirst"?
> 
> Thanks in advance 

 _Originally posted by @VictorGillioz in [#425](https://github.com/astral-sh/ruff-vscode/issues/425#issuecomment-2624785894)_

---

_Label `server` added by @dhruvmanila on 2025-02-17 11:01_

---

_Comment by @dhruvmanila on 2025-02-17 11:04_

We'd need to be careful around versioning as this would be added in Ruff and the VS Code extension but if there's a mismatch between the version the extension shouldn't error.

---

_Comment by @T-256 on 2025-02-19 19:47_

> This results in undesirable behaviors, including inconsistencies in linting and formatting. For example, if line length is not defined in the `.toml` file but defined in `settings.json`, running `ruff format` in the terminal will apply the default of 88 while vscode's format will apply the `settings.json` length.
> 
> Maybe there is an easy workaround I missed?

I think it is editor-related feature and already could be handled by VSCode: reset modified settings per workspace/folder.

Also, IMO, [ruff.configurationPreference](https://docs.astral.sh/ruff/editors/settings/#configurationpreference) setting could have only two varients: `editor` and `filesystem`. Use separated settings `fileystemDiscover` with default of `true` and set to `false` for apply `editorOnly` behavior.

Since the _preference_ word points to selecting one among others, I think it makes more sense to NOT having `First` suffix in its varients.


---

_Comment by @dhruvmanila on 2025-02-20 05:57_

> I think it is editor-related feature and already could be handled by VSCode: reset modified settings per workspace/folder.

Can you say more about this? What's "VSCode: reset modified settings"?

I think the user still wants to keep the settings but not use it, it does seem counterintuitive to still keep them.

---

_Label `needs-decision` added by @dhruvmanila on 2025-02-20 05:59_

---

_Label `needs-decision` removed by @dhruvmanila on 2025-02-20 05:59_

---

_Comment by @MichaReiser on 2025-02-20 07:56_

Reading the issue description. I don't think `filesystemOnly` would do what I'd expect. I'd expect that `filesystemOnly` would disregard the editor settings completely even if no configuration exists for the current project. But that's not what's proposed. The proposed behavior is to use the file system configuration if it exists, then fall back to the editor one (but never merge the configurations). 

I do agree with @T-256 that I don't think a `filesystemOnly` as I understood makes sense. In that case, users should unset their editor settings. 

Maybe it's just a naming issue, but I get the impression that `configuration precedence` is trying to do too much. 

1. It specifies in which precedence options from the discovered configurations should be merged
2. It configures which configurations should be discovered/respected

That makes me wonder if we should have a dedicated settings instead.

A related option -- that I'm surprised it has never come up before -- is whether Ruff should run on projects that don't have a project configuration. I don't think we have to solve this here, but maybe something to keep in mind.

---

_Comment by @InSyncWithFoo on 2025-02-20 10:18_

@MichaReiser Currently `editorOnly` means "discard any configurations found in `.toml` files". `filesystemOnly`, if added, must work similarly to preserve consistency. These two have quite unfortunate names, sure, but the ship has sailed.

Instead, a fifth option with the meaning "use `.toml` files if found, fall back to editor otherwise" should be added (tentatively named `filesystemFallingBackToEditor`).

---

_Comment by @MichaReiser on 2025-02-20 10:45_

> These two have quite unfortunate names, sure, but the ship has sailed.

I still see an opportunity to error correct. We have to support `editorOnly` for the near future. 

Either way, I don't think a `filesystemOnly` setting makes sense. Users should unset their editor settings in that case.

---

_Label `needs-decision` added by @dhruvmanila on 2025-02-20 10:47_

---

_Comment by @dhruvmanila on 2025-02-20 10:48_

(I'm going to mark this as "needs-decision" as after talking with Micha I agree that we can improve here.)

---

_Comment by @T-256 on 2025-02-20 21:44_

> I think the user still wants to keep the settings but not use it, it does seem counterintuitive to still keep them.

You can keep them as _User_ settings and overriding them to default values in _Workspace_ settings.
Your workflow should become like these:
* _User Settings_:
  ```jsonc
  {
    "ruff.configurationPreference": "filesystemFirst",
    "ruff.lint.extendSelect": [
        "RUF013",  // implicit-optional (RUF013)
    ],
  }
  ```
* _Workspace Settings_ - Where you want apply configs from FileSystem only and all editor settings to be ignored:
  ```jsonc
  {
    // seetings in this file would be merge to `User Settings`
    "ruff.lint.extendSelect": [],  // reset modified settings of `User Settings`,
                                   // So, now it seems there is no configuration set on editor side for ruff server.
  }
  ```
Then only filesystem configs are respected.

---

_Comment by @dhruvmanila on 2025-03-03 07:09_

> You can keep them as _User_ settings and overriding them to default values in _Workspace_ settings.

That's a good suggestion, thanks. @VictorGillioz does this help your use-case?

---

_Comment by @VictorGillioz on 2025-04-04 14:39_

Hi @dhruvmanila, overriding all my ruff User settings in each Workspace where I define a .toml file should work, but kind of goes against the benefits of having ruff User settings in the first place imo (e.g. if I add a new User setting, I then have to go through all my workspaces to override it). In that case I might prefer not having default settings at all and creating a .toml file each time instead.

I also think it makes the overriding even more tricky between User settings, Workspace settings, and filesystem.

Having the option of defaults settings but ignoring them when a .toml is present would really be valuable in my opinion, but I'll work around it if it's too much overhead. And thanks for the suggestions!

---
