---
number: 3272
title: Parser error and Rule violation on syntax error breaks Emacs integration
type: issue
state: closed
author: biscanli
labels:
  - question
assignees: []
created_at: 2023-02-28T11:31:26Z
updated_at: 2023-10-30T17:14:44Z
url: https://github.com/astral-sh/ruff/issues/3272
synced_at: 2026-01-07T13:12:14-06:00
---

# Parser error and Rule violation on syntax error breaks Emacs integration

---

_Issue opened by @biscanli on 2023-02-28 11:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Whenever ruff detects a syntax error that falls under rule `E999`, it also gives a parser error. A simple example for this is just starting a file (lets say `a.py`), typing `def` and saving. Once you run `ruff check a.py`. You receive two lines
```
error: Failed to parse a.py: invalid syntax. Got unexpected token Newline at line 1 column 4
a.py:1:5: E999 SyntaxError: invalid syntax. Got unexpected token Newline
Found 1 error.
```
This is not optimal because:
- `ruff` did give the proper rule violation, so on a semantic level it feels weird to also provide an error
- The combination of parser error and rule violation breaks integration with Emacs's `flymake`, which is the default package to integrate linters.

I tested the latter by turning off the rule, which solves the error out issue, but that is also not optimal because then you cannot see the errors.

I use a config with all rules turned on, but the default config also gave the same result, and the ruff version is `0.0.253`

---

_Comment by @charliermarsh on 2023-02-28 16:35_

Can you tell me more about how this impacts `flymake`? I would expect that the `error:` output goes to `stderr`, and that tooling built atop `ruff` would rely on `stdout`, but I'm guessing I'm wrong :)

---

_Label `question` added by @charliermarsh on 2023-02-28 16:35_

---

_Comment by @biscanli on 2023-02-28 18:59_

Yes. First of all, I should clarify that `flymake` uses a bit of a different command, namely `ruff --quiet --stdin-filename=stdin -`. I do believe `flymake` is gathering the output from `stdout` and working with it, but my emacs-fu is not strong enough to prove it 100%. `flymake` is an on the fly syntax checker so as you type the error below keeps popping up: 
```
error in process sentinel: flymake--highlight-line: Wrong type argument: integer-or-marker-p, nil
error in process sentinel: Wrong type argument: integer-or-marker-p, nil
```
I did try getting more debugging information out of Emacs, which got me this:
```
Debugger entered--Lisp error: (wrong-type-argument integer-or-marker-p nil)
  make-overlay(nil nil)
  flymake--highlight-line(#s(flymake--diag :locus #<buffer a2.py> :beg nil :end nil :type :error :text "E999 SyntaxError: invalid syntax. Got unexpected t..." :backend python-flymake :data nil :overlay-properties nil :overlay nil :orig-beg nil :orig-end nil))
  flymake--publish-diagnostics((#s(flymake--diag :locus #<buffer a2.py> :beg nil :end nil :type :error :text "E999 SyntaxError: invalid syntax. Got unexpected t..." :backend python-flymake :data nil :overlay-properties nil :overlay nil :orig-beg nil :orig-end nil)) :backend python-flymake :state #s(flymake--state :running backend-token18 :reported-p nil :disabled nil :diags (#s(flymake--diag :locus #<buffer a2.py> :beg nil :end nil :type :error :text "E999 SyntaxError: invalid syntax. Got unexpected t..." :backend python-flymake :data nil :overlay-properties nil :overlay nil :orig-beg nil :orig-end nil)) :foreign-diags #<hash-table eql 0/65 0x156a0ba34beb>) :region nil)
  flymake--handle-report(python-flymake backend-token18 (#s(flymake--diag :locus #<buffer a2.py> :beg nil :end nil :type :error :text "E999 SyntaxError: invalid syntax. Got unexpected t..." :backend python-flymake :data nil :overlay-properties nil :overlay nil :orig-beg nil :orig-end nil)))
  apply(flymake--handle-report python-flymake backend-token18 (#s(flymake--diag :locus #<buffer a2.py> :beg nil :end nil :type :error :text "E999 SyntaxError: invalid syntax. Got unexpected t..." :backend python-flymake :data nil :overlay-properties nil :overlay nil :orig-beg nil :orig-end nil)))
  #f(compiled-function (&rest args) #<bytecode -0x11abfd71816a6476>)((#s(flymake--diag :locus #<buffer a2.py> :beg nil :end nil :type :error :text "E999 SyntaxError: invalid syntax. Got unexpected t..." :backend python-flymake :data nil :overlay-properties nil :overlay nil :orig-beg nil :orig-end nil)))
  python--flymake-parse-output(#<buffer a2.py> #<process python-flymake> #f(compiled-function (&rest args) #<bytecode -0x11abfd71816a6476>))
  #f(compiled-function (proc event) #<bytecode 0xbff79c6a0c2e8d9>)(#<process python-flymake> "exited abnormally with code 1\n")
```
Since the `--quite` option is there it should not be a problem of reporting the error, but from what I can tell from reading on [sentinels](https://www.gnu.org/software/emacs/manual/html_node/elisp/Sentinels.html), it seems that when there is a parser error and a linting error the ruff subprocess changes state/gets interrupted abruptly. So to sum up
- Just linting error: fine
- Just parser error: fine
- Linting and parser error: sentinel chaos

P.S. Great package, and appreciate the fast reply :)

