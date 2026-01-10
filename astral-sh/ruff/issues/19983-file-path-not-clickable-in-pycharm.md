---
number: 19983
title: File path not clickable in PyCharm
type: issue
state: open
author: CorneiZeR
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-08-19T07:44:15Z
updated_at: 2025-12-29T15:56:35Z
url: https://github.com/astral-sh/ruff/issues/19983
synced_at: 2026-01-10T01:23:01Z
---

# File path not clickable in PyCharm

---

_Issue opened by @CorneiZeR on 2025-08-19 07:44_

### Summary

After upgrading from `ruff==0.12.8` to `ruff==0.12.9`, the default output format of ruff check no longer includes the file path and position.

# Steps to reproduce
```bash
uv sync -p ruff==0.12.8
ruff check .
```
# Output
```bash
common/decorators.py:16:9: ANN202 Missing return type annotation for private function `wrapper`
   |
15 |     @wraps(func)
16 |     def wrapper(self: object, request: Request, *args: Any, **kwargs: Any):
   |         ^^^^^^^ ANN202
17 |         try:
18 |             return func(self, request, *args, **kwargs)
   |
   = help: Add return type annotation

Found 1 error.
```

---

# New version
```bash
uv sync -p ruff==0.12.9
ruff check .
```
# Output
```bash
ANN202 Missing return type annotation for private function `wrapper`
  --> common/decorators.py:16:9
   |
15 |     @wraps(func)
16 |     def wrapper(self: object, request: Request, *args: Any, **kwargs: Any):
   |         ^^^^^^^
17 |         try:
18 |             return func(self, request, *args, **kwargs)
   |
help: Add return type annotation

Found 1 error.
```

# Expected behavior
File path and position should be shown in the default output, same as in `0.12.8`.

**common/decorators.py:16:9**: ANN202...

---

# Notes
With `--output-format=concise` the file path is still printed:
```bash
common/decorators.py:16:9: ANN202 Missing return type annotation for private function `wrapper`
```



### Version

0.12.9

---

_Comment by @CorneiZeR on 2025-08-19 07:55_

<img width="817" height="578" alt="Image" src="https://github.com/user-attachments/assets/5ddc2394-ffa2-4cb7-adc8-f4521931c73f" />
Hmm, I wasn't paying attention, you just changed the format, and PyCharm hasn't had time to get used to the new format yet.

Closing the issue.

---

_Closed by @CorneiZeR on 2025-08-19 07:55_

---

_Comment by @ntBre on 2025-08-19 13:21_

