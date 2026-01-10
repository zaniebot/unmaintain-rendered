---
number: 3918
title: powershell support for native completions 
type: issue
state: open
author: epage
labels:
  - A-completion
  - E-help-wanted
  - E-easy
  - ":money_with_wings: $20"
assignees: []
created_at: 2022-07-13T14:16:57Z
updated_at: 2025-03-28T14:05:30Z
url: https://github.com/clap-rs/clap/issues/3918
synced_at: 2026-01-10T01:27:49Z
---

# powershell support for native completions 

---

_Issue opened by @epage on 2022-07-13 14:16_

See #3166 for more context

- [code](https://github.com/clap-rs/clap/blob/master/clap_complete/src/env/shells.rs)
- [tests](https://github.com/clap-rs/clap/blob/master/clap_complete/tests/testsuite/powershell.rs)
- [argcomplete](https://pypi.org/project/argcomplete/) and [cobra](https://github.com/spf13/cobra) can serve as examples

Tasks
- [x] #5646
- [ ] Identify and resolve gaps with static completions

---

_Label `A-completion` added by @epage on 2022-07-13 14:16_

---

_Label `E-easy` added by @epage on 2022-07-13 14:16_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2022-07-13 14:20_

---

_Label `:money_with_wings: $20` added by @epage on 2022-09-13 14:35_

---

_Comment by @Rhondapetal on 2022-09-14 12:06_

Hello 

---

_Label `E-help-wanted` added by @epage on 2022-09-20 14:29_

---

_Comment by @epage on 2023-05-24 16:05_

For Powershell
```Powershell
Register-ArgumentCompleter
        -CommandName <String[]>
        -ScriptBlock <ScriptBlock>
        [-Native]
        [<CommonParameters>]
```
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/register-argumentcompleter?view=powershell-7.2

The block receives
> When you specify the Native parameter, the script block must take the following parameters in the specified order. The names of the parameters aren't important because PowerShell passes in the values by position.
>
> - $wordToComplete (Position 0) - This parameter is set to value the user has provided before they pressed Tab. Your script block should use this value to determine tab completion values.
> - $commandAst (Position 1) - This parameter is set to the Abstract Syntax Tree (AST) for the current input line. For more information, see [Ast Class](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.language.ast).
> - $cursorPosition (Position 2) - This parameter is set to the position of the cursor when the user pressed Tab.

The block provides [CompletionResult](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.completionresult?view=powershellsdk-7.0.0)

> The CompletionResult object allows you to provide additional details to each returned value:
>
> - completionText (String) - The text to be used as the auto completion result. This is the value sent to the command.
> - listItemText (String) - The text to be displayed in a list, such as when the user presses Ctrl+Space. This is used for display only and is not passed to the command when selected.
> - resultType ([CompletionResultType](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.completionresulttype)) - The type of completion result.
> - toolTip (String) - The text for the tooltip with details to be displayed about the object. This is visible when the user selects an item after pressing Ctrl+Space.

So it seems like Powershell can fit within rust-driven completions and provide the full feature set.

---

_Referenced in [clap-rs/clap#5338](../../clap-rs/clap/pulls/5338.md) on 2024-02-03 09:21_

---

_Referenced in [clap-rs/clap#5564](../../clap-rs/clap/pulls/5564.md) on 2024-07-04 08:59_

---
