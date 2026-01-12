```yaml
number: 2227
title: Ripgrep13 times out on simple search. Ripgrep12 works like a champ.
type: issue
state: closed
author: firepick1
labels: []
assignees: []
created_at: 2022-06-03T13:21:15Z
updated_at: 2022-06-03T13:46:18Z
url: https://github.com/BurntSushi/ripgrep/issues/2227
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep13 times out on simple search. Ripgrep12 works like a champ.

---

_@firepick1_

#### What version of ripgrep are you using?
13.0.0

We are using 12.1.1 since 13.0.0 doesn't work

#### How did you install ripgrep?
RQDVER=13.0.0
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/${RQDVER}/ripgrep_${RQDVER}_amd64.deb                                                            
sudo dpkg -i ripgrep_${RQDVER}_amd64.deb 

If you installed ripgrep with snap and are getting strange file permission or
file not found errors, then please do not file a bug. Instead, use one of the
Github binary releases.

#### What operating system are you using ripgrep on?
Chromebook Pixel Linux [sudo lsb_release -a]

Distributor ID: Debian
Description:    Debian GNU/Linux 10 (buster)
Release:        10
Codename:       buster

#### Describe your bug.
Invoking following command from NodeJS 16 or 18 works fine on rg12.1.1 but times out on rg13.0.0
```
  rg -c -e "root of suffering"
```

From shell command prompt, both rg12 and rg13 work as expected.

Invoking following command does work from any NodeJS, however:
```
  rg --version
```
#### What are the steps to reproduce the behavior?
See https://github.com/sc-voice/rg-bug

#### What is the actual behavior?
The test times out after 5 seconds. Increasing the timeout does not affect outcome. Here is the stdout and stderr from exec():


```
DEBUG|rg::config|crates/core/config.rs:40: /home/karl/pixel/ripgreprc: arguments loaded from config file: ["--glob=!git/*", "--glob=!local/*", "--glob=!dis
t/*", "--glob=!**/lib/", "--glob=!*min.css", "--glob=!node_modules/*", "--glob=!package-lock.json"]\n' +                                                               
      'DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--glob=!git/*", "--glob=!local/*", "--glob=!dist/*", "--glob=!**/lib/", "--glob=!*min.css", "--glob=!
node_modules/*", "--glob=!package-lock.json", "-c", "--debug", "-e", "root of suffering"]\n' +
      'DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(root of suffering)], limit_size: 250, limit_class
: 10 }\n' +
      'DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes\n'
+          
      `DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "git/*", re: "(?-u)^git/[^/]*$", opts: GlobOptions { case_insensitive: false, 
literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('g'), Literal('i'), Literal('t'), Literal('/'), ZeroOrMore]) }\n` +
      `DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "local/*", re: "(?-u)^local/[^/]*$", opts: GlobOptions { case_insensitive: fal
se, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('l'), Literal('o'), Literal('c'), Literal('a'), Literal('l'), Literal('/'), ZeroOrMore]) 
}\n` +  
      `DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "dist/*", re: "(?-u)^dist/[^/]*$", opts: GlobOptions { case_insensitive: false
, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('d'), Literal('i'), Literal('s'), Literal('t'), Literal('/'), ZeroOrMore]) }\n` +
      `DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "node_modules/*", re: "(?-u)^node_modules/[^/]*$", opts: GlobOptions { case_in
sensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('n'), Literal('o'), Literal('d'), Literal('e'), Literal('_'), Literal('m'),
 Literal('o'), Literal('d'), Literal('u'), Literal('l'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore]) }\n` +
      'DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 2 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 4 regexes\n'
```

#### What is the expected behavior?

What do you think ripgrep should have done?
ripgrep 13 should work just like ripgrep 12 and return a result quickly.

_"Aye, the haggis is in the fire for sure." (TOS: "A Taste of Armageddon")_


---

_Comment by @BurntSushi on 2022-06-03 13:31_

Thanks for the excellent reproduction. This patch fixes the issue for you:

```
$ git diff
diff --git a/test/rg.js b/test/rg.js
index b799063..5a7f1e1 100644
--- a/test/rg.js
+++ b/test/rg.js
@@ -46,6 +46,7 @@
       //`--threads 1`,      // DOES NOT HELP
       //`--with-filename`,  // DOES NOT HELP
       `-e '${pattern}'`,
+      `./`,
     ].join(' ');
     var execOpts = {
         cwd,
```

Basically, the problem here is likely because Node is attaching something to ripgrep's stdin even though there's nothing there. Since you didn't tell ripgrep to search anything, it has to "auto detect" whether to search the CWD or stdin. Since there's something on stdin, that's what it chooses. But since Node attaches something to stdin that has no data and is never closed, ripgrep never stops searching it. So this is really a Node problem.

The reason why ripgrep's behavior changed here was because it's a result of fixing another bug: https://github.com/BurntSushi/ripgrep/commit/020c5453a5880257c87949030550d24c596f5c8d

In any case, _many_ people have reported this exact same bug. The issue with the biggest discussion on this problem is #1892.

---

_Closed by @BurntSushi on 2022-06-03 13:31_

---

_Comment by @firepick1 on 2022-06-03 13:46_

Thanks, Andrew!

I'm blown away by how simple that was. Yay rg13!!!



---

_Closed by @firepick1 on 2022-06-03 13:46_

---