Sorry for the trouble here. We didn't expect this to be a disruptive change, but it seems that more programs are parsing our full output than expected (https://github.com/astral-sh/ruff/issues/19966 as another example). It's probably not very helpful for PyCharm, but [this](https://github.com/rust-lang/rust-mode/blob/9915b3a585a7a75e9126df9e0e9d1df8057ae3cf/rust-compile.el#L12-L23) is the regex used in the `rust-mode` for Emacs (with the many double escapes typical of Emacs regexes). Our new formatting should closely match that of rustc.

---

_Comment by @Danipulok on 2025-08-20 04:08_

@ntBre I also had a similar issue. Thanks a lot for the response, even though the issue got closed! I tried to debug how Pycharm highlights file paths in the console window, here’s some helpful (maybe) info:

Pycharm highlights file paths only if:
1. Files exist (absolute or relative path);
2. There are no special symbols before file name (e.g., "<>|" etc.)

And because the `ruff` output format is:
`—> file.py:line`
It "bugs" because of `>`

I tried searching for a plugin in Pycharm for this, and only found partially working one. But unfortunately it handles only absolute paths or paths starting with `/`, so it doesn’t help in this case

Here’s the issue I created for that plugin, maybe author will change the regex. And there are also some examples and screenshots of how Pycharm handles file paths:
https://github.com/Shadow-Devil/output-link-filter/pull/1

Including that a lot of people are using Pycharm (I suppose) and use `ruff` on a daily basis, maybe there will be some changes/suggestions for `ruff` users?

I thought about a global config file, but it’s not convenient, since you have to put `-c ~/.ruff.toml` to every run. And it’s not really project-related to put `output_format` option inside each `pyproject.toml` IMHO. 

The only kinda good solution I thought of without making breaking changes is adding env variable like `RUFF_OUTPUT_PATH`. Or maybe someone could create some plugin for the Pycharm for this? Because unfortunately in my experience, Pycharm is _really_ slow with updates and I doubt they could release a change in nearest half a year at least

---

_Comment by @ntBre on 2025-08-20 13:54_

I was writing up some suggestions for configuring Ruff as an external tool in PyCharm based on our  [docs](https://docs.astral.sh/ruff/editors/setup/#pycharm), but I'm realizing that this might be an embedded terminal inside of PyCharm? Which I'm guessing isn't affected by the tool configuration.

This is a bit of a hack, but one option to avoid setting the output format would be some kind of shell alias like this:

```shell
function myruff {
    CLICOLOR_FORCE=1 ruff "$@" | sed 's/ .*-.*-.*>.* //'
}
```

The combination of `CLICOLOR_FORCE` and all of the wildcards in the sed regex should preserve the escape characters for colorizing the output while stripping the `-->` from the filename.

This could also potentially be a useful feature for the [ruff-pycharm-plugin](https://github.com/koxudaxi/ruff-pycharm-plugin) since it sounds like plugins can add filters on the console output.

---

_Comment by @Danipulok on 2025-08-20 20:17_

@ntBre thanks a lot! This works!

I guess I will create an issue in the plugin you have mentioned. It would be better and more convenient for the users, you are completely right

---

_Referenced in [koxudaxi/ruff-pycharm-plugin#614](../../koxudaxi/ruff-pycharm-plugin/issues/614.md) on 2025-08-20 20:43_

---

_Referenced in [astral-sh/ty#1079](../../astral-sh/ty/issues/1079.md) on 2025-08-21 10:44_

---

_Comment by @MichaReiser on 2025-08-21 11:06_

I think we should keep this issue open as it is clearly a regression

---

_Renamed from "[0.12.9] File path not shown in default output of ruff check" to "[0.12.9] File path not clickable in PyCharm" by @MichaReiser on 2025-08-21 11:06_

---

_Renamed from "[0.12.9] File path not clickable in PyCharm" to "File path not clickable in PyCharm" by @MichaReiser on 2025-08-21 11:06_

---

_Label `bug` added by @MichaReiser on 2025-08-21 11:07_

---

_Label `diagnostics` added by @MichaReiser on 2025-08-21 11:07_

---

_Reopened by @MichaReiser on 2025-08-21 11:07_

---

_Comment by @MichaReiser on 2025-08-21 11:08_

The links are clickable in RustRover. I think our best course of action here is to open an issue on the PyCharm repository

---

_Comment by @MichaReiser on 2025-08-21 11:19_

I created https://youtrack.jetbrains.com/issue/PY-83546/Ruff-File-paths-are-no-longer-clickable

---

_Comment by @kxrob on 2025-08-22 19:11_

I have this problem in other contexts. Not worth discussing them specifically. But:

IMO, this is not acceptable to change the default format away from like

`<LINESTART>common/decorators.py:16:9:  INFOEXT`

That's a widely used and efficient to read / grasp / parse standard of most outputting tools and any parsing editors/debuggers/IDEs can understand it. Most of them do not anytime soon have a special case parser for such a new funky default format. There is even extra wasteful space with extra line break to be aware of to parse the INFOTEXT. 

To put the issue location info some where nest far after the INFOTEXT is very hard for humans to perceive too.

Why should a python tool do "Our new formatting should closely match that of rustc." - and even disruptively ?? 
If anything, then rustc, if it really has such an odd output format, should the other way around adopt a common default format IMO :-)

Such new format should be optional, if needed.

Even before ruff 0.12.8 outputted certain warnings in a different format, which is also hardly parsable:

`warning: Invalid `# noqa` directive on somefile.py:12: expected `:` followed by a comma-separated list of codes (e.g., `# noqa: F401, F841`).
`
A more general parser for many tools needs to detect filenames with spaces. Such strang format is rather impossible to parse unless the parser has a billion special regex'es for any different tool and restricted filename convention subcase ...
(And Python scripts filenames can principally contain spaces.)

Please restore the default output format.

---

_Comment by @ntBre on 2025-08-22 20:28_

I totally understand the frustration and that this is a different format from many other tools. If we had realized the impact this would have on tools like editors, we also would have held off until a minor release instead of releasing this in a patch version.

That said, there are good reasons to keep this format other than just that rustc also uses it. In particular, one of the main motivations is to work well with the new sub-diagnostic system we've added. An example of this just went out in the last release for F811:

```
F811 Redefinition of unused `bar` from line 6 
  --> F811_0.py:10:5                          
   |                                          
10 | def bar():                               
   |     ^^^ `bar` redefined here             
11 |     pass                                 
   |                                          
  ::: F811_0.py:6:5                           
   |                                          
 5 | @foo                                     
 6 | def bar():                               
   |     --- previous definition of `bar` here
 7 |     pass                                 
   |                                          
help: Remove definition: `bar`                
```

These are both part of the same diagnostic, with the `-->` line signaling the start of the diagnostic and the `:::` divider showing a secondary piece of the diagnostic that can be in a different part of the file. Ruff still only analyzes a single file at a time, but this generalizes to diagnostics pointing at other files in the case of rustc and ty, which also uses this diagnostic system. Ruff should be able to use that functionality one day too.

There are also some additional motivations in the Rust [RFC](https://rust-lang.github.io/rfcs/1644-default-and-expanded-rustc-errors.html) that proposed this format for Rust, such as placing the focus of the diagnostic on the error code rather than the line number.

Again, I know that this is jarring. I wasn't writing Rust in 2016 when the change in this RFC went into effect, but I remember being quite disoriented when similar changes came to the `panic` message format more recently. My editor regexes also broke then, so I've felt that pain point myself.

However, I now quite prefer the new error format, and we're hoping Ruff users will like it too, even if it's uncomfortable initially. And if you need to parse Ruff's output, you can still use the `concise` output format, which has the same format as before, or one of the structured output formats like JSON.

We also agree with you about the different formatting of the warnings. Our new diagnostic system has support for different error severities, and I believe we should be able to turn these into real diagnostics with the same format as lint rules, as well as support for all of the `--output-format` options.

---

_Comment by @Cjkjvfnby on 2025-08-25 14:30_

For myself, I mitigated it by adding `RUFF_OUTPUT_FORMAT=pylint` to PyCharm terminal (Settings -> Tools -> Terminal -> Environment variables,  start new terminal). 

This has the same effect as `--output-format=pylint`.

However, I haven't found a way to include it in the `pyproject.toml`, to solve it for the whole team.

---

_Comment by @Danipulok on 2025-08-25 14:37_

@Cjkjvfnby there is an option for this in `pyproject.toml`: https://docs.astral.sh/ruff/settings/#output-format

However I did not see `RUFF_OUTPUT_FORMAT` anywhere, so it's kind of funny that each of us only saw one method of solving this

---

_Comment by @kxrob on 2025-08-25 16:01_

> one of the main motivations is to work well with the new sub-diagnostic system we've added. An example of this just went out in the last release for F811

In one case I have control over the regex. And watched a few days how I typically use things. 
Overall, I can say, that the most important thing I learned here is that the 'concise' output format even exists 😃 - and is my default or rather only format I really use - despite the updated regex.

Already with v0.12.8, the verbose output of 10 lines (now 11) per error was basically cluttering the limited display space (typically in a small output / console / message pane) with unnecessary noise, compared to pyflakes / flake8 / pylint default output format. Making it difficult to overlook and navigate.  

I would be interested how many people actually read the lots of verbose text at all more than once per year or so?

The errors are typically trivial in 99.9+% of cases and clear with minimal wording, particularly with Python, for any programmer with minimal experience, and the file location(s) and click on jump positions are the primary and only thing mattering for me - to go fix it. 

In C and possibly Rust there may be a little more usecases for multiple 'connected' infos and sometimes multiple jump marks with complex types etc.
Redefinition error in e.g. GCC by default looks like this:

```
x.c: In function 'main':
x.c:42:8: error: redefinition of 'x'
   42 |    int x = 77;
      |        ^
x.c:27:8: note: previous definition of 'x' with type 'int'
   27 |    int x = (a && 1) | (b && 1) << 3;
      |        ^
```

That's fairly simple, clear, and compatible, not breaking things. And 'wasting' only 3 lines per location. And is still way more readable.
(IMO in trivial unambiguous cases, hence most of the time, the extra column position mark line(s) should be omitted automatically as well)

The programmer of the format may have different ideas about the importance of verbose fancy output. Yet I doubt that the new, or even former extremely verbose and space wasting format is a good and efficient format at all for most users, and would possibly better be a verbose level format on demand. Thats the usual way in most other tools: high attention consuming verbosity on demand.

---

_Comment by @InSyncWithFoo on 2025-08-27 03:00_

As it happens, I [implemented this feature recently](https://github.com/InSyncWithFoo/ryecharm/commit/8adc485fc2e755841c8229bfecfaa080556333fd) in RyeCharm.

It hasn't been documented or released yet, but I'll get to that this weekend. Plus, [nightly releases](https://plugins.jetbrains.com/plugin/25230-ryecharm/versions/nightly) are always available.

---

_Comment by @Danipulok on 2025-08-27 03:07_

@InSyncWithFoo wow, thanks a lot!
Didn’t know about this plugin, but the comments seem really nice
Will check it for sure
Thanks a lot for the plugin and for the feature!

---

_Comment by @provinzkraut on 2025-12-13 13:37_

Are there hopes of any solution that does not require custom IDE setup / plugins, i.e. to simply restore the old behaviour? This is breaking a lot of people's workflows, and we've held off upgrading ruff at our organisation for this reason. IMO mandating a specific IDE setup for a command line tool to work isn't scalable, and I would really appreciate if ruff could continue working as expected out of the box.

> However, I now quite prefer the new error format, and we're hoping Ruff users will like it too, even if it's uncomfortable initially.

I think the issue isn't as much about preferring the old over the new format visually. It's that it's breaking existing tools. Being able to navigate with a simple click to the source of the error beats any visual improvement. 

---

_Comment by @Danipulok on 2025-12-13 13:49_

@provinzkraut, a lot of time has passed, and if nothing has been done yet, I doubt anything will. It seems if you want a specific format, you have to rely on "output_format". About existing tools, they have to be upgraded.

---

_Comment by @provinzkraut on 2025-12-29 12:36_

@MichaReiser any updates on this? This is still broken for the majority of PyCharm users by default.

---

_Comment by @MichaReiser on 2025-12-29 12:44_

> [@MichaReiser](https://github.com/MichaReiser) any updates on this? This is still broken for the majority of PyCharm users by default.

I don't think so. You can upvote https://youtrack.jetbrains.com/issue/PY-83546/Ruff-File-paths-are-no-longer-clickable here to help the JetBrains team prioritize the issue.

@KotlinIsland is there something we can do on our end to help resolving this issue in PyCharm?

---

_Comment by @provinzkraut on 2025-12-29 14:20_

> > [@MichaReiser](https://github.com/MichaReiser) any updates on this? This is still broken for the majority of PyCharm users by default.
> 
> I don't think so. You can upvote https://youtrack.jetbrains.com/issue/PY-83546/Ruff-File-paths-are-no-longer-clickable here to help the JetBrains team prioritize the issue.

Hm. So this will definitely not be addressed from ruff's side? i.e. it's a no to [this](https://github.com/astral-sh/ruff/issues/19983#issuecomment-3649441914)?

That would be a bit disappointing to be honest. It worked before, and due to an update to ruff it no longer works. So while I understand the intent behind the change, and do agree that it *would* be great if it worked, it doesn't feel entirely correct to put the ball in PyCharm's court only. To me, as an end user at least.


---

_Comment by @MichaReiser on 2025-12-29 14:30_

> Hm. So this will definitely not be addressed from ruff's side? i.e. it's a no to https://github.com/astral-sh/ruff/issues/19983#issuecomment-3649441914?

I consider it unlikely, given that it is an issue with JetBrains IDEs only (and the format is supported within RustRover). But we'll explore alternatives if it's a definitive no from the JetBrains side.

---

_Comment by @Danipulok on 2025-12-29 14:32_

Well, the alternative is using plugin for PyCharm. I don't think going back is possible now after so much time. And this is actually an PyCharm issue, I agree with that. All we (PyCharm) users can do it upvote related issue and install a plugin (which is an upgrade anyway)

---

_Comment by @KotlinIsland on 2025-12-29 15:56_

I'm going to look into this soon when I have capacity, hopefully will be available soon

---
