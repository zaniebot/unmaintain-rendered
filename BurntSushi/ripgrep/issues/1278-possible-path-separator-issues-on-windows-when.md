```yaml
number: 1278
title: Possible path-separator issues on windows when invoked from git-bash
type: issue
state: open
author: markhepburn
labels: []
assignees: []
created_at: 2019-05-10T23:56:28Z
updated_at: 2019-05-14T00:09:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1278
synced_at: 2026-01-12T16:13:23Z
```

# Possible path-separator issues on windows when invoked from git-bash

---

_@markhepburn_

#### What version of ripgrep are you using?

ripgrep 11.0.0 (rev d7f57d9aab)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Github binary release.

#### What operating system are you using ripgrep on?

Windows 10 (1809)

#### Describe your question, feature request, or bug.

My root problem is that rg is encountering errors on files locked by Visual Studio, yet ignored in .gitignore, and thus returning an exit status of 2.  My motivation even noticing this is I'm using it through Emacs where [helm-ag](https://github.com/syohex/emacs-helm-ag) checks for an exit-status of 0; git-bash has the same behaviour although it's not really a problem there.  Cmd and powershell work as expected.

So I am assuming it is to do with path-handling, although the Emacs case surprises me as I expected that to be "native" (\ as a separator).

#### If this is a bug, what are the steps to reproduce the behavior?

I have a Visual Studio solution, including a sqlproject.  The SQL project contains files called schema.jfm and schema.dbmdl.  With the solution open in Visual Studio, those files are apparently locked.

.gitignore (at the project root) contains
```
*.jfm
*.dbmdl
```

`git check-ignore` confirms that both should be ignored.

#### If this is a bug, what is the actual behavior?

From git-bash, I get the following output (note, --quiet is just to show the issue):
```
$ rg --quiet --no-heading --smart-case --line-number ObjectAllocations                  
./RDSS.Schema\RDSS.Schema.dbmdl: The process cannot access the file because it is being  used by another process. (os error 32)                                                   
./RDSS.Schema\RDSS.Schema.jfm: The process cannot access the file because it is being used by another process. (os error 32)
```

The same issue occurs if run from the RDSS.Schema containing directory.

I have also tried using both `--path-separator "//"` and `--path-separator "\\"` which seem to have no effect (although embarrassingly I only found those options and related issues as I was about to submit :))

#### If this is a bug, what is the expected behavior?

Arguably it isn't even a bug; it's inconvenient, but I appreciate it's a corner case.  I haven't tried to determine how rg handles ignoring, and if its even its fault or whether git itself is invoked, etc.


---

_Comment by @BurntSushi on 2019-05-11 00:22_

Please show the output with `--debug`.

You might also try using the `--no-messages` flag.

---

_Comment by @markhepburn on 2019-05-11 01:51_

