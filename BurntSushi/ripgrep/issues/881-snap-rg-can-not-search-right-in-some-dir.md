```yaml
number: 881
title: snap rg can not search right in some dir
type: issue
state: closed
author: fcying
labels:
  - invalid
  - question
assignees: []
created_at: 2018-04-11T13:03:13Z
updated_at: 2019-07-01T21:38:43Z
url: https://github.com/BurntSushi/ripgrep/issues/881
synced_at: 2026-01-12T16:13:22Z
```

# snap rg can not search right in some dir

---

_@fcying_

#### What version of ripgrep are you using?
snap install
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### What operating system are you using ripgrep on?
ubuntu 16.04

in some dir, rg install by snap work abnormality 
version ripgrep-0.8.1-x86_64-unknown-linux-musl.tar.gz work fine. 

```
ll                                                                                                                                             
total 16
drwxrwxr-x  2 fcying fcying 4096 Apr 11 14:52 ./
drwxrwxr-x 11 fcying fcying 4096 Apr 11 20:49 ../
-rwxrwxr-x  1 fcying fcying 3780 Apr 11 14:52 vim_build.sh*
-rwxrwxr-x  1 fcying fcying 1003 Dec  5 23:31 ycm_build.sh*
```

`snap rg 0.8.1`   work abnormality 
```
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX
/snap/bin/rg "dirname" --debug
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'd',
        'i',
        'r',
        'n',
        'a',
        'm',
        'e'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(dirname)], limit_size: 250, limit_class: 10 }
./: Permission denied (os error 13)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug
```

`ripgrep-0.8.1-x86_64-unknown-linux-musl`  work correct
```
ripgrep 0.8.1 (rev c8e9f25b85)
+SIMD -AVX
~/bin/rg "dirname" --debug                                                                                                                       20:59:42
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'd',
        'i',
        'r',
        'n',
        'a',
        'm',
        'e'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(dirname)], limit_size: 250, limit_class: 10 }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "vim*", re: "(?-u)^vim[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([Literal('v'), Literal('i'), Literal('m'), ZeroOrMore]) }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 11 literals, 2 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
ycm_build.sh
3:cd `dirname $0`

vim_build.sh
5:cd `dirname $0`
```

#### If this is a bug, what is the expected behavior?
search  correct



---

_Comment by @BurntSushi on 2018-04-11 13:07_

Please [read the README](https://github.com/BurntSushi/ripgrep#installation) more carefully: 

> If you get permission errors when running ripgrep after installation, try uninstalling and then re-installing with the `--classic` flag.

Moreover, I don't maintain the ripgrep snap, so there's nothing that can be done about it here.

---

_Closed by @BurntSushi on 2018-04-11 13:07_

---

_Label `invalid` added by @BurntSushi on 2018-04-11 13:07_

---

_Label `question` added by @BurntSushi on 2018-04-11 13:07_

---

_Comment by @Dieff on 2019-07-01 21:38_

I had a similar problem. It was due to a non-default home directory. If your `$HOME` isn't `/home/<username>`, then the default Apparmor policies used by Snap may be causing the issue.

I fixed by
- Edit the file `/etc/apparmor.d/tunables/home`
- Add the location of the parent of your homedir to the line `@{HOMEDIRS}=/home/`, seperated by a space
- Example, if your home dir is /home/mydomain.com/bob, you would edit the line to
```
@{HOMEDIRS}=/home/ /home/mydomain.com/
```
- Reinstall ripgrep with `--classic` (may not actually be necessary).

Probably a bug should be opened with Ubuntu or Snap. I found this issue with Google, so I think that others will too.

---
