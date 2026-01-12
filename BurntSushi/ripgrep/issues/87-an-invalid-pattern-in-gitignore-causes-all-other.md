```yaml
number: 87
title: an invalid pattern in gitignore causes all other patterns to be ignored
type: issue
state: closed
author: ahmedelgabri
labels:
  - bug
assignees: []
created_at: 2016-09-25T17:52:16Z
updated_at: 2016-10-11T08:02:12Z
url: https://github.com/BurntSushi/ripgrep/issues/87
synced_at: 2026-01-12T18:23:11Z
```

# an invalid pattern in gitignore causes all other patterns to be ignored

---

_@ahmedelgabri_

I have this line in my project `.gitignore` but when I do search for a string it still returns results from these paths. While `ag` respect the ignored paths & doesn't show any results from there. Maybe I'm missing something here?

```
public/assets/admin
```

Here the commands

``` shell
$ rg -w -tjs document # not very creative :)
$ ag --js "\bdocument\b"
```


---

_Comment by @BurntSushi on 2016-09-25 17:59_

I can't seem to reproduce the problem, could you please give a more detailed test case?

```
[andrew@Cheetah 87] echo public/assets/admin > .gitignore
[andrew@Cheetah 87] mkdir -p public/assets/admin
[andrew@Cheetah 87] echo document > public/assets/admin/foo
[andrew@Cheetah 87] tree
.
└── public
    └── assets
        └── admin
            └── foo

3 directories, 1 file
[andrew@Cheetah 87] rg -w document
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

Note that I've fixed a few bugs with `.gitignore` on master that haven't made it into a release yet. It doesn't look like this particular case should have been impacted, but I kind of suspect there are some details missing. :-)


---

_Comment by @BurntSushi on 2016-09-25 17:59_

Also, please show the output of `rg` with the `--debug` flag.


---

_Comment by @ahmedelgabri on 2016-09-25 18:25_

Replicating your example everything works but in my repo the result is different 

It's a big repo & the output with `--debug` is really big but here is one line from the debug that shows the file was _whitelisted_ by pattern which I assume is the `-tjs` part.

```
DEBUG:rg::ignore: ./public/assets/admin/application.js whitelisted by Pattern { from: "<filetype>", original: "<N/A>", pat: "<N/A>", whitelist: false, only_dir: false }
```

But, I commend out my `.gitignore` file leaving only the `public/assets/admin` & it didn't show anything from there. So it seems like it's the project's `.gitignore` issue, but I'm trying to wrap my head around it since there is only `public/assets/admin` in that file.


---

_Comment by @BurntSushi on 2016-09-25 18:28_

Is there any way for you to try current master? It would require installing Rust and compiling ripgrep (instructions in the README).

If not, that's OK, but I'd suggest we stop debugging the issue until I get the next release out for you to try (which should be today). I say this because I've fixed a few bugs related to gitignore.


---

_Comment by @ahmedelgabri on 2016-09-25 18:32_

Ok I'll wait for the new release then. Thanks anyway 👍 


---

_Label `question` added by @BurntSushi on 2016-09-25 22:35_

---

_Comment by @BurntSushi on 2016-09-26 02:36_

I just tagged a new release, `0.2.0`. CI needs to run, but it should show up soon.


---

_Comment by @BurntSushi on 2016-09-26 02:53_

Looks like they're up. The `brew` formula has been updated.


---

_Comment by @ahmedelgabri on 2016-09-26 22:18_

Here is another test after installing the latest version, same repo different string:

``` shell
$ brew rm ripgrep && brew install https://raw.githubusercontent.com/BurntSushi/ripgrep/master/pkg/brew/ripgrep.rb