@BurntSushi thanks for the reply.  (I think `--no-messages` would suppress error output but not the exit status wouldn't it?)

Here's debug; I'll attach the .gitignore too.  One thing I notice -- and I don't know what I'm looking for sorry -- is that there's no message about turning a glob into a regex for either jfm or dbmdl.

```
$ rg --debug --quiet --no-heading --smart-case --line-number ObjectAllocations
DEBUG|grep_regex::literal|grep-regex\src\literal.rs:59: literal prefixes detected: Literals { lits: [Complete(ObjectAllocations)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Dd]ebug", re: "(?-u)^(?:/?|.*/)[Dd]ebug$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Rr]elease", re: "(?-u)^(?:/?|.*/)[Rr]elease$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('e')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Bb]in", re: "(?-u)^(?:/?|.*/)[Bb]in$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('i'), Literal('n')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Oo]bj", re: "(?-u)^(?:/?|.*/)[Oo]bj$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('O', 'O'), ('o', 'o')] }, Literal('b'), Literal('j')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "packages/*/build", re: "(?-u)^packages/[^/]*/build$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('p'), Literal('a'), Literal('c'), Literal('k'), Literal('a'), Literal('g'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore, Literal('/'), Literal('b'), Literal('u'), Literal('i'), Literal('l'), Literal('d')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Tt]est[Rr]esult*", re: "(?-u)^(?:/?|.*/)[Tt]est[Rr]esult[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('T', 'T'), ('t', 't')] }, Literal('e'), Literal('s'), Literal('t'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('s'), Literal('u'), Literal('l'), Literal('t'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Bb]uild[Ll]og.*", re: "(?-u)^(?:/?|.*/)[Bb]uild[Ll]og\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('u'), Literal('i'), Literal('l'), Literal('d'), Class { negated: false, ranges: [('L', 'L'), ('l', 'l')] }, Literal('o'), Literal('g'), Literal('.'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/_ReSharper*", re: "(?-u)^(?:/?|.*/)_ReSharper[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('R'), Literal('e'), Literal('S'), Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/*.[Rr]e[Ss]harper", re: "(?-u)^(?:/?|.*/)[^/]*\\.[Rr]e[Ss]harper$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Class { negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/_TeamCity*", re: "(?-u)^(?:/?|.*/)_TeamCity[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('T'), Literal('e'), Literal('a'), Literal('m'), Literal('C'), Literal('i'), Literal('t'), Literal('y'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/*.ncrunch*", re: "(?-u)^(?:/?|.*/)[^/]*\\.ncrunch[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('n'), Literal('c'), Literal('r'), Literal('u'), Literal('n'), Literal('c'), Literal('h'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Ee]xpress", re: "(?-u)^(?:/?|.*/)[Ee]xpress$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('E', 'E'), ('e', 'e')] }, Literal('x'), Literal('p'), Literal('r'), Literal('e'), Literal('s'), Literal('s')]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Ss]tyle[Cc]op.*", re: "(?-u)^(?:/?|.*/)[Ss]tyle[Cc]op\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('t'), Literal('y'), Literal('l'), Literal('e'), Class { negated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('o'), Literal('p'), Literal('.'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/~$*", re: "(?-u)^(?:/?|.*/)\\~\\$[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('~'), Literal('$'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/Backup*", re: "(?-u)^(?:/?|.*/)Backup[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('B'), Literal('a'), Literal('c'), Literal('k'), Literal('u'), Literal('p'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 3 literals, 20 basenames, 37 extensions, 0 prefixes, 0 suffixes, 13 required extensions, 16 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./.gitattributes: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "packages/", actual: "**/packages", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "packages/", actual: "**/packages", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSSUpload\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\S3Connector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "packages/", actual: "**/packages", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataConsole\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\TestResults: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Tt]est[Rr]esult*/", actual: "**/[Tt]est[Rr]esult*", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataService\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\.nuget: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataService\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\CKANConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\S3Connector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\Datastore\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\CKANConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\S3Connector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\S3Connector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataController\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataConsole\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\OrcidConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\CKANConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\OrcidConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataController\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\CKANConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RDSSMetadataConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RDSSMetadataConnectorTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "packages/", actual: "**/packages", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\Datastore\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RMDBConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\DatastoreTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RDSSMetadataConnectorTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSSUpload\RDSSUpload\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSSUpload\RDSSUpload\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RDSSMetadataConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\OrcidConnectorTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RMDBConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\DatastoreTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\AynscDataControllerTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
./RDSS.Schema\RDSS.Schema.dbmdl: The process cannot access the file because it is being used by another process. (os error 32)
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\RDSSTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\RMDBConnectorTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\OrcidConnectorTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\AynscDataControllerTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\S3ConnectTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
./RDSS.Schema\RDSS.Schema.jfm: The process cannot access the file because it is being used by another process. (os error 32)
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\RDSSTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\RDSS.Schema.sqlproj.user: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.user", actual: "**/*.user", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\S3ConnectTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\OrcidConsoleSpike\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\RMDBConnectorTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\OrcidConsoleSpike\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
```

---

_Comment by @markhepburn on 2019-05-11 01:54_

.gitignore:
```
## Ignore Visual Studio temporary files, build results, and
## files generated by popular Visual Studio add-ons.

# User-specific files
*.suo
*.user
*.sln.docstates

# Build results

[Dd]ebug/
[Rr]elease/
x64/
build/
[Bb]in/
[Oo]bj/

# Enable "build/" folder in the NuGet Packages folder since NuGet packages use it for MSBuild targets
!packages/*/build/

# MSTest test Results
[Tt]est[Rr]esult*/
[Bb]uild[Ll]og.*

*_i.c
*_p.c
*.ilk
*.meta
*.obj
*.pch
*.pdb
*.pgc
*.pgd
*.rsp
*.sbr
*.tlb
*.tli
*.tlh
*.tmp
*.tmp_proj
*.log
*.vspscc
*.vssscc
.builds
*.pidb
*.log
*.scc
*.datasource

# Visual C++ cache files
ipch/
*.aps
*.ncb
*.opensdf
*.sdf
*.cachefile

# Visual Studio profiler
*.psess
*.vsp
*.vspx

# Guidance Automation Toolkit
*.gpState

# ReSharper is a .NET coding add-in
_ReSharper*/
*.[Rr]e[Ss]harper

# TeamCity is a build add-in
_TeamCity*

# DotCover is a Code Coverage Tool
*.dotCover

# NCrunch
*.ncrunch*
.*crunch*.local.xml

# Installshield output folder
[Ee]xpress/

# DocProject is a documentation generator add-in
DocProject/buildhelp/
DocProject/Help/*.HxT
DocProject/Help/*.HxC
DocProject/Help/*.hhc
DocProject/Help/*.hhk
DocProject/Help/*.hhp
DocProject/Help/Html2
DocProject/Help/html

# Click-Once directory
publish/

# Publish Web Output
*.Publish.xml


# NuGet Packages Directory
## TODO: If you have NuGet Package Restore enabled, uncomment the next line
packages/

# Windows Azure Build Output
csx
*.build.csdef

# Windows Store app package directory
AppPackages/

# Others
sql/
*.Cache
ClientBin/
[Ss]tyle[Cc]op.*
~$*
*~
*.dbmdl
*.[Pp]ublish.xml
*.pfx
*.publishsettings
.vs/

# RIA/Silverlight projects
Generated_Code/

# Backup & report files from converting an old project file to a newer
# Visual Studio version. Backup files are not needed, because we have git ;-)
_UpgradeReport_Files/
Backup*/
UpgradeLog*.XML
UpgradeLog*.htm

# SQL Server files
App_Data/*.mdf
App_Data/*.ldf

# =========================
# Windows detritus
# =========================

# Windows image file caches
Thumbs.db
ehthumbs.db

# Folder config file
Desktop.ini

# Recycle Bin used on file shares
$RECYCLE.BIN/

# Mac crap
.DS_Store
Project Fields.xlsx

# =========================
# Front-end
# =========================
node_modules
*.jfm
```

---

_Comment by @BurntSushi on 2019-05-11 02:47_

> git-bash has the same behaviour although it's not really a problem there. Cmd and powershell work as expected.

I missed this initially. Could you unpack this a bit more? What is the behavioral difference between git-bash and cmd/powershell in this case?

---

_Comment by @markhepburn on 2019-05-11 03:40_

> I missed this initially. Could you unpack this a bit more? What is the behavioral difference between git-bash and cmd/powershell in this case?

Aah, sorry -- git-bash is mingw64, so a bash-shell on windows (with all the path-sep differences that can entail)

The difference is that on git-bash the two locked files are accessed, causing a soft error, and an exit status of 2.  On cmd/powershell the files are ignored as expected.

(oh, I've just re-read my "not a problem" statement by the way: that's because I can still scroll through the expected output, and a couple of "could not access" messages aren't an issue.  It was on Emacs that I first discovered this, because helm-ag checks for a "success" status, so there it's a problem because it won't display any results.  That might not have been too clear, sorry)

---

_Comment by @BurntSushi on 2019-05-11 11:03_

> because helm-ag checks for a "success" status, so there it's a problem because it won't display any results.

I see. So just to be clear, this sounds like a bug in helm-ag. Aside from whether or not ripgrep should be tripping over the files in this specific example, it doesn't make sense to me that a text editor plugin would completely hide all results if just one error occurs. ripgrep obviously doesn't do that, so text editor plugins that aim to support ripgrep shouldn't either.

If helm-ag started out with ag, then that might explain things, since I doubt ag gets its exit status codes correct. But still, this is helm-ag's problem.

> The difference is that on git-bash the two locked files are accessed, causing a soft error, and an exit status of 2. On cmd/powershell the files are ignored as expected.

Okay. This is definitely the most interesting part of this bug report. I see now why you're talking about path separators now. Let's try to be good detectives and forget about possible answers, and first just follow the evidence.

Assuming the debug output above was from git-bash, I think the next bit of information should be to show the debug output when in cmd/powershell. Please make sure to show the command your are running. :)

Thanks for your patience in helping me debug this!

---

_Comment by @markhepburn on 2019-05-11 23:24_

No, thanks for your patience and responsiveness!  And yeah, as I said initially, it's a fairly specific set of circumstances.  I can patch helm-ag to check for exit status of 0 or 2, and run with `--no-messages`, but it still seemed worth unpacking further.

Here's the output from powershell; `$LASTEXITCODE` is 0 (the equivalent of `$?`):
```
PS C:\Users\mark\Projects\RDSS> rg --debug --quiet --no-heading --smart-case --line-number ObjectAllocations
DEBUG|grep_regex::literal|grep-regex\src\literal.rs:59: literal prefixes detected: Literals { lits: [Complete(ObjectAllocations)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Dd]ebug", re: "(?-u)^(?:/?|.*/)[Dd]ebug$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g')]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Rr]elease", re: "(?-u)^(?:/?|.*/)[Rr]elease$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('e')]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Bb]in", re: "(?-u)^(?:/?|.*/)[Bb]in$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('i'), Literal('n')]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Oo]bj", re: "(?-u)^(?:/?|.*/)[Oo]bj$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('O', 'O'), ('o', 'o')] }, Literal('b'), Literal('j')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "packages/*/build", re: "(?-u)^packages/[^/]*/build$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('p'), Literal('a'), Literal('c'), Literal('k'), Literal('a'), Literal('g'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore, Literal('/'), Literal('b'), Literal('u'), Literal('i'), Literal('l'), Literal('d')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Tt]est[Rr]esult*", re: "(?-u)^(?:/?|.*/)[Tt]est[Rr]esult[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('T', 'T'), ('t', 't')] }, Literal('e'), Literal('s'), Literal('t'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('s'), Literal('u'), Literal('l'), Literal('t'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Bb]uild[Ll]og.*", re: "(?-u)^(?:/?|.*/)[Bb]uild[Ll]og\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('u'), Literal('i'), Literal('l'), Literal('d'), Class { negated: false, ranges: [('L', 'L'), ('l', 'l')] }, Literal('o'), Literal('g'), Literal('.'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/_ReSharper*", re: "(?-u)^(?:/?|.*/)_ReSharper[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('R'), Literal('e'), Literal('S'), Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/*.[Rr]e[Ss]harper", re: "(?-u)^(?:/?|.*/)[^/]*\\.[Rr]e[Ss]harper$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Class { negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/_TeamCity*", re: "(?-u)^(?:/?|.*/)_TeamCity[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('T'), Literal('e'), Literal('a'), Literal('m'), Literal('C'), Literal('i'), Literal('t'), Literal('y'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/*.ncrunch*", re: "(?-u)^(?:/?|.*/)[^/]*\\.ncrunch[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('n'), Literal('c'), Literal('r'), Literal('u'), Literal('n'), Literal('c'), Literal('h'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Ee]xpress", re: "(?-u)^(?:/?|.*/)[Ee]xpress$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('E', 'E'), ('e', 'e')] }, Literal('x'), Literal('p'), Literal('r'), Literal('e'), Literal('s'), Literal('s')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/[Ss]tyle[Cc]op.*", re: "(?-u)^(?:/?|.*/)[Ss]tyle[Cc]op\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('t'), Literal('y'), Literal('l'), Literal('e'), Class { negated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('o'), Literal('p'), Literal('.'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/~$*", re: "(?-u)^(?:/?|.*/)\\~\\$[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('~'), Literal('$'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
DEBUG|globset|globset\src\lib.rs:430: glob converted to regex: Glob { glob: "**/Backup*", re: "(?-u)^(?:/?|.*/)Backup[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('B'), Literal('a'), Literal('c'), Literal('k'), Literal('u'), Literal('p'), ZeroOrMore]) }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 3 literals, 20 basenames, 37 extensions, 0 prefixes, 0 suffixes, 13 required extensions, 16 regexes
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./.gitattributes: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "packages/", actual: "**/packages", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\CKANConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\Datastore\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\S3Connector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSSUpload\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\.nuget: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "packages/", actual: "**/packages", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\CKANConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\Datastore\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Datastore\S3Connector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\OrcidConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "packages/", actual: "**/packages", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\.vs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".vs/", actual: "**/.vs", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RDSSMetadataConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RDSSMetadataConnectorTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RMDBConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataConsole\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataConsole\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\OrcidConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\TestResults: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Tt]est[Rr]esult*/", actual: "**/[Tt]est[Rr]esult*", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "packages/", actual: "**/packages", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RDSSMetadataConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RDSSMetadataConnectorTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./MetadataConnector\RMDBConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataController\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataService\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\CKANConnector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSSUpload\RDSSUpload\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\AynscDataControllerTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\AynscDataControllerTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\OrcidConnectorTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\RDSS.Schema.dbmdl: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.dbmdl", actual: "**/*.dbmdl", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataController\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\AsyncDataService\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\CKANConnector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSSUpload\RDSSUpload\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\DatastoreTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\S3Connector\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\OrcidConnectorTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\RDSS.Schema.jfm: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.jfm", actual: "**/*.jfm", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\RDSSTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\RMDBConnectorTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\S3ConnectTests\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\OrcidConsoleSpike\bin: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Bb]in/", actual: "**/[Bb]in", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./Website\OrcidConsoleSpike\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./AsyncDataService\S3Connector\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./RDSS.Schema\RDSS.Schema.sqlproj.user: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.user", actual: "**/*.user", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\RDSSTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\RMDBConnectorTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\S3ConnectTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./UnitTests\DatastoreTests\obj: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "[Oo]bj/", actual: "**/[Oo]bj", is_whitelist: false, is_only_dir: true })))
```

---

_Comment by @BurntSushi on 2019-05-13 17:13_

I think I'm personally stumped on what's causing this without being able to reproduce it myself. Do you know of an easier way to reproduce this issue without needing to setup a Visual Studio project? Basically, it doesn't make sense to me that git bash would result in a file access error, but cmd/PowerShell wouldn't. My mental model says that the shell environment shouldn't have anything to do with how ignore rules are processed or how files are accessed. But that appears to be wrong? Not sure.

---

_Comment by @markhepburn on 2019-05-13 23:45_

Ok, I did a quick bit of testing (I'm not a windows-native sorry!); powershell can be used to easily lock a file:
```powershell
$file = [System.io.File]::Open('c:\path\to\file', 'Open', 'Read', 'None')
# to release when finished:
# $file.Close()
```

I'll see if I can put together a simple test gist or something.  (The other thing at the back of my mind is why the same thing happens from within Emacs, which off the top of my head suggests it might be something to do with environment settings... actually, it does still use "/" as path-separator even on windows, so that's probably consistent with that theory)

---

_Comment by @BurntSushi on 2019-05-13 23:49_

Thanks. If you can manage to get a concise reproduction, then I'll definitely investigate further.

> I'm not a windows-native sorry!

Same. It's the blind leading the blind. :-) 

---

_Comment by @markhepburn on 2019-05-14 00:07_

Ok, here we go: https://gist.github.com/markhepburn/e138c1954a6a7e78be41c5101f1a8bfe

So, clone that and then you can run `rg test` and should only see the one file matching.  To reproduce: in a powershell prompt use the snippet above to `Open` "f1.ignore" (you need to specify the full path), and try again.  From powershell, all is expected, but from git-bash I get:
```
$ rg test                                                                                                                                  
./f1.ignore: The process cannot access the file because it is being used by another process. (os error 32)                                  matches.txt                                                                                                                                 
1:testing one                                                                                                                               
2:two tests

$ echo $?                                                                                                                                   
2
```

With debug output:
```
$ rg --debug test
DEBUG|grep_regex::literal|grep-regex\src\literal.rs:59: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore\src\walk.rs:1617: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
./f1.ignore: The process cannot access the file because it is being used by another process. (os error 32)
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
matches.txt
1:testing one
2:two tests
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

---

_Comment by @markhepburn on 2019-05-14 00:09_

(In case it didn't get mentioned / wasn't obvious, "git-bash" is the bash prompt that ships with the standard  git-for-windows distro: https://gitforwindows.org/  It's using mingw64 I believe)

---
