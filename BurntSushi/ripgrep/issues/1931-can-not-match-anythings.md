```yaml
number: 1931
title: Can not match anythings
type: issue
state: closed
author: WingDust
labels:
  - question
assignees: []
created_at: 2021-07-10T10:58:00Z
updated_at: 2021-07-10T11:50:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1931
synced_at: 2026-01-12T16:13:24Z
```

# Can not match anythings

---

_@WingDust_

#### What version of ripgrep are you using?
13.0.0
Replace this text with the output of `rg --version`.

#### How did you install ripgrep?
scoop

If you installed ripgrep with snap and are getting strange file permission or
file not found errors, then please do not file a bug. Instead, use one of the
Github binary releases.

#### What operating system are you using ripgrep on?
windows10
Replace this text with your operating system and version.
19043.1083
#### Describe your bug.

Give a high level description of the bug.
Can not match anythings
#### What are the steps to reproduce the behavior?

in [Rustexp](https://rustexp.lpil.uk/)
 I use Regex :`(?U)/\*\\[\s\S]+\\\*/` to match 
Subject:
```text
qweqwe
/*\ 1
|*|  2
|*|  3
|*|  4
|*|  5
|*|  6
\*/ 7
end
222
/*\ 1
|*|  2
|*|  3
|*|  4
|*|  5
|*|  6
\*/ 7
1233
```
Get:
```text
Some(Captures({
    0: Some("/*\\ 1\n|*|  2\n|*|  3\n|*|  4\n|*|  5\n|*|  6\n\\*/"),
})),
Some(Captures({
    0: Some("/*\\ 1\n|*|  2\n|*|  3\n|*|  4\n|*|  5\n|*|  6\n\\*/"),
})),
```
Result is what I expect
But When I call `rg  '(?U)/\*\\[\s\S]+\\\*/'` in local
I have a file with
```ts
/*\
|*|
|*| 12132
|*|
\*/

/*\
|*| 11
|*| 11
|*| 11
\*/
```
**Get nothing !**

#### What is the actual behavior?
`rg '(?U)/\*\\[\s\S]+\\\*/' --debug`
Show:
```text
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:180: required literal found: "/*\\\\"
DEBUG|grep_regex::matcher|crates\regex\src\matcher.rs:50: extracted fast line regex: (?-u:/\x2a\x5c)
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 0 basenames, 7 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 0 basenames, 7 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 2 literals, 3 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./.env: Ignore(IgnoreMatch(Hidden))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./.vscode: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./.yarnrc: Ignore(IgnoreMatch(Hidden))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./dist: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "dist", actual: "**/dist", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./node_modules: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "node_modules", actual: "**/node_modules", is_whitelist: false, is_only_dir: false })))
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 12 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./zju-course-scene-change-detection\.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./zju-course-scene-change-detection\.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Style\node_modules: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "node_modules", actual: "**/node_modules", is_whitelist: false, is_only_dir: false })))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 0 basenames, 7 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./src\render\dist: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "dist", actual: "**/dist", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./src\main\.env.development: Ignore(IgnoreMatch(Hidden))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\VideoScale\.vscode: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|crates\globset\src\lib.rs:416: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\keyevent\pkg\.gitignore: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./Legacy\\keyevent\\pkg\\.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\keyevent\pkg\keyevent.d.ts: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./Legacy\\keyevent\\pkg\\.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\keyevent\pkg\keyevent.js: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./Legacy\\keyevent\\pkg\\.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\keyevent\pkg\keyevent_bg.js: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./Legacy\\keyevent\\pkg\\.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\keyevent\pkg\keyevent_bg.wasm: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./Legacy\\keyevent\\pkg\\.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\keyevent\pkg\keyevent_bg.wasm.d.ts: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./Legacy\\keyevent\\pkg\\.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 0 basenames, 7 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\keyevent\pkg\package.json: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./Legacy\\keyevent\\pkg\\.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|ignore::walk|crates\ignore\src\walk.rs:1741: ignoring ./Legacy\keyevent\pkg\README.md: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./Legacy\\keyevent\\pkg\\.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 0 basenames, 7 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```


#### What is the expected behavior?
Should match 
```text
filename
linenumber:/*\
linenumber:|*|
linenumber:|*| 12132
linenumber:|*|
linenumber:\*/

filename
linenumber:/*\
linenumber:|*| 11
linenumber:|*| 11
linenumber:|*| 11
linenumber:\*/
```
What do you think ripgrep should have done?


---

_Comment by @BurntSushi on 2021-07-10 11:06_

Works just fine here. First test:

```
$ cat subject1
qweqwe
/*\ 1
|*|  2
|*|  3
|*|  4
|*|  5
|*|  6
\*/ 7
end
222
/*\ 1
|*|  2
|*|  3
|*|  4
|*|  5
|*|  6
\*/ 7
1233

$ rg -U '(?U)/\*\\[\s\S]+\\\*/' subject1
2:/*\ 1
3:|*|  2
4:|*|  3
5:|*|  4
6:|*|  5
7:|*|  6
8:\*/ 7
11:/*\ 1
12:|*|  2
13:|*|  3
14:|*|  4
15:|*|  5
16:|*|  6
17:\*/ 7
```

Second test:

```
$ cat subject2
/*\
|*|
|*| 12132
|*|
\*/

/*\
|*| 11
|*| 11
|*| 11
\*/

$ rg -U '(?U)/\*\\[\s\S]+\\\*/' subject2
1:/*\
2:|*|
3:|*| 12132
4:|*|
5:\*/
7:/*\
8:|*| 11
9:|*| 11
10:|*| 11
11:\*/
```

I note that you did not provide a complete reproduction here. In particular, it looks like you're using multiline mode, but your ripgrep commands lack the `-U` flag. This means it's impossible for me to be confident that you're showing me the commands you're actually running. Are you using a config file? Do you have an alias or wrapper script in play here?

---

_Label `question` added by @BurntSushi on 2021-07-10 11:06_

---

_Comment by @WingDust on 2021-07-10 11:48_

@BurntSushi Thanks fast response .

I am new come to ripgrep 
Don't hava any config yet and alias or wrapper script
`rg -U '(?U)/\*\\[\s\S]+\\\*/'` work nice 
![image](https://user-images.githubusercontent.com/45911286/125161910-ac3ee580-e1b7-11eb-93ac-62cf48115167.png)


---

_Closed by @WingDust on 2021-07-10 11:49_

---

_Comment by @BurntSushi on 2021-07-10 11:50_

To be clear, your first query doesn't work for me without `-U` either:

```
$ rg '(?U)/\*\\[\s\S]+\\\*/' subject1
$
```

But anyway, glad you figured it out.

---