$ rg -w -tjs "selectize" -c
app/admin/assets/javascripts/application.js:1
app/admin/assets/javascripts/lib/address.js:2
app/admin/assets/javascripts/view/selectize.js:13
app/admin/assets/javascripts/view/tables.js:1
app/services/assets/javascripts/fair.js:1
app/services/assets/javascripts/signup.js:1
app/services/assets/javascripts/input.js:1
node_modules/selectize/dist/js/selectize.js:22
node_modules/selectize/dist/js/standalone/selectize.js:23
node_modules/selectize/examples/js/index.js:1
node_modules/selectize/Gruntfile.js:27
node_modules/selectize/dist/js/selectize.min.js:3
node_modules/selectize/karma.conf.js:4
node_modules/selectize/src/.wrapper.js:1
node_modules/selectize/src/defaults.js:4
node_modules/selectize/src/plugins/drag_drop/plugin.js:1
node_modules/selectize/src/plugins/dropdown_header/plugin.js:5
node_modules/selectize/src/plugins/optgroup_columns/plugin.js:1
node_modules/selectize/src/plugins/remove_button/plugin.js:1
node_modules/selectize/src/plugins/restore_on_backspace/plugin.js:1
node_modules/selectize/src/selectize.jquery.js:7
node_modules/selectize/src/selectize.js:6
node_modules/selectize/test/events_dom.js:3
node_modules/selectize/dist/js/standalone/selectize.min.js:3
node_modules/selectize/test/events.js:57
node_modules/selectize/test/setup.js:37
node_modules/selectize/test/support/base.js:3
node_modules/selectize/test/interaction.js:73
node_modules/selectize/test/api.js:204
node_modules/selectize/test/xss.js:2
public/assets/services/fair.js:2
public/assets/services/signup.js:2
public/assets/admin/application.js:4

# with ag
$ ag --js "\bselectize\b" -l
app/admin/assets/javascripts/application.js
app/admin/assets/javascripts/lib/address.js
app/admin/assets/javascripts/view/selectize.js
app/admin/assets/javascripts/view/tables.js
app/services/assets/javascripts/fair.js
app/services/assets/javascripts/signup.js
app/services/assets/javascripts/input.js
```

`node_modules` is in the `.gitignore` too, the `--debug` output is way too big because it enters `node_modules` but simple all line are showing this

```
DEBUG:rg::ignore: ./node_modules/xregexp/src/addons/matchrecursive.js whitelisted by Pattern { from: "<filetype>", original: "<N/A>", pat: "<N/A>", whitelist: false, only_dir: false }
```

Simply any file that matches `-tjs` even if it is ignored shows as `whitelisted by Pattern`


---

_Comment by @BurntSushi on 2016-09-26 22:51_

I still can't reproduce this. I tried running `rg` in my own Javascript repo (with a `node_modules` directory) using `-tjs` and I get expected results, with `node_modules` ignored.

Is there anyway for you to pair down your `.gitignore` to find a smaller test case? I know that's probably asking a lot, since I expect that might take some time, so I understand if you can't. But it would be very very helpful. :-)


---

_Comment by @ahmedelgabri on 2016-09-27 08:38_

I found the problem! it's in our `.gitignore` which is this line `**no-vcs**` commenting this out makes `rg` works as expected. Not sure if this a bug in `rg` or an issue with the `.gitignore` pattern. 


---

_Comment by @BurntSushi on 2016-09-27 11:59_

@ahmedelgabri Oh! That will do it. The actual problem is that `**no-vcs**` actually isn't a valid `gitignore` pattern. From `man gitignore`:

```
      Two consecutive asterisks ("**") in patterns matched against full
       pathname may have special meaning:

       ·   A leading "**" followed by a slash means match in all directories.
           For example, "**/foo" matches file or directory "foo" anywhere, the
           same as pattern "foo". "**/foo/bar" matches file or directory "bar"
           anywhere that is directly under directory "foo".

       ·   A trailing "/**" matches everything inside. For example, "abc/**"
           matches all files inside directory "abc", relative to the location
           of the .gitignore file, with infinite depth.

       ·   A slash followed by two consecutive asterisks then a slash matches
           zero or more directories. For example, "a/**/b" matches "a/b",
           "a/x/b", "a/x/y/b" and so on.

       ·   Other consecutive asterisks are considered invalid.
