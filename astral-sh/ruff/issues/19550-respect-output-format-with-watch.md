```yaml
number: 19550
title: "Respect `--output-format` with `--watch`"
type: issue
state: closed
author: sathishvj
labels:
  - cli
assignees: []
created_at: 2025-07-25T04:03:59Z
updated_at: 2025-10-27T18:35:47Z
url: https://github.com/astral-sh/ruff/issues/19550
synced_at: 2026-01-12T15:54:56Z
```

# Respect `--output-format` with `--watch`

---

_@sathishvj_

### Question

The ruff linter for python in watch mode does not show code details and position of error.


If I run "ruff check .", if there is a code error, it shows me the code where the issue is.

If I run "ruff check --watch .", it does not show me code details.





### Version

_No response_

---

_Label `question` added by @sathishvj on 2025-07-25 04:04_

---

_Comment by @MeGaGiGaGon on 2025-07-25 04:12_

Please give more information on the ruff version you are using (run `ruff version`), the files/folder you are trying to watch, and the configuration you are using. Testing locally it works fine on ruff 0.12.4 (ee2759b36 2025-07-17) (running `uvx ruff check --watch . --isolated`), so more details are needed. Info about your operating system may also be useful.

---

_Comment by @sathishvj on 2025-07-25 04:29_

ruff 0.12.4
MacOs Sequoia 15.5
BASH_VERSION = 5.2.37(1)-release
I don't have a ruff.toml locally

I installed ruff with homebrew. I also ran it as you did with "uvx ruff". 
The output is still minimal. 

The folder I am watching is the current folder with a bunch of python files. 

---

_Comment by @MeGaGiGaGon on 2025-07-25 05:30_

👍 . Sadly I can't test it since I'm on windows, but hopefully someone else with a mac can. Can you send the output of `uvx ruff check --watch . --isolated -v`, since there might be something useful there?

---

_Comment by @sathishvj on 2025-07-25 05:37_

[11:01:51 AM] Starting linter in watch mode...
[2025-07-25][11:01:51][ruff_workspace::resolver][DEBUG] Included path via `include`: "<full path>/main.py"
[2025-07-25][11:01:51][ruff::diagnostics][DEBUG] Checking: /<full path>/main.py
<local path>/main.py:344:3: SyntaxError: Expected an expression

There are lots of other files that are listed too, but I believe this would be the relevant logs.

Thank you. 

---

_Comment by @MeGaGiGaGon on 2025-07-25 05:58_

Ah I see, I misunderstood the problem. I assumed it was that the file paths weren't appearing, instead of wanting the full output format.

As far as I'm aware, currently `--watch` only supports that singular output format, and trying to set other ones with `--output-format` has no effect.


---

_Comment by @sathishvj on 2025-07-25 06:17_

Oh. My apologies for not being clearer in the original issue message - I shouldn't have wasted your time like that and I shall do better next time. Yet, thank you for your responses. 

For others here, my workaround is to use a file watcher like fswatch/watchman/watchexec to watch for file changes and then execute ruff. 

To the ruff team, it would be good to have a flag to enable detailed output with code snippet even in --watch mode, like so: 

```
main.py:344:3: SyntaxError: Expected an expression
342 |     app.run(host='0.0.0.0', port=int(os.environ.get('PORT', 8080)), debug=True)
343 |
344 | x-
    |   ^
```

---

_Closed by @sathishvj on 2025-07-25 06:17_

---

_Comment by @MichaReiser on 2025-07-25 06:18_

@ntBre another diagnostic related improvement

---

_Comment by @ntBre on 2025-07-25 13:25_

@sathishvj I'll reopen this if it's okay with you. I think it makes sense to support more output formats in `watch` mode as you said.

---

_Reopened by @ntBre on 2025-07-25 13:25_

---

_Label `question` removed by @ntBre on 2025-07-25 13:25_

---

_Label `cli` added by @ntBre on 2025-07-25 13:25_

---

_Renamed from "How do i make ruff show more details in watch mode?" to "Respect `--output-format` with `--watch`" by @ntBre on 2025-07-25 13:26_

---

_Comment by @ntBre on 2025-07-25 13:33_

I noticed this when looking into https://github.com/astral-sh/ruff/issues/19552 after commenting here, but the `--preview` flag actually enables full output with `--watch`. We can still leave this open to allow other output formats, but that might help with your specific use case!

---

_Comment by @sathishvj on 2025-07-25 13:55_

Yes, it does! Thank you. 👍 
So, is this a fix needed in the docs? Or planned for a later release?


---

_Comment by @ntBre on 2025-07-25 19:53_

Glad to hear it! Yeah, I guess this was supposed to be stabilized in a release at some point, but I think it would still be nice to respect `--output-format` instead of just switching the default to `full`. If this preview behavior isn't document anywhere, that could be nice to add in the meantime.

---

_Comment by @mathieugouin on 2025-10-23 19:26_

Just saying I would love this to work:

```
ruff check --output-format concise --watch .
```


---

_Comment by @ntBre on 2025-10-23 19:31_

I think that exact command _will_ (appear to) work, but only because `concise` is still the default output format 😄 

In preview the default is now `full`, so we still need to respect the flag.

---

_Comment by @mathieugouin on 2025-10-24 19:45_

Strange... when I type the command I pasted above, I get the expanded/detailed output.

```
ruff --version
ruff 0.14.2
```

```
ruff check --output-format concise --watch .

D400 First line should end with a period
  --> Informal\test_gpib_name.py:28:5
   |
27 |   def main():
28 | /     """
29 | |     Test data:
30 | |
31 | |     ***
32 | |     ***
33 | |     ***
34 | |     ***
35 | |     ***
36 | |     ***
37 | |     ***
38 | |     ***
39 | |     """
   | |_______^
40 |       test_data = {
41 |           "ABC": "ABC",
   |
help: Add period
```


---

_Comment by @MichaReiser on 2025-10-27 07:43_

@mathieugouin, are you using `preview` mode (e.g., in your config?) because I can reproduce your behavior with `--preview`, and changing the output format isn't possible. This seems at least inconsistent with non-preview behavior and is probably related to showing fixes by default (CC: @ntBre). 

I'd be inclined to just support different output formats (at least in preview), or are there reasons why we can't do that for watch mode?

---

_Comment by @ntBre on 2025-10-27 13:09_

I don't think this is related to showing fixes. I thought there was an even older issue about this, but https://github.com/astral-sh/ruff/issues/19552 is the best I found. Non-preview watch mode uses concise output, and at some point we planned to switch to full output, but it has been in preview for quite a while. Neither stable nor preview currently support `--output-format`, but I agree that they should!

---

_Closed by @ntBre on 2025-10-27 16:04_

---

_Comment by @mathieugouin on 2025-10-27 18:35_

@MichaReiser You were right, I had `preview = true` in my `ruff.toml` and in fact this automatically "force" the output format to full.  In non-preview, the `--watch` output format reverts to concise.

Thanks for pointing that!

---
