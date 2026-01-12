```yaml
number: 2056
title: rg hangs when called in a child process from node.js
type: issue
state: closed
author: chuanqisun
labels:
  - duplicate
assignees: []
created_at: 2021-11-11T02:07:58Z
updated_at: 2022-01-05T04:35:31Z
url: https://github.com/BurntSushi/ripgrep/issues/2056
synced_at: 2026-01-12T16:13:24Z
```

# rg hangs when called in a child process from node.js

---

_@chuanqisun_

#### What version of ripgrep are you using?

13.0.0
(for reference, 12.1.1 does not have this issue)

#### How did you install ripgrep?

Downloaded the binary from GitHub release. Installed via `dpkg -i`

#### What operating system are you using ripgrep on?

WSL2 `5.10.60.1-microsoft-standard-WSL2 #1 SMP Wed Aug 25 23:20:18 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux`
POP!_OS `5.13.0-7620-generic #20~1634827117~21.04~874b071-Ubuntu SMP Fri Oct 29 15:06:55 UTC x86_64 x86_64 x86_64 GNU/Linux`

#### Describe your bug.

Node.js [child_process.exec](https://nodejs.org/api/child_process.html#child_processexeccommand-options-callback) api allow you to spawn a shell and execute a command. In 12.1.1, rg works as expected. In 13.0.0, the exec never finishes, i.e. the callback is never invoked.

#### What are the steps to reproduce the behavior?

(the repro uses execSync instead of exec, in order to collect error log. exec has no observable output)

1. Ensure you have node v16 (v14) on the system
2. Create the following file `index.js`
   ```javascript
   const { execSync } = require("child_process");
   execSync("rg const --trace");
   ```
3. In the same directory, run `node index.js`
4. Observe console output

```
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(const)], limit_size: 250, limit_class: 10 }
TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:54: final regex: "const"
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:734: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|crates/searcher/src/searcher/core.rs:56: searcher core: will use fast line searcher
node:child_process:903
    throw err;
    ^

Error: Command failed: rg const --trace
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(const)], limit_size: 250, limit_class: 10 }
TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:54: final regex: "const"
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:734: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|crates/searcher/src/searcher/core.rs:56: searcher core: will use fast line searcher

    at checkExecSyncError (node:child_process:826:11)
    at execSync (node:child_process:900:15)
    at Object.<anonymous> (/home/chusun/repos/rg-repro/index.js:3:1)
    at Module._compile (node:internal/modules/cjs/loader:1101:14)
    at Object.Module._extensions..js (node:internal/modules/cjs/loader:1153:10)
    at Module.load (node:internal/modules/cjs/loader:981:32)
    at Function.Module._load (node:internal/modules/cjs/loader:822:12)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:17:47 {
  status: 1,
  signal: null,
  output: [
    null,
    Buffer(0) [Uint8Array] [],
    Buffer(734) [Uint8Array] [
       68,  69,  66,  85,  71, 124, 103, 114, 101, 112,  95, 114,
      101, 103, 101, 120,  58,  58, 108, 105, 116, 101, 114,  97,
      108, 124,  99, 114,  97, 116, 101, 115,  47, 114, 101, 103,
      101, 120,  47, 115, 114,  99,  47, 108, 105, 116, 101, 114,
       97, 108,  46, 114, 115,  58,  53,  56,  58,  32, 108, 105,
      116, 101, 114,  97, 108,  32, 112, 114, 101, 102, 105, 120,
      101, 115,  32, 100, 101, 116, 101,  99, 116, 101, 100,  58,
       32,  76, 105, 116, 101, 114,  97, 108, 115,  32, 123,  32,
      108, 105, 116, 115,
      ... 634 more items
    ]
  ],
  pid: 7293,
  stdout: Buffer(0) [Uint8Array] [],
  stderr: Buffer(734) [Uint8Array] [
     68,  69,  66,  85,  71, 124, 103, 114, 101, 112,  95, 114,
    101, 103, 101, 120,  58,  58, 108, 105, 116, 101, 114,  97,
    108, 124,  99, 114,  97, 116, 101, 115,  47, 114, 101, 103,
    101, 120,  47, 115, 114,  99,  47, 108, 105, 116, 101, 114,
     97, 108,  46, 114, 115,  58,  53,  56,  58,  32, 108, 105,
    116, 101, 114,  97, 108,  32, 112, 114, 101, 102, 105, 120,
    101, 115,  32, 100, 101, 116, 101,  99, 116, 101, 100,  58,
     32,  76, 105, 116, 101, 114,  97, 108, 115,  32, 123,  32,
    108, 105, 116, 115,
    ... 634 more items
  ]
}
```

For comparison. running the command `rg const --trace` in terminal (also 13.0.0)
```
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(const)], limit_size: 250, limit_class: 10 }
TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:54: final regex: "const"
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: index.js: binary detection: BinaryDetection(Quit(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:683: Some("index.js"): searching using generic reader
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:734: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|crates/searcher/src/searcher/core.rs:56: searcher core: will use fast line searcher
index.js
1:const { execSync } = require("child_process");
3:execSync("rg const --trace");
TRACE|rg::search|crates/core/search.rs:334: debug.txt: binary detection: BinaryDetection(Quit(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:683: Some("debug.txt"): searching using generic reader
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:734: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|crates/searcher/src/searcher/core.rs:56: searcher core: will use fast line searcher
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
In terminal, `echo $?` return `0`

Running `node index.js` with rg 12.1.1
```
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(const)], limit_size: 250, limit_class: 10 }
TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:54: final regex: "const"
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:680: Some("index.js"): searching using generic reader
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:729: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|crates/searcher/src/searcher/core.rs:56: searcher core: will use fast line searcher
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

Running `rg const --trace` in terminal with rg 12.1.1
```
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(const)], limit_size: 250, limit_class: 10 }
TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:54: final regex: "const"
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:680: Some("index.js"): searching using generic reader
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:729: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|crates/searcher/src/searcher/core.rs:56: searcher core: will use fast line searcher
DEBUGindex.js
1:const { execSync } = require("child_process");
3:execSync("rg const --trace");
|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

What do you think ripgrep should have done?

Possible a regression introduced by version 13? I'm not a rust expert so no clue on what might have caused it. Some wild guesses:
1. Any changes to how rg exits the process?
2. Any changes to rust threading logic? (e.g. joining child threads before exits)
3. Any changes to how rg communicates with stdout?
4. The fact that node error log is interwoven with rg trace might say something.


---

_Comment by @BurntSushi on 2021-11-11 13:34_

This is likely a dupe of #1892.

Note that the man page says:

> ripgrep will automatically detect if stdin exists and search stdin for a regex pattern, e.g. `ls | rg foo`. In some environments, stdin may exist when it shouldn’t. To turn off stdin detection explicitly specify the directory to search, e.g. `rg foo ./`.

This is likely relevant to you. There are likely also parameters you can pass to Node's "create process" API that will close off stdin and not make it look readable.

---

_Closed by @BurntSushi on 2021-11-11 13:34_

---

_Label `duplicate` added by @BurntSushi on 2021-11-11 13:34_

---

_Comment by @chuanqisun on 2022-01-05 04:35_

Just to confirm, adding `./` at the end of the command fixed the problem. Thanks again for the pointer!

---