```

`rg` actually detects that this is an invalid pattern, which is fine, but then it completely drops every other pattern in the file, which is bad. (And it doesn't tell you that it did this either.) So this one pattern essentially killed your entire `.gitignore`. :-)

To clarify, the correct way to write that pattern is `**/no-vcs/**`. Or, if you want to match any directory name with `no-vcs` in it, then, `**/*no-vcs*/**`.


---

_Label `bug` added by @BurntSushi on 2016-09-27 11:59_

---

_Label `question` removed by @BurntSushi on 2016-09-27 11:59_

---

_Renamed from "Doesn't respect ignored paths in .ignore or .gitignore?" to "an invalid pattern in gitignore causes all other patterns to be ignored" by @BurntSushi on 2016-09-27 12:00_

---

_Comment by @BurntSushi on 2016-09-27 12:00_

Thank you so much for looking into this!


---

_Comment by @ahmedelgabri on 2016-09-27 12:04_

Yeah I'm working with person who added this & we are fixing this, thank you for the clarification! 


---

_Comment by @ahmedelgabri on 2016-09-27 12:10_

Here is a couple of ideas on how I'm thinking about dealing with this

1- ignore the faulty pattern & respect the rest _(maybe show a warning message too)_
2- show a warning message directly & exit the search


---

_Comment by @BurntSushi on 2016-09-27 12:26_

@ahmedelgabri (1) is absolutely the way to go. We should dump the error message from parsing the pattern to the debug output (which is only shown when `--debug` is passed). The specific place in the code where this needs to happen is in [both of these methods](https://github.com/BurntSushi/ripgrep/blob/67abbf6f22019f353f44bfb04277c69a15d2d6ae/src/gitignore.rs#L262-L276). Specifically, using `try!` causes the method to stop and report the error. Instead, we should extract the error explicitly, call `debug!("{}", err)` and keep on processing.

I think this means that `add_str` can't ever fail, but `add_path` can still fail if it can't read the file. That's OK because sometimes the file doesn't exist. This [fact is used](https://github.com/BurntSushi/ripgrep/blob/67abbf6f22019f353f44bfb04277c69a15d2d6ae/src/ignore.rs#L359-L372) to avoid building a matcher for a particular directory if the file doesn't exist.

I'd be happy to mentor this change (even if you don't know Rust). Otherwise, I'm also happy to just fix it. :-)


---

_Comment by @ahmedelgabri on 2016-09-27 12:47_

I have never written a single Rust line in my life, I don't mind trying 

But I'll need help starting from setting up my env to maybe even the smallest of stuff 😬


---

_Comment by @BurntSushi on 2016-09-27 12:48_

The [Rust book](https://doc.rust-lang.org/book/) should help get you started!


---

_Comment by @ahmedelgabri on 2016-09-27 12:51_

Thanks!


---

_Comment by @BurntSushi on 2016-09-29 00:38_

@ahmedelgabri Let me know if there's anything I can do to help. :-)


---

_Comment by @ahmedelgabri on 2016-09-30 08:28_

@BurntSushi sorry didn't have enough time to look into this yet, will see if I can do it over the weekend


---

_Closed by @BurntSushi on 2016-10-10 23:27_

---

_Comment by @BurntSushi on 2016-10-10 23:28_

@ahmedelgabri I was touching this code and wanted to get this bug fixed, so I went ahead and did it. If you'd like to work on something else, let me know and I can try to help you get started!


---

_Comment by @ahmedelgabri on 2016-10-11 08:02_

@BurntSushi I'm very sorry. I'm extremely busy these days, didn't have time to do anything. Thanks for fixing this & I'll try to look for something else that I can help with when I have some time. 


---
