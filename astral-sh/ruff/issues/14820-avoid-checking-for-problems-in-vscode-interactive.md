```yaml
number: 14820
title: Avoid checking for problems in VSCODE interactive
type: issue
state: closed
author: arccarlinbr
labels:
  - bug
  - server
assignees: []
created_at: 2024-12-06T15:31:27Z
updated_at: 2025-06-30T14:14:10Z
url: https://github.com/astral-sh/ruff/issues/14820
synced_at: 2026-01-10T11:09:56Z
```

# Avoid checking for problems in VSCODE interactive

---

_Issue opened by @arccarlinbr on 2024-12-06 15:31_

It looks like this

```
[{
	"resource": "/Interactive-1.interactive",
	"owner": "_generated_diagnostic_collection_name_#3",
	"severity": 8,
	"message": "SyntaxError: Expected `except` or `finally` after `try` block",
	"source": "Ruff",
	"startLineNumber": 1,
	"startColumn": 1,
	"endLineNumber": 1,
	"endColumn": 7
},{
	"resource": "/Interactive-1.interactive",
	"owner": "_generated_diagnostic_collection_name_#3",
	"severity": 8,
	"message": "SyntaxError: Expected ',', found name",
	"source": "Ruff",
	"startLineNumber": 1,
	"startColumn": 27,
	"endLineNumber": 1,
	"endColumn": 45
},{
	"resource": "/Interactive-1.interactive",
	"owner": "_generated_diagnostic_collection_name_#3",
	"severity": 8,
	"message": "SyntaxError: Expected ',', found string",
	"source": "Ruff",
	"startLineNumber": 1,
	"startColumn": 45,
	"endLineNumber": 1,
	"endColumn": 89
},{
	"resource": "/Interactive-1.interactive",
	"owner": "_generated_diagnostic_collection_name_#3",
	"code": {
		"value": "F821",
		"target": {
			"$mid": 1,
			"path": "/ruff/rules/undefined-name",
			"scheme": "https",
			"authority": "docs.astral.sh"
		}
	},
	"severity": 8,
	"message": "Undefined name `con`",
	"source": "Ruff",
	"startLineNumber": 1,
	"startColumn": 1,
	"endLineNumber": 1,
	"endColumn": 4
},{
	"resource": "/Interactive-1.interactive",
	"owner": "_generated_diagnostic_collection_name_#3",
	"code": {
		"value": "F821",
		"target": {
			"$mid": 1,
			"path": "/ruff/rules/undefined-name",
			"scheme": "https",
			"authority": "docs.astral.sh"
		}
	},
	"severity": 8,
	"message": "Undefined name `carteira`",
	"source": "Ruff",
	"startLineNumber": 1,
	"startColumn": 65,
	"endLineNumber": 1,
	"endColumn": 73
},{
	"resource": "/Interactive-1.interactive",
	"owner": "_generated_diagnostic_collection_name_#3",
	"code": {
		"value": "F821",
		"target": {
			"$mid": 1,
			"path": "/ruff/rules/undefined-name",
			"scheme": "https",
			"authority": "docs.astral.sh"
		}
	},
	"severity": 8,
	"message": "Undefined name `safra`",
	"source": "Ruff",
	"startLineNumber": 1,
	"startColumn": 76,
	"endLineNumber": 1,
	"endColumn": 81
},{
	"resource": "/Interactive-1.interactive",
	"owner": "_generated_diagnostic_collection_name_#3",
	"code": {
		"value": "F821",
		"target": {
			"$mid": 1,
			"path": "/ruff/rules/undefined-name",
			"scheme": "https",
			"authority": "docs.astral.sh"
		}
	},
	"severity": 8,
	"message": "Undefined name `con`",
	"source": "Ruff",
	"startLineNumber": 1,
	"startColumn": 1,
	"endLineNumber": 1,
	"endColumn": 4
}]

```


---

_Comment by @MichaReiser on 2024-12-06 16:30_

I'm not sure I understand what you're asking for. Could you share some more details? 

Do you want Ruff to stop checking the `/Interactive-1.interactive` file? What kind of file is it? Is it a notebook? Any chance that you specified the language Python in VS Code?

---

_Label `question` added by @MichaReiser on 2024-12-06 16:30_

---

_Label `needs-mre` added by @MichaReiser on 2024-12-06 16:30_

---

_Comment by @arccarlinbr on 2024-12-06 19:39_

Yes, interactive1 is an interactive notebook on vscode that I run using Run in interactive Window > Run Selection/ Line in interactive Window

---

_Comment by @MichaReiser on 2024-12-07 13:17_

