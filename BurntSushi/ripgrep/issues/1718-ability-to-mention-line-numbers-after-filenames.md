```yaml
number: 1718
title: Ability to mention line numbers after filenames
type: issue
state: closed
author: ssbarnea
labels:
  - wontfix
assignees: []
created_at: 2020-10-29T09:54:36Z
updated_at: 2021-07-02T10:30:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1718
synced_at: 2026-01-12T16:13:24Z
```

# Ability to mention line numbers after filenames

---

_@ssbarnea_

#### Describe your feature request

Most terminals are able to convert filepaths into clickable links that open the files in your desired editor and they also support `filepath:line:column` format.

Sadly ripgrep default output format does not allow use to benefit from line-number clicking on matches as line numbers are separated from the filename.

Example:
```
$ rg "pytest.helpers.run_command"
lib/molecule/test/functional/conftest.py
223:    pytest.helpers.run_command(cmd)
229:    pytest.helpers.run_command(cmd)
232:    pytest.helpers.run_command(cmd)
235:    pytest.helpers.run_command(cmd)
```

# Desired behavior
```
$ rg "pytest.helpers.run_command"
lib/molecule/test/functional/conftest.py:223
223:    pytest.helpers.run_command(cmd)
lib/molecule/test/functional/conftest.py:229
229:    pytest.helpers.run_command(cmd)
lib/molecule/test/functional/conftest.py:232
232:    pytest.helpers.run_command(cmd)
lib/molecule/test/functional/conftest.py:235
235:    pytest.helpers.run_command(cmd)
```

I should mention that I would fine useful enough even if only the first match line number would be included after the filename (ignoring the other ones).




---

_Comment by @BurntSushi on 2020-10-29 10:28_

While the output doesn't quite match what you desire, `--no-heading` should solve your problem.

---

_Closed by @BurntSushi on 2020-10-29 10:28_

---

_Label `wontfix` added by @BurntSushi on 2020-10-29 10:28_

---

_Comment by @ssbarnea on 2020-10-29 10:41_

@BurntSushi Thanks! Yeah, it breaks the ability to see context but in 99% of the cases I used one liner anyway, so not a real issue. Time to update the alias.

PS. Sorry for not using discussions, apparently github did not make it very visible yet.

---

_Comment by @BurntSushi on 2020-10-29 10:46_

What do you mean about it breaking the ability to see context? Not sure I understand.

---

_Comment by @ssbarnea on 2020-10-29 11:26_

Never mind, context still seems to be working fin.

---

_Comment by @ssbarnea on 2020-10-30 10:34_

@BurntSushi There is an issue that breaks this integration: if the match happens to on a line that does not start with space the link would not be clickable because editor will receive some text as column parameter.
```
rg --no-heading colorama 
Dockerfile:27:py3-colorama
# ^ this does not work because a space is needed after the second ":"
````

The specification requires "filepath:line:column" -- the match becomes the column in that case.

Lines that do start with a whitespace character are not effected by this because terminal linkification works ok.



---

_Comment by @BurntSushi on 2020-10-30 12:13_

Use the `--column` flag.

---

_Comment by @ssbarnea on 2020-11-19 11:11_

I recently found a [bug in vscode](https://github.com/microsoft/vscode/issues/110938) which prevents the column from working fine when the line does not start with a space.

While that seems to be a regex issue with vscode, I wonder if there is a trick I can use to make ripgrep add an extra space after the column, so I avoid the bug. In the end, the line after is only informative, so an extra space would not hurt anyone.

---

_Comment by @BurntSushi on 2020-11-19 13:01_

I doubt that's a bug in VS Code. It's probably a bug in your terminal.

> I wonder if there is a trick I can use to make ripgrep add an extra space after the column

Not that I can think of. Sorry.

---

_Comment by @ssbarnea on 2020-11-19 15:42_

Maybe I was not clear: that behavior is from vscode-embedded-terminal, that terminal is part of vscode!

---

_Comment by @BurntSushi on 2020-11-19 15:48_

Ah, gotya.

---

_Comment by @ssbarnea on 2021-07-02 09:16_

I am not sure why I missed to file a bug on vscode, but here it is https://github.com/microsoft/vscode/issues/127762 --  now the question is how to get someone's attention with that bug.

The second question is: can we configure ripgrep to add the space that we need before the matched line?

I seen that it is possible to change the separator but that has no value as I want to obtain something like below, exatra space being essential for avoiding that bug:
```
{path}:{line}:{column}: {match}
```

Many other tools are not affected by this because they decided to not use colon after the column and use a space and a message. Most linters do something like `{path}:{line}:{column} {message}`. Now we can guess why our minor deviance from standard linting output triggered this bug in vscode.

From the aesthetic point of view, I do like avoiding a colon after column number as it makes it easier to read the output.

---

_Comment by @BurntSushi on 2021-07-02 10:30_

No, there is no way. This is the standard grep format, which predates any kind of standard "linting format."

The only way to make this happen on ripgrep's end, as far as I can tell, is to add a new flag. (Because the default is not changing.) And I don't see myself adding a flag for this, especially when it's to work around a bug in done other tool.

---
