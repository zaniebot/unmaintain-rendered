```yaml
number: 2823
title: Potentially incorrect or undocumented .gitignore files handling
type: issue
state: closed
author: fabiospampinato
labels: []
assignees: []
created_at: 2024-05-27T18:31:56Z
updated_at: 2024-05-27T19:01:00Z
url: https://github.com/BurntSushi/ripgrep/issues/2823
synced_at: 2026-01-12T16:13:25Z
```

# Potentially incorrect or undocumented .gitignore files handling

---

_@fabiospampinato_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

13.0.0

### How did you install ripgrep?

brew install ripgrep

### What operating system are you using ripgrep on?

macOS 12.5.1

### Describe your bug.

I'm trying to understand how `.gitignore` files are handled.

Let's say I have the following structure:

```
<cwd>
├── .gitignore
└── deep
   ├── deep
   │  └── deeper
   │     └── bar.txt
   └── deeper
      └── foo.txt
```

And this is the content of my `.gitignore` file:

```
deep/deeper
```

According to my understanding of the [spec](https://git-scm.com/docs/gitignore) that pattern is relative to the directory where the `.gitignore` file exists in, so it should match `<cwd>/deep/deeper`, so I'd expect `<cwd>/deep/deeper/foo.txt` to get ignored.

Yet if I run this command while at the `<cwd>`:

```sh
rg deep --files
```

I get the following output, basically the `<cwd>/deep/deeper` directory wasn't ignored:

```
deep/deep/deeper/bar.txt
deep/deeper/foo.txt
```

So I'm not quite understanding how that works in `ripgrep`, because:
1. if we are checking for the relative paths of files and directories, relative to the directory where that `.gitignore` file was found in, which I think is what the spec intended for it to happen, then presumably `<cwd>/deep/deeper` would be ignored, and `<cwd>/deep/deeper/foo.txt` wouldn't be listed by the `rg` command I'm running, so that doesn't seem to be the case here. Possibly for good reasons, otherwise if we are ignoring for example `foo`, but then the user runs a search inside `<cwd>/foo`, `rg` wouldn't find anything, which is most probably not what the user wanted, although in this case I'm excluding a directory _inside_ the directory I'm searching into, so perhaps that should still be excluded?
2. if we are matching the rules in the `.gitignore` file against relative paths that are relative to our root search directory, which in this case is `<cwd>/deep`, then presumably `<cwd>/deep/deep/deeper` should have been excluded, but it isn't, so that's not what's happening either.

I guess the more specific questions are:
1. Are `.gitignore` files handled according to the spec?
2. If yes, then what strings is `ripgrep` actually trying to check if they are ignored by that `.gitignore` file?
3. If not, is this documented anywhere? I couldn't find more info about it.
4. Which hypothetical paths would a rule like "deep/deeper" actually ignore, according to `ripgrep`? And if the answer is "no paths" are we sure that's right?

### What are the steps to reproduce the behavior?

```
mkdir -p test/deep/deeper
touch test/deep/deeper/foo.txt     

mkdir -p test/deep/deep/deeper
touch test/deep/deep/deeper/bar.txt

echo 'deep/deeper' >> .gitignore

cd test        
rg deep --files
```

### What is the actual behavior?

`deep/deeper/foo.txt` is not ignored, and more generally `ripgrep` doesn't seem to be following the spec in ways that seem non-obvious.

### What is the expected behavior?

`deep/deeper/foo.txt` to be ignored, and more generally handling `.gitignore` files to be less surprising, if it makes sense.

---

_Locked by @ghost on 2024-05-27 19:00_

---