I don't think the extension has a dedicated setting for this. Maybe we should consider adding one (CC: @dhruvmanila). 

For now, you'd have to create ` ruff.toml` and set `extend-exclude = ["*.ipynb"]` as described in the [0.6 announcement blog post](https://astral.sh/blog/ruff-v0.6.0#jupyter-notebooks-are-now-linted-and-formatted-by-default)

---

_Label `needs-mre` removed by @MichaReiser on 2024-12-07 13:18_

---

_Comment by @dhruvmanila on 2024-12-09 08:08_

> It looks like this

Can you tell me where do you get this output from? I don't really see any diagnostics surfaced by Ruff in the interactive window:

![Image](https://github.com/user-attachments/assets/e7473739-f516-43b7-8fa6-aa9b0d6b6751)

Additionally, it seems that Ruff is already ignoring those files?

```
2024-12-09 13:37:14.574 [info] [Trace - 1:37:14 PM]   12.583897833s DEBUG      ruff:main ruff_server::resolve: Ignored path as it's not in the inclusion set: /Interactive-1.interactive
```

> I don't think the extension has a dedicated setting for this. Maybe we should consider adding one (CC: [<img alt="" width="16" height="16" src="https://avatars.githubusercontent.com/u/67177269?s=64&amp;u=3f6384e9bb2b4338388f8729ce4820e080607ebb&amp;v=4">@dhruvmanila](https://github.com/dhruvmanila)).

Are you referring to a setting to explicitly disable linting and formatting Jupyter Notebooks in VS Code ?

> For now, you'd have to create ` ruff.toml` and set `extend-exclude = ["*.ipynb"]` as described in the [0.6 announcement blog post](https://astral.sh/blog/ruff-v0.6.0#jupyter-notebooks-are-now-linted-and-formatted-by-default)

I don't think this will work because the interactive file doesn't have a `.ipynb` extension.


---

_Comment by @arccarlinbr on 2024-12-09 13:12_

It is not a jupyter notebook, is is just an interactive window that `spawns` when you click run in interactive window, I think vscode behind the screens create a temporary python file

---

_Comment by @arccarlinbr on 2024-12-09 14:11_

I am on a company pc so I can't post the pictures from vscode directly but here it is ![image](https://i.imghippo.com/files/LoJ7489To.png)

---

_Comment by @dhruvmanila on 2024-12-09 16:56_

Just to confirm that it's this command (Jupyter: Run current file in interactive window) that you're running, right?

![Image](https://github.com/user-attachments/assets/600b6c42-3d2c-49ac-969c-e83a1d373fbf)

I'll explore a bit more to see if I can get Ruff to pick the file up.

---

_Comment by @arccarlinbr on 2024-12-09 18:59_

I dont think so, I am using F9

![image](https://i.imghippo.com/files/xR8250Fwk.png)

which in ctrl+shift+p looks like

![run_in_interactive](https://i.imghippo.com/files/JR7913TQ.png)

---

_Comment by @dhruvmanila on 2024-12-10 04:35_

Thanks, but the "Run selection/line in interactive window" doesn't give me any warnings either :(

I at least see the diagnostics from Pylance but not from Ruff:

![Image](https://github.com/user-attachments/assets/fcff9c23-1f4c-453b-a0d4-91771d337687)

When I click on the diagnostics from Pylance, it opens up an empty "Interactive-1.interactive.py" file which tells me that they might be "copying" the code into a temporary file and then running it.

I'm still not seeing the diagnostics from "Interactive-1.interactive"


---

_Label `needs-mre` added by @MichaReiser on 2024-12-10 07:21_

---

_Closed by @MichaReiser on 2025-01-25 10:51_

---

_Comment by @Lanikeha on 2025-01-27 22:33_

I am not sure if there is any new information below, but you can run any python file in jupyter interactive windows. This is the setting:

![Image](https://github.com/user-attachments/assets/0c433730-ce83-4045-947e-a3b717d79508)

When I do that, I get tons or errors from code that is copied into execution to the interactive windows. When that happens, Ruff is unusable due to all kind of highlighted "problems" that don't exist in the original .py source file (just in the interactive window).

![Image](https://github.com/user-attachments/assets/780fe291-22f6-44f7-bead-d5cfe8e60a55)

To reproduce:
1. install Jypyter extentions
2. activate "Execute Selection" option as on screenshot above
3. open any .py source file and run it in the interactive window by selecting code you want to run (best to select all code) and hitting Shift + Enter
4. The "Interactive — youfile.py" tab should appear with script output (this one looks like the notebook, has cells with source code and output cells)

---

_Comment by @arccarlinbr on 2025-01-28 12:34_

Exactly, this shouldn't be a closed issue, my vscode had many extensions when I executed my code, the inability to reproduce it was my fault, but the issue highlighted with pylance wasn't the one I mentioned on text, even though it was also on the pictures

---

_Comment by @dhruvmanila on 2025-01-29 10:48_

> To reproduce:
> 
> 1. install Jypyter extentions
> 2. activate "Execute Selection" option as on screenshot above
> 3. open any .py source file and run it in the interactive window by selecting code you want to run (best to select all code) and hitting Shift + Enter
> 4. The "Interactive — youfile.py" tab should appear with script output (this one looks like the notebook, has cells with source code and output cells)

Thanks for providing these steps but unfortunately I'm still not seeing the behavior that you're expecting. Can you provide a screen recording of the same like I've below?

https://github.com/user-attachments/assets/b8e06c14-7902-4922-8d47-73e66621a7ab

---

_Comment by @Lanikeha on 2025-01-29 11:05_

There you go. The difference is that in your example Ruff does not evaluate contents of "Interactive" tab. In my case it does (so every problem is reported twice and does not dissapear from Interactive file even after was corected in .py source file).

I am not able to upload file, hence the link to my google drive:
https://drive.google.com/file/d/1NTtMwWYYibwvpy1vYrkjrff12kO3WxQs/view?usp=sharing

---

_Comment by @dhruvmanila on 2025-01-29 11:10_

Do you have any other VS Code settings that might be relevant here? Which OS are you on?

---

_Comment by @Lanikeha on 2025-01-29 11:26_

> Do you have any other VS Code settings that might be relevant here? Which OS are you on?

I don't know what to identify as unusual, here are my settings:

[settings.json](https://github.com/user-attachments/files/18586660/settings.json)

Windows 11 Pro, 23H2. compile 22631.4317, Windows Feature Experience Pack 1000.22700.1041.0

---

_Comment by @dhruvmanila on 2025-02-11 10:17_

> There you go. The difference is that in your example Ruff does not evaluate contents of "Interactive" tab. In my case it does (so every problem is reported twice and does not dissapear from Interactive file even after was corected in .py source file).

I'm not sure what does "evaluate" mean here, do you mind expanding on that?

I saw your video and that does seem unfortunate that it happens, it could be specific to Windows although not sure without being able to reproduce the issue.

Do you see this issue in other extensions like flake8, pylint, etc.?

---

_Reopened by @MichaReiser on 2025-02-11 10:22_

---

_Comment by @arccarlinbr on 2025-02-12 14:25_

Pylance used to have this same problem, but they appear to have fixed it, maybe the same "fix" could be applied here? https://github.com/microsoft/pylance-release/issues/6617

---

_Comment by @contang0 on 2025-02-28 10:16_

Also interested. It makes no sense at all to lint the interactive window because it is not a part of the project.

---

_Comment by @b-a0 on 2025-03-18 21:44_

I'm also running into this issue (on Windows) and found that I can work around this by running `# ruff: noqa` in the interactive window. Ruff then ignores all of Interactive window contents as described in the docs on [error suppression](https://docs.astral.sh/ruff/linter/#error-suppression).

I tried setting that as one of the items in VS code settings `"jupyter.runStartupCommands"`, but that didn't work unfortunately, so for now I will have to type this in manually once per window to suppress Ruff warnings for interactive windows.

---

_Comment by @dhruvmanila on 2025-03-21 04:27_

Hmm, my guess is that this seems specific to Windows. I'll try to look at the Pylance issue to see if anything obvious jumps out but anyone else is welcomed to try to debug it as well. I don't have a working Windows machine :)

---

_Comment by @rio-raimundo on 2025-05-24 10:26_

Just wanted to say that I also experience this issue, and am also on a Windows PC.

However, it seems like putting the following into a pyproject.toml file removes the linting for the interactive terminal:

```toml
[tool.ruff.lint.per-file-ignores]
"*.interactive" = ["ALL"]
```

Presumably this would work in a ruff.toml too but I haven't tested it. 

However, other strategies like ruff.exclude (in both pyproject.toml and directly in the VSCode settings do not work: 

```toml
[tool.ruff]
exclude = ["*.interactive"]
```

```json
"ruff.exclude": [
     "**/*.interactive"
]
```

Using the 'force-exclude' flag doesn't seem to save these.

Very new to ruff so I was just messing around from the docs and I'm sure there's lots more I haven't tried, but hopefully this will be helpful:) 

I'd still love to see a setting in VSCODE (or just ignore interactive terminals by default) so that I don't have to add this line to every project:) Thanks! 


---

_Comment by @valentinetech on 2025-05-31 15:30_

Same issue using interactive window same issue with vscode, cursor, mac and windows.

Can be reproducted 100% of time clicking `Run Below` when you have .py file put in cell chunks and interactive mode turned on.

Example: 
```
# %%
code

# %%
more code

# %%
more code
```
Getting annoying error: Ruff encountered a problem. Check the logs for more details.

`2025-05-31 18:25:17.406351000  WARN No settings available for untitled:/Interactive-1.interactive - falling back to default setting`

Would be nice option to permanently turn off this error atleast

---

_Comment by @vfmax397 on 2025-06-07 08:06_

Same issue when running code in interactive window, but i found the reason:

when using jupyter interactive, Ruff didn't follow the 【pyproject.toml】setting under your workspace
but the default config file under the 【C:\Users\Administrator\AppData\Roaming\ruff 】 if it exists

so what need to do:
create a file 【C:\Users\Administrator\AppData\Roaming\ruff\pyproject.toml】 and edit:

'''
[tool.ruff]
select = ["FLY"]         # at least 1 rule, shortest rule
ignore = ["FLY002"]   # ignore it
'''

and keep your own projectpy.toml under your workspace for your vscode

---

_Comment by @dhruvmanila on 2025-06-23 11:35_

Can someone who's facing this issue provide the extension and server logs? You can find the instructions here: https://github.com/astral-sh/ruff-vscode#troubleshooting.

Use the following settings:
```json
{
  "ruff.trace.server": "verbose",
  "ruff.logLevel": "debug"
}
```

And, provide the logs messages that are under the "Ruff", "Ruff Language Server", and "Ruff Language Server Trace" output channel (Command Palette > Output: Show Output Channels):

https://github.com/user-attachments/assets/9f6f67f0-ea83-4cc8-ae75-ad695fe82315

As this is specific to Windows, I'm guessing it might be related to https://github.com/astral-sh/ruff/pull/18544 but I can't confirm as I don't have a Windows machine.

---

_Comment by @MichaReiser on 2025-06-23 12:25_

Here the logs from my windows machine. `create.py` is the original python file from where I open the interactive window. 

```
2025-06-23 14:23:57.155846600  INFO main ruff_server::session::index: Registering workspace: c:\Users\Micha\astral\test
2025-06-23 14:23:57.156251500 DEBUG ruff:main notification{method="textDocument/didOpen"}: ruff_server::server::api: enter
2025-06-23 14:23:57.156470800 DEBUG ruff:main notification{method="notebookDocument/didOpen"}: ruff_server::server::api: enter
2025-06-23 14:23:57.156499800 DEBUG ruff:main notification{method="notebookDocument/didOpen"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:57.156510700 DEBUG ruff:main notification{method="notebookDocument/didOpen"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:57.156455700 DEBUG ruff:worker:0 request{id=1 method="textDocument/diagnostic"}: ruff_server::server::api: enter
2025-06-23 14:23:57.156565400 DEBUG ruff:worker:0 request{id=1 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: c:\Users\Micha\astral\test\create.py
2025-06-23 14:23:57.157480100 DEBUG     ruff:main client_response{id=0 method="client/registerCapability"}: ruff_server::session::client: enter
2025-06-23 14:23:57.157505600  INFO     ruff:main client_response{id=0 method="client/registerCapability"}: ruff_server::server::main_loop: Configuration file watcher successfully registered
2025-06-23 14:23:57.157524500 TRACE     ruff:main ruff_server::server::main_loop: method="textDocument/diagnostic" response.id=1 duration=1.16ms
2025-06-23 14:23:57.157548200 DEBUG ruff:worker:1 request{id=2 method="textDocument/codeAction"}: ruff_server::server::api: enter
2025-06-23 14:23:57.157709900 TRACE     ruff:main ruff_server::server::main_loop: method="textDocument/codeAction" response.id=2 duration=276.00µs
2025-06-23 14:23:57.487445300 DEBUG ruff:worker:2 request{id=3 method="textDocument/codeAction"}: ruff_server::server::api: enter
2025-06-23 14:23:57.487638400 TRACE     ruff:main ruff_server::server::main_loop: method="textDocument/codeAction" response.id=3 duration=316.70µs
2025-06-23 14:23:57.573940600 DEBUG ruff:worker:3 request{id=4 method="textDocument/codeAction"}: ruff_server::server::api: enter
2025-06-23 14:23:57.574141900 TRACE     ruff:main ruff_server::server::main_loop: method="textDocument/codeAction" response.id=4 duration=311.80µs
2025-06-23 14:23:58.037476200 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::server::api: enter
2025-06-23 14:23:58.037519400 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.037527500 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.039213800 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::server::api: enter
2025-06-23 14:23:58.039241700 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.039247900 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.050830200 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::server::api: enter
2025-06-23 14:23:58.050858400 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.050867000 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.051612800 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::server::api: enter
2025-06-23 14:23:58.051632200 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.051639300 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.052370600 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::server::api: enter
2025-06-23 14:23:58.052385000 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.052390400 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.061283400 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::server::api: enter
2025-06-23 14:23:58.061310900 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.061317000 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.062200400 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::server::api: enter
2025-06-23 14:23:58.062216700 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:23:58.062222200 DEBUG     ruff:main notification{method="notebookDocument/didChange"}: ruff_server::session::index: Falling back to configuration of the only active workspace for the new document 'untitled:/Interactive-1.interactive'.
2025-06-23 14:24:18.386307500 DEBUG ruff:worker:0 request{id=5 method="textDocument/codeAction"}: ruff_server::server::api: enter
2025-06-23 14:24:18.386416200 TRACE     ruff:main ruff_server::server::main_loop: method="textDocument/codeAction" response.id=5 duration=192.50µs
```

I can see Ruff diagnostics when expanding the code. Although I think seeing diagnostics doesn't feel that wrong to me unless `.interactive` is explicitly excluded (which I haven't in this testing but maybe I should have?). I've to run now. I can to some more testing when I'm back (or tomorrow). 

---

_Comment by @MichaReiser on 2025-06-23 14:07_

IMO, the current behavior does make sense. Especially when opting into the exerimental behavior to use the notebook editor for interactive windows (where you can edit the code).

But I can also understand that the diagnostics can be noisy or undesired and it's not clear why setting `exclude` doesn't work on Windows (I've to debug). However, I do think that adding a toggle option to disable diagnostics in interactive windows is more a Jupyter feature than something we should add to Ruff. I suggest that someone opens an issue upstream.

---

_Comment by @MichaReiser on 2025-06-23 14:19_

The reasons why the exclude don't work on windows is because we use `file_path` here instead of `virtual_path`. 

https://github.com/astral-sh/ruff/blob/10a1d9f01e899201c8d37d29071fe68752d09544/crates/ruff_server/src/lint.rs#L74

but also skip the exclude check entirely if the file has no `file_path`

---

_Comment by @dhruvmanila on 2025-06-23 14:29_

> But I can also understand that the diagnostics can be noisy or undesired and it's not clear why setting `exclude` doesn't work on Windows (I've to debug). However, I do think that adding a toggle option to disable diagnostics in interactive windows is more a Jupyter feature than something we should add to Ruff. I suggest that someone opens an issue upstream.

Thank you!

Yeah, I think that makes sense. There's https://github.com/microsoft/vscode-jupyter/issues/8289 which is already closed but I couldn't find the mentioned `"python.pylanceLspNotebooksEnabled"` option nor any PR that closes that issue. Then, there's this Pylance issue (https://github.com/microsoft/pylance-release/issues/6617) that's open which is related to this.

---

_Comment by @MichaReiser on 2025-06-23 14:34_

Oh right, I forgot about those issues. I feel like implementing this on the LSP side is "hard", unless there's a way for us to know which cells are read-only (and even then, maybe another notebook feature has read-only cells but wants to show diagnostics?).

Either way. I think fixing exclude here would offer a work around to users

---

_Comment by @dhruvmanila on 2025-06-27 03:28_

Hi all, we've released a new Ruff version (`0.12.1`) that fixes the issue where adding the `*.interactive` path to `exclude` option wasn't working on Windows as reported in https://github.com/astral-sh/ruff/issues/14820#issuecomment-2906758898. That should be fixed, it would be great if someone in this thread could test it out!

---

_Label `needs-mre` removed by @dhruvmanila on 2025-06-27 03:28_

---

_Comment by @arccarlinbr on 2025-06-27 22:27_

I checked it, unfortunately Pylance is now once again bugged lol, but Ruff is good to go.


---

_Comment by @arccarlinbr on 2025-06-27 22:28_

Looks fixed to me

---

_Closed by @arccarlinbr on 2025-06-27 22:28_

---

_Label `question` removed by @dhruvmanila on 2025-06-30 14:13_

---

_Label `bug` added by @dhruvmanila on 2025-06-30 14:13_

---

_Label `server` added by @dhruvmanila on 2025-06-30 14:13_

---

_Renamed from "How to avoid checking for problems in VSCODE interactive" to "Avoid checking for problems in VSCODE interactive" by @dhruvmanila on 2025-06-30 14:14_

---
