---
number: 14985
title: Add CLI flag to configure relative vs full path printing for emitters
type: issue
state: open
author: brupelo
labels:
  - needs-design
assignees: []
created_at: 2024-12-15T16:11:06Z
updated_at: 2025-05-15T03:16:34Z
url: https://github.com/astral-sh/ruff/issues/14985
synced_at: 2026-01-07T13:12:16-06:00
---

# Add CLI flag to configure relative vs full path printing for emitters

---

_Issue opened by @brupelo on 2024-12-15 16:11_

**Follow-up from [#9417](https://github.com/astral-sh/ruff/issues/9417)**

I’ve added a new flag to the CLI that allows configuring the Printer to print either relative or full paths across the different existing emitters.

The problem with relative paths is that they are not always convenient, especially when linting in an IDE or when dealing with additional cases where valuable information is lost. This is particularly relevant when parsing paths from Ruff's stdout, which can be useful for feeding into LLMs. There are also situations where full paths are necessary to redirect Ruff’s stdout, but the information is lost when using relative paths. Furthermore, we can’t always assume that the working directory of Ruff’s execution is the same as the working directory where the `pyproject.toml` file resides.

While relative paths are more compact, they introduce a variety of limitations in different use cases. Therefore, it would be beneficial to retain existing functionality while adding the option to configure how paths are printed.

I’ve created a draft PR for you to review so you can see where I am with this implementation.

Draf PR here -> https://github.com/astral-sh/ruff/pull/14987

---

_Referenced in [astral-sh/ruff#14987](../../astral-sh/ruff/pulls/14987.md) on 2024-12-15 16:12_

---

_Comment by @MichaReiser on 2024-12-15 16:15_

Thanks for opening this issue. 

Would you mind sharing a few examples for

> The problem with relative paths is that they are not always convenient, especially when linting in an IDE or when dealing with additional cases where valuable information is lost.

Most IDEs have good support for relative links and you can click them to jump right to the file. This works for as long as ruff is run from the same directory as the IDE's working directory.

> Furthermore, we can’t always assume that the working directory of Ruff’s execution is the same as the working directory where the pyproject.toml file resides.

I don't think Ruff makes this assumption. You can see how ruff always uses paths relative to the current working directory. The location of the pyproject.toml doesn't matter. (I ran the command from the root directory)

```
~/astral/ruff/target/debug/ruff check --cache-dir ~/astral/test/.ruff_cache ~/astral/test --config ~/astral/test/pyproject.toml --select=ALL
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
warning: Detected debug build without --no-cache.
Users/micha/astral/test/test.py:1:1: INP001 File `Users/micha/astral/test/test.py` is part of an implicit namespace package. Add an `__init__.py`.
Users/micha/astral/test/test.py:1:1: D100 Missing docstring in public module
Users/micha/astral/test/test.py:1:1: I001 [*] Import block is un-sorted or un-formatted
```

---

_Label `cli` added by @MichaReiser on 2024-12-15 16:15_

---

_Label `wish` added by @MichaReiser on 2024-12-15 16:15_

---

_Comment by @brupelo on 2024-12-15 16:38_

@MichaReiser Thanks for the questions! I'll come back to this over the next few days with more details so you can better understand the value added. Could you please keep the PR open until this issue is resolved?

The idea is straightforward: I want to extend Ruff's behavior with a new flag that doesn’t break any existing functionality. So, the default behavior and existing formatters would remain as they are, and this would be an additive feature aimed at a niche use case.

It's a bit difficult for me to provide a complete example right now as I need to make some changes to the code, rebuild it, and prepare relevant examples. I can say that adding this flag to the ongoing PR would take me about 10 minutes, but I haven’t reached that part of the codebase yet. I'm not sure how hard it will be to add the Clap flags without breaking anything. As mentioned, I’m still a beginner with Rust, and what I thought would be a 30-minute task has already taken me a few hours, and I've definitely picked up some antipatterns along the way. Please bear with me 😅

That said, it’s been a really cool project to contribute to. One challenge I’m facing is reducing iteration times in Rust, especially when compiling top-level crates.

Also, today I was trying to get Sublime LSP analyzer to work with imports but had no luck. Out of curiosity, what’s the typical IDE and environment used by Ruff contributors for efficient iteration and refactoring?

It would be great to have some tips for new contributors so they can onboard quickly.

Again, thanks for your time. I’m excited about getting this feature into the main branch. I want to use Ruff in place of my old {black, isort, autoflake8, flake8} combo and be able to run and refactor dozens of projects in different repos efficiently, without breaking a sweat. I’m confident that Ruff will make this process even faster.

P.S. To keep it simple, imagine you want to lint several large codebases, aggregate the metrics, and then reformat the output to pass to another tool for specific reasons. Right now, the process is very rigid. I recall some linters offering configurability to adjust how paths are output by the emitters. Would that work better for you instead a simple boolean flag? Essentially, something similar to how Python’s logging formatter gives users the flexibility to customize the output. Again, the default behavior would remain as is, with no breaking changes.

---

_Comment by @brupelo on 2024-12-16 10:50_

I've cleaned up a bit my feature branch so it now uses env.vars instead of the previous hardcoded solution, here's some fast testing:


```
========================================
File: a.py
========================================

x()x()

----------------------------------------


========================================
File: Justfile
========================================

    config_file := "d:/pyproject.toml"
    ruff_file := "ruff"

    test-abs:
        @RUFF_SHOW_FULL_PATH=1 {{ruff_file}} check --fix a.py --output-format=concise --config {{config_file}}

    test-rel:
        @RUFF_SHOW_FULL_PATH=0 {{ruff_file}} check --fix a.py --output-format=concise --config {{config_file}}

----------------------------------------

(venv) d:\tests>just test-rel
a.py:1:4: SyntaxError: Simple statements must be separated by newlines or semicolons
Found 1 error.
error: Recipe `test-rel` failed on line 8 with exit code 1

(venv) d:\tests>just test-abs
d:\tests\a.py:1:4: SyntaxError: Simple statements must be separated by newlines or semicolons
Found 1 error.
error: Recipe `test-abs` failed on line 5 with exit code 1

```


---

_Label `cli` removed by @MichaReiser on 2024-12-16 11:11_

---

_Label `wish` removed by @MichaReiser on 2024-12-16 11:11_

---

_Label `needs-info` added by @MichaReiser on 2024-12-16 11:11_

---

_Comment by @brupelo on 2025-01-18 20:23_

@MichaReiser  Hi,

Out of curiosity, what does the needs-info label mean in this context? I’ve been using this branch successfully for the past month without any issues. I’ve also been rebasing it consistently against the latest origin/main as it got stagnated. However, I’m unsure if this will ever make it to master.

I’d like to help finalize this for release. While my time is limited, I could allocate some if only minor reviews are needed. Could you clarify the bare minimum requirements for a feature to be merged into master? I’ve made efforts to ensure this doesn’t introduce any breaking changes, but I’d like to know what the next steps are.

Thanks!

P.S. The other issue I reported—where adding a lot of imports causes it to get stuck—is also a blocker preventing me from fully leveraging Ruff, that one doesn't seem like a good first-issue though. Other than these, the tool is truly fantastic.

---

_Comment by @MichaReiser on 2025-01-18 21:20_

Needs info means that we need more information from the author, you, to move the issue forward. 

To me it's still unclear what you need this feature for. Can you tell me a specific use case/example? 



---

_Comment by @brupelo on 2025-01-19 05:52_

@MichaReiser Hey Micha, sure! I thought I had explained this use-case a few months ago already, and again when I opened this one last month. But let me try again, okay?

I’m using Sublime Text, and I’ve set up some custom commands for running ruff. Here’s how it works: I can either lint a bunch of Python folders (which may not even be in the same workspace, as they include editable packages and subdependencies from different companies or organizations), or I can pick individual Python files from different drives via the sidebar menu.

Of course, the more typical and frequent use case would be running a command with a shortcut while editing a single file or on save. That’s what I usually do when coding and focusing on single tasks. These two workflows—batch linting multiple folders or linting individual files—are things I deal with on a daily basis.

To make this work, I’ve set up some custom commands that hook into ruff, and I pass the corresponding paths. I can show you an example of a typical command so you’ll understand how it looks.

```
        {
            "command": "zebra_exec_run",
            "keys": ["ctrl+shift+8"],
            "args": {
                "cmd": "ruff check --fix ${file_name} --output-format=concise && ruff format ${file_name}",
                "file_regex": "(.*?\\.py):(.*?):(.*?):(.*)",
                "quiet": true,
                "working_dir": "${directory}",
                "env": {
                    "RUFF_SHOW_FULL_PATH": "1"
                }
            },
            "context": [{"key": "selector", "operator": "equal", "operand": "(source.python)", "match_all": true} ]
        },
        {
            "command": "zebra_exec_run",
            "keys": ["ctrl+shift+9"],
            "args": {
                "cmd": "ruff check --fix ${file_name} --output-format=concise && ruff format ${file_name}",
                "file_regex": "(.*?\\.py):(.*?):(.*?):(.*)",
                "quiet": true,
                "working_dir": "${directory}",
                "env": {
                    "RUFF_SHOW_FULL_PATH": "0"
                }
            },
            "context": [{"key": "selector", "operator": "equal", "operand": "(source.python)", "match_all": true} ]
        },
```

the above would be simple commands showcasing ruff with absolute paths and with not (i've used my commit rebased on latest origin/main)

but when i need to lint several folders from a single codebase or multiple ones I'd have something like this

```
                {
                    "caption": "Lint",
                    "command": "zebra_exec_run",
                    "args": {
                        "cmd": "ruff check --fix ${paths} --output-format=concise && ruff format ${paths}",
                        "file_regex": "(.*?\\.py):(.*?):(.*?):(.*)",
                        "quiet": true,
                        "working_dir": "${directory}",
                        "env": {
                            "RUFF_SHOW_FULL_PATH": "1"
                        },
                        "paths": []
                    },
                    "context": [{"key": "selector", "operator": "equal", "operand": "(source.python)", "match_all": true} ]
                },
```

which i trigger from here

![Image](https://github.com/user-attachments/assets/6c4e6aa9-2373-413c-a1c5-769a7e6e1fc8)

The reason I insisted on this change and spent time working on it is that, with other linters providing absolute paths, I can easily use regex to fix the source code, double-click to navigate, and go to files directly. Without absolute paths, this was impossible in those scenarios.

Does that make sense now? I hope this can make it to the master branch somehow. As I mentioned, I’ve implemented it in a way that wouldn’t break anything for others, but it covers these additional cases. That said, it’s quite a pain to keep rebasing frequently to ensure it stays in sync with the latest origin/main.

I’d really like to be able to check out the latest origin/main without worrying about spending extra time resolving potential merge conflicts or bugs. The good news is that every time I’ve rebased my small diff over the past month, it hasn’t conflicted at all and has worked out of the box. It seems the impacted files are rarely changed these days, which is reassuring.



---

_Comment by @MichaReiser on 2025-01-19 12:11_

Have you tried using `--output-format json` with `jq` because it includes the full path and is the intended machine readable output whereas concise is for humans. 

Another alternative is to set the working directory to the project root. Maybe that's already what you do but I couldn't find any sublime documentation explaining what the `$directory` variable resolves to. Do you have a link where I could learn more about it? 


---

_Comment by @MichaReiser on 2025-01-19 12:13_

Can you try `$project_path` instead of `$directory`? The former resolves the project root and the latter to the enclosing directory (which then makes ruff's paths relative to the directory rather than the project). 

See this chat gpt explanation https://chatgpt.com/share/678cec46-c288-8002-9d70-6966d8ff4356

---

_Comment by @brupelo on 2025-01-19 13:31_

Hey Micha,

A few months ago, I tried using `jq` with custom plugins. While I made it work, it required adding another plugin on top of my custom Sublime exec command, which I wrote back in 2017. My command has been working well—spawning processes, capturing stdout/stderr with proper interleaving, regexes, custom cancellation, and threading. It offers proper interleaved output, unlike custom Sublime ones.

When I met `ruff`, I explored `jq` options, but it didn't meet my needs. After thinking it through, I realized the best solution was being able to report output the way I wanted without relying on shortcuts or extra plugins, just by using a barebones st without any extra plugin.

I understand the `project_path` concept ofc, and I saw few Sublime projects over the years using non-cross platform tooling such as `sed` and other hacks to make paths cross-platform to capture file/line/col in the output when using tool such as cargo that don't provide proper way to write absolute paths, as least that I know

However, even with those hacks you'll find limitations when handling projects across different drives or folders. On that case, the only proper way would be using a simple regex with a reporter that works seamlessly with Sublime Text’s built-in build systems—no extra plugins required, allowing for mixed drives and folders across multiple workspaces.

Regarding your question, I use SublimeText’s custom exec command when no command is specified in my build system, that's why you won't find that custom $directory arg in the official docs, cos I'm not using sublime's default exec. For more info how the default one works you can take a look at sublimetext\Data\Packages\Default\exec.py  , you can use this one https://packagecontrol.io/packages/PackageResourceViewer and https://github.com/randy3k/AutomaticPackageReloader if you want fully understand how it works behinds the scenes, it's pretty simple.

Anyway, long story short, it would be great to have a way to customize reporting to get full paths to use simple regexes without modifying the Rust codebase every time (e.g., recompiling, testing). 

From what I'm reading it feels like you've got some concerns about env.vars modifying reporters behaviour? Could you confirm? If that's the case, I understand... but think it'd still be nice having a customizable way to report output the way the user wants somehow, other linters give this flexibility.

But let me stop here, instead of explaining my use case any further, could you please elaborate what you think it'd be the best solution that'd satisfy this use-case, or other similar ones without having to modify the codebase each time? IMHO the solution should be user-configurable while still adhering to the Open/Closed Principle.

Please let me know the simplest, most cost-effective way to implement this feature while respecting codebase’s structure, fundamentals, etc...

Thanks!

---

_Comment by @brupelo on 2025-01-19 13:40_

If you think about it, tools like `jq` and `awk` aren't ideal solutions because they force the user to rely on external tools. For example, using `$project_path` with `awk` could look something like this:

```json
{
    "name": "Cargo",
    "working_dir": "$project_path",
    "shell_cmd": "cargo build  2>&1 | awk 'BEGIN { errmsg=\"\" } /(error|warning)(\\[E[0-9]+\\])?:.*/ {errmsg=\\$0; next} / *--> *.*/ { printf \"%s::::%s\\n\", \\$0, errmsg; next} {print \\$0}'",
    "file_regex": " +--> +([a-zA-Z0-9_\\/.-]+):(\\d+):(\\d+)::::(.*)"
}
```

The problem If we used a similar approach in `ruff` is:

1. It’s not cross-platform.
2. It still wouldn't work across different drives on Windows.
3. It'd require the installation of additional software or plugins.

Ideally, `ruff` should work seamlessly across different IDEs in a cross-platform way, without forcing extra dependencies.

---

_Comment by @MichaReiser on 2025-01-19 14:50_

Thanks for the extra context. Your setup seems very specific and custom which makes it difficult and time intensive for me to reproduce. Considering that there are alternatives -- that I understand are somewhat frustrating because they aren't ideal -- I don't think me figuring out the right design is the right prioritization because there are more fundamental issues that affect more users. I know that this isn't very helpful for you but I hope you understand.

I'll keep this issue open so that we can come back when we've more time. 

---

_Label `needs-info` removed by @MichaReiser on 2025-01-19 14:51_

---

_Label `needs-design` added by @MichaReiser on 2025-01-19 14:51_

---

_Comment by @brupelo on 2025-01-19 16:33_

@MichaReiser First of all, thanks for taking the time! No problem at all—I completely understand. Whenever you guys are able to revisit this after clearing the high-priority stack, that’s fine with me. My goal is simply to understand the direction and identify features that will benefit all users, not just me. Please don’t take these requests as a personal wish list—I aim to open issues that report bugs or suggest improvements that help others without breaking anyone’s setup or wasting time.

I just hope this doesn’t end up like some repos where you open an issue, and five years later, the author responds when you’ve completely forgotten what the repo was even about! ;)

In the meantime, I’ll continue rebasing my local branch as I’ve been doing for the past month. The good news is my small diff seems to touch a relatively stable part of the codebase that isn’t affected by daily commits, so it’s been smooth sailing so far, allowing me to use ruff with my setup.

If you need more specifics or details to make a better-informed decision, feel free to ping me—I’ll be monitoring this closely.

---

_Comment by @alexeagle on 2025-05-09 21:49_

Drive-by upvote; we'd like this for Bazel as well. It runs tools in a custom sandbox called the "execroot" so it produces ugly SARIF like

```
#           "locations": [
#             {
#               "physicalLocation": {
#                 "artifactLocation": {
#                   "uri": "file:///home/runner/.cache/bazel/_bazel_runner/89e91fd6b6dccc7b506d63833e3c5c70/sandbox/processwrapper-sandbox/542/execroot/_main/src/unused_import.py"
```

which makes it impossible to plumb the data into tooling like an editor.

---

_Comment by @brupelo on 2025-05-10 10:16_

@alexeagle A few months ago, I created a fork with some fixes for this. Initially, I sought feedback and explained my issues, hoping to get it merged into the master branch, as that would have been really convenient. I made an effort to keep my branch up-to-date by recompiling, verifying, and syncing it with the latest version of Ruff a few times. However, as you can imagine, this process was quite time-consuming. At a certain point, I gave up after submitting several passing PRs that were rejected simply because I didn't have the time to keep up with everything.

Nowadays, I'm using my fork with an outdated custom branch of Ruff, which I've been using happily ever since. While it doesn’t introduce any breaking changes, the downside is that I haven't merged the latest features from Ruff. Merging now would likely take some time, which I’m not going to do. I’ve already tried, and after encountering a couple of conflicts, trying to build (which took several minutes), and dealing with crashes, I decided not to go down that rabbit hole. So, I’ll stick with my outdated custom version.

I hope this clarifies things. It’s unfortunate, as I would have liked to contribute further, but with the resistance to small branches and the time-consuming nature of contributing, I’ve stopped for now—at least until I come across new, specific use cases for me. In the meantime, I’ll continue using my current version.

That said, absolute paths are a real use case and quite convenient in certain situations. A lot of other linters provide this ability out of the box, and relative paths often result in lossy information.

Anyway, if you want to give it a shot, you can find the outdated commit here. However, you’ll need to resolve conflicts, go through the conversations, review, etc. So… it’s up to you 😅

I've tried right now my outdated [custom branch](https://github.com/brupelo/ruff/commit/0da956dbf486c1ebfe31d5f7477ce4ab03a6c9be) just to make sure it's still building and yeah, it does... Hope it helps over there!

---

_Comment by @MichaReiser on 2025-05-10 10:27_

> Drive-by upvote; we'd like this for Bazel as well. It runs tools in a custom sandbox called the "execroot" so it produces ugly SARIF like

I'm not sure I understand how absolute paths would help with this. Could you elaborate?

---

_Comment by @brupelo on 2025-05-10 10:54_

FYI, I just went down the rabbit hole and caught up with the latest origin/main. You can find a simple [patch](https://github.com/brupelo/ruff/commit/d27bdd9f57c789ca8420628513ac78c16bf9a1da) on top of the latest main. So far, it worked fine with the basic testing I did with rel/abs paths. Resolving conflicts wasn’t as bad as I had anticipated, despite being ~600+ commits behind.

---

_Comment by @alexeagle on 2025-05-12 14:50_

@MichaReiser I currently see an absolute path in the SARIF file, what's needed is an option to print a relative path instead.

---

_Comment by @MichaReiser on 2025-05-12 14:52_

> [@MichaReiser](https://github.com/MichaReiser) I currently see an absolute path in the SARIF file, what's needed is an option to print a relative path instead.

Paths relative to the current working directory is the default output for the CLI. Absolute paths are only emitted if the path can't be relativized to the current working directory.

---

_Comment by @alexeagle on 2025-05-15 03:16_

Bazel also uses symlinks from the execroot back to the source tree, so I would guess that's why the SARIF report doesn't relativize. I'll have to construct a reproduction outside Bazel with a similar symlinking scheme.

---
