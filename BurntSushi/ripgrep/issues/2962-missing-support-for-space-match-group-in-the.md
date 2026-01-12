```yaml
number: 2962
title: "Missing support for `[[:space:]]` match group in the ignore crate"
type: issue
state: open
author: weiznich
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-01-06T14:08:16Z
updated_at: 2025-10-28T23:01:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2962
synced_at: 2026-01-12T16:13:25Z
```

# Missing support for `[[:space:]]` match group in the ignore crate

---

_@weiznich_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ignore = "0.4.23"

### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

Fedora

```
NAME="Fedora Linux"
VERSION="41 (Workstation Edition)"
RELEASE_TYPE=stable
ID=fedora
VERSION_ID=41
VERSION_CODENAME=""
PLATFORM_ID="platform:f41"
PRETTY_NAME="Fedora Linux 41 (Workstation Edition)"
ANSI_COLOR="0;38;2;60;110;180"
LOGO=fedora-logo-icon
CPE_NAME="cpe:/o:fedoraproject:fedora:41"
DEFAULT_HOSTNAME="fedora"
HOME_URL="https://fedoraproject.org/"
DOCUMENTATION_URL="https://docs.fedoraproject.org/en-US/fedora/f41/system-administrators-guide/"
SUPPORT_URL="https://ask.fedoraproject.org/"
BUG_REPORT_URL="https://bugzilla.redhat.com/"
REDHAT_BUGZILLA_PRODUCT="Fedora"
REDHAT_BUGZILLA_PRODUCT_VERSION=41
REDHAT_SUPPORT_PRODUCT="Fedora"
REDHAT_SUPPORT_PRODUCT_VERSION=41
SUPPORT_END=2025-12-15
VARIANT="Workstation Edition"
VARIANT_ID=workstation

```

### Describe your bug.

The ignore crate fails to handle certain character classes as part of it's matcher implementation. I noticed this for `[[:space:]]` which happens to be contained in some local `.gitattributes` file I try to parse and use via the ignore crate to teach [`jj'](https://github.com/jj-vcs/jj/) to just ignore git-lfs files. Git itself [documents the pattern syntax](https://git-scm.com/docs/gitattributes) to be the same (beside minor restrictions) than that one from `.gitignore` files, therefore I've tried to use the ignore crate for this.

### What are the steps to reproduce the behavior?

Run the following code and see the assertion fail:

```rust
    let mut ignore_builder = ignore::gitignore::GitignoreBuilder::new("");

    ignore_builder.add_line(None, "giga_las/samples/MOAT[[:space:]]HOUSE[[:space:]]FARM[[:space:]]BH_Raw1[[:space:]](Disks).las").unwrap();
    ignore_builder
        .add_line(None, "giga_las/samples/MOAT_HOUSE_FARM_BH_Raw1_(Disks).las")
        .unwrap();

    let ignore = ignore_builder.build().unwrap();

    assert!(matches!(
        ignore.matched(
            "giga_las/samples/MOAT_HOUSE_FARM_BH_Raw1_(Disks).las",
            false
        ),
        ignore::Match::Ignore(_)
    ));

    assert!(
        matches!(
            ignore.matched(
                "giga_las/samples/MOAT HOUSE FARM BH Raw1 (Disks).las",
                false
            ),
            ignore::Match::Ignore(_)
        ),
        "Did not match, did not satisfy the [[:space:]] matchers"
    );
```

### What is the actual behavior?

The assertion fails

### What is the expected behavior?

The assertion passes. See the character class tests from the git repository itself here: https://github.com/git/git/blob/8d8387116ae8c3e73f6184471f0c46edbd2c7601/t/t3070-wildmatch.sh#L144 for future examples

---

_Comment by @BurntSushi on 2025-01-06 14:13_

The only `[[:space:]]` I see on the [`gitattributes` docs](https://git-scm.com/docs/gitattributes) is in a _regex_, not a glob.

Now, the tests you link do seem to _suggest_ that `[[:space:]]` and the like are supported in globs as well, but I can't tell for sure.

If git supports this syntax, then I'm probably open to supporting it as well. But I probably won't be adding it any time soon.

---

_Comment by @BurntSushi on 2025-01-06 14:14_

It might help if you can find some git docs for the specific glob pattern syntax that is supported.

---

_Label `question` added by @BurntSushi on 2025-01-06 14:14_

---

_Comment by @weiznich on 2025-01-06 15:12_

I've not found a documentation entry for this, but a quick test with git 2.47.1 with the following commands indicate that this also seems to work for `.gitignore` files:

```sh
echo "/foo[[:space:]]bar.txt" >> .gitignore
git add .gitignore
git commit -m "Add [[:space:]] matcher"
touch foo\ bar.txt
git status
# foo\ bar.txt is not listed by git status
```

---

_Comment by @BurntSushi on 2025-01-06 16:00_

Blech. Glob implementations are truly the wild west. I don't think I've ever seen that syntax in a glob before.

---

_Comment by @okdana on 2025-01-06 16:38_

it's specified in posix that shell patterns, `fnmatch(3)`, etc. support character classes the same as in regular expressions:

> A \<left-square-bracket> shall introduce a bracket expression if the characters following it meet the requirements for bracket expressions stated in XBD [9.3.5 RE Bracket Expression](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap09.html#tag_09_03_05)

bash and zsh support for character classes is described here:

* https://www.gnu.org/software/bash/manual/bash.html#Pattern-Matching
* https://zsh.sourceforge.io/Doc/Release/Expansion.html#Glob-Operators

the `gitignore` documentation doesn't explicitly say anything about it, but it strongly implies that it uses `fnmatch(3)`:

> An asterisk "`*`" matches anything except a slash. The character "`?`" matches any one character except "`/`". The range notation, e.g. `[a-zA-Z]`, can be used to match one of the characters in a range. See `fnmatch(3)` and the `FNM_PATHNAME` flag for a more detailed description.

it doesn't, though. it uses a modified version of rsync's `wildmatch()`: https://github.com/git/git/blob/master/wildmatch.c

which i guess is good because otherwise it would be at the mercy of platform-specific inconsistencies like this (from freebsd and macos `fnmatch(3)`):

> The current implementation of the fnmatch() function does not conform to IEEE Std 1003.2 (“POSIX.2”).  Collating symbol expressions, equivalence class expressions and character class expressions are not supported.

---

_Label `question` removed by @BurntSushi on 2025-01-06 17:37_

---

_Label `enhancement` added by @BurntSushi on 2025-01-06 17:37_

---

_Comment by @BurntSushi on 2025-01-06 17:38_

Fair enough. I'm fine with adding stuff like this, but I draw the line at locale related shenanigans.

---

_Label `help wanted` added by @BurntSushi on 2025-09-23 01:33_

---

_Comment by @matanshavit on 2025-10-28 22:08_

I would like to work on this issue

---

_Comment by @matanshavit on 2025-10-28 23:01_

Is this in line with what you were thinking? https://github.com/BurntSushi/ripgrep/pull/3210

---