---

_Comment by @charliermarsh on 2023-02-28 19:39_

That's really interesting, thank you for following up with all this additional info. Hard for me to say what's going on without digging in a bit more, the exit codes _should_ be the same in both cases.

We could suppress the error message as long as the `E999` error is "expected" to be surfaced to the user. The reason we introduced this separate error message is that if you did something like `ruff --select F841 /path/to/file.py`, and `file.py` contained a syntax error, Ruff would fail silently: we wouldn't be able to lint the file, but you wouldn't know, because `E999` wouldn't be surfaced to you.

> P.S. Great package, and appreciate the fast reply :)

Thank you so much :)

---

_Comment by @biscanli on 2023-02-28 21:57_

> That's really interesting, thank you for following up with all this additional info. Hard for me to say what's going on without digging in a bit more, the exit codes should be the same in both cases.

That is my experience as well. I tested this with two files, one as
```
def
```
and the other as
```
import b
import a
```
to get some import errors. I ran `ruff check --quiet <filename.py>` on both, saw the proper rule violations. Both returned status code `1` when I checked with `echo $?` afterwards. Would there be any other information that `ruff` returns during the process that a sentinel or any other process watcher can observer? My thought was maybe the error triggers something _before_ the rules are returned when it is listening for `stdin` with `--stdin-filename=stdin -`. Or maybe when it acts as a stream like this, it terminates before the `flymake` process can collect the rule violation and process it? I couldn't find how to test this theory via `bash`, nor how to send EOF to a running `ruff --quiet --stdin-filename=stdin -` instance.

> The reason we introduced this separate error message is that if you did something like ruff --select F841 /path/to/file.py, and file.py contained a syntax error, Ruff would fail silently: we wouldn't be able to lint the file, but you wouldn't know, because E999 wouldn't be surfaced to you.

I think this makes sense. I guess another alternative would be to not return it if rule `E999` is on, but it does feel like patching the symptom rather than fixing it.

---

_Comment by @biscanli on 2023-03-13 12:04_

FYI I wanted to drop an update here:

After digging around on the Emacs side of things, I found a [package](https://github.com/erickgnavar/flymake-ruff) that implements `ruff` different than how I did it. This fixed the problem of constant error outs. Now, there is another issue with the process sometimes silently dying with the scenario I mentioned above, where there is a syntax error and a ruff parsing error. However, I did not experience the same issue on a fresh Emacs install so its probably something within my config messing up, and is not relevant to this project.

---

_Closed by @biscanli on 2023-03-13 12:04_

---

_Comment by @doolio on 2023-10-30 17:14_

@biscanli Just to say I only get the following error reported by ruff v0.1.3 using the simple test case suggested in your OP and simply configuring ruff as follows i.e. not using any (Emacs) LSP client and Emacs 27.1/flymake v1.3.6:

````emacs-lisp
 (setq python-flymake-command '("ruff" "--quiet" "--stdin-filename=stdin" "-"))
````

```
a.py 1 error   E999 SyntaxError: Expected 'name', but got Newline
```

The error message is slightly different so perhaps some changes to ruff improved the situation since you raised this issue.

@charliermarsh The `setq` above is a setting within the Emacs built-in `python.el` mode. So there is no need for users to use the third party `flymake-ruff` package you mention [here](https://docs.astral.sh/ruff/integrations/#emacs-unofficial) if they don't want to. Though it may offer other niceties. IIUC (I'm just learning to program), the intention of that third party package is to use `ruff` via `flymake` when not using an LSP client/server. I believe (but still need to test myself) the different Emacs LSP clients have mechanisms to utilise `ruff` even if not using the `ruff-lsp` server. Happy to submit a PR for consideration to improve the docs.

---
