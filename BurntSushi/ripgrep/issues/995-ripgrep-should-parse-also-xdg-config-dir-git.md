```yaml
number: 995
title: Ripgrep should parse also $XDG_CONFIG_DIR/.git/config
type: issue
state: closed
author: hauleth
labels: []
assignees: []
created_at: 2018-07-28T11:09:04Z
updated_at: 2018-07-28T14:05:59Z
url: https://github.com/BurntSushi/ripgrep/issues/995
synced_at: 2026-01-12T16:13:22Z
```

# Ripgrep should parse also $XDG_CONFIG_DIR/.git/config

---

_@hauleth_

#### What version of ripgrep are you using?

```
ripgrep 0.8.1 (rev c8e9f25b85)
+SIMD -AVX
```

#### How did you install ripgrep?

BurntSushi's Brew repository

#### What operating system are you using ripgrep on?

```
Darwin Niuniobook 17.5.0 Darwin Kernel Version 17.5.0: Fri Apr 13 19:32:32 PDT 2018; root:xnu-4570.51.2~1/RELEASE_X86_64 x86_64
```

#### Describe your question, feature request, or bug.

Ripgrep looks for definition of global `gitignore` in `$HOME/.gitconfig` file only, while Git (at least in version 2.18.0) supports only "standard" `$XDG_CONFIG_DIR/git/config` location.

#### If this is a bug, what are the steps to reproduce the behavior?

Have anything set in Git file pointed by `core.excludesfile` and move your `.gitconfig` to `~/.config/git/config`. Now RipGrep will search in files it should ignore.

#### If this is a bug, what is the actual behavior?

Expected:

```
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'p',
        'a',
        'c',
        'k',
        'a',
        'g',
        'e'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(package)], limit_size: 250, limit_class: 10 }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/.DS_Store?", re: "(?-u)^(?:/?|.*/)\\.DS_Store.$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('D'), Literal('S'), Literal('_'), Literal('S'), Literal('t'), Literal('o'), Literal('r'), Literal('e'), Any]) }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "tags.*", re: "(?-u)^tags\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([Literal('t'), Literal('a'), Literal('g'), Literal('s'), Literal('.'), ZeroOrMore]) }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/perf.data*", re: "(?-u)^(?:/?|.*/)perf\\.data.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('p'), Literal('e'), Literal('r'), Literal('f'), Literal('.'), Literal('d'), Literal('a'), Literal('t'), Literal('a'), ZeroOrMore]) }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 2 literals, 17 basenames, 20 extensions, 0 prefixes, 1 suffixes, 2 required extensions, 4 regexes
DEBUG/globset/globset/src/lib.rs:401: built glob set; 12 literals, 4 basenames, 2 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 0 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.envrc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".envrc", actual: "**/.envrc", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.iex.exs: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.credo.exs: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./pg_indices.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./sample.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.tool-versions: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./uploads: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "uploads", actual: "**/uploads", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.tool-versions-e: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: ".tool-versions-e", actual: "**/.tool-versions-e", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./tags: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: "/tags", actual: "tags", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./pg_waits.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.elixir_ls: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: ".elixir_ls", actual: "**/.elixir_ls", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.dockerignore: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./test/serializers/fixture.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./test/serializers/fixture_categories_update.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./test/serializers/fixture_price_update.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.sobelow: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./_build: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/_build", actual: "_build", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./db: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/db", actual: "db", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./deps: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/deps", actual: "deps", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./log: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/log", actual: "log", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./doc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/doc", actual: "doc", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.gitlab-ci.yml: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.formatter.exs: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.projections.json: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/*.json", actual: "*.json", is_whitelist: false, is_only_dir: false })))
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "*", re: "(?-u)^[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([ZeroOrMore]) }
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/ui: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/priv/ui", actual: "priv/ui", is_whitelist: false, is_only_dir: false })))
DEBUG/globset/globset/src/lib.rs:401: built glob set; 2 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1431: whitelisting ./rel/config.exs: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./rel/.gitignore"), original: "!/config.exs", actual: "config.exs", is_whitelist: true, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1431: whitelisting ./rel/.gitignore: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./rel/.gitignore"), original: "!/.gitignore", actual: ".gitignore", is_whitelist: true, is_only_dir: false })))
DEBUG/globset/globset/src/lib.rs:401: built glob set; 3 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.babelrc: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/yarn-error.log: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: "*.log", actual: "**/*.log", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/node_modules: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./assets/.gitignore"), original: "/node_modules", actual: "node_modules", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.editorconfig: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.gitattributes: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.eslintignore: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.gitlab-ci.yml: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/coverage: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./assets/.gitignore"), original: "/coverage", actual: "coverage", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/static/api.html.gz: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: "*.gz", actual: "**/*.gz", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/static/api-80327c94ee2a2bb64e24c103260f6019.html.gz: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: "*.gz", actual: "**/*.gz", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/static/.keep: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/static/access-f7cd8ee569a9b4cef078cd39a7f742e8.txt.gz: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: "*.gz", actual: "**/*.gz", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/static/robots.txt.gz: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: "*.gz", actual: "**/*.gz", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/static/robots-af4b612dcd1660973481595c0c34f7a4.txt.gz: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: "*.gz", actual: "**/*.gz", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/static/access.txt.gz: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore"), original: "*.gz", actual: "**/*.gz", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/medications.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/allergies_non_drug.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/complaints.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/states_and_cities.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/conditions.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/immunizations.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/allergies_drug.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/anesthesias.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/substances.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/surgical_procedures.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/app/.eslintrc.js: Ignore(IgnoreMatch(Hidden))
```

Got:

```
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'p',
        'a',
        'c',
        'k',
        'a',
        'g',
        'e'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(package)], limit_size: 250, limit_class: 10 }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 12 literals, 4 basenames, 2 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 0 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.envrc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".envrc", actual: "**/.envrc", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.iex.exs: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.credo.exs: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./pg_indices.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./sample.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.tool-versions: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./uploads: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "uploads", actual: "**/uploads", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.tool-versions-e: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./pg_waits.sql: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.sql", actual: "**/*.sql", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.elixir_ls: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.dockerignore: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.sobelow: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./_build: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/_build", actual: "_build", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./db: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/db", actual: "db", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./deps: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/deps", actual: "deps", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./log: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/log", actual: "log", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./doc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/doc", actual: "doc", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.gitlab-ci.yml: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.formatter.exs: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.projections.json: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/*.json", actual: "*.json", is_whitelist: false, is_only_dir: false })))
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "*", re: "(?-u)^[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([ZeroOrMore]) }
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/ui: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/priv/ui", actual: "priv/ui", is_whitelist: false, is_only_dir: false })))
DEBUG/globset/globset/src/lib.rs:401: built glob set; 2 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./test/serializers/fixture.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./test/serializers/fixture_categories_update.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1431: whitelisting ./rel/config.exs: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./rel/.gitignore"), original: "!/config.exs", actual: "config.exs", is_whitelist: true, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./test/serializers/fixture_price_update.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/globset/globset/src/lib.rs:401: built glob set; 3 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1431: whitelisting ./rel/.gitignore: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./rel/.gitignore"), original: "!/.gitignore", actual: ".gitignore", is_whitelist: true, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.babelrc: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/node_modules: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./assets/.gitignore"), original: "/node_modules", actual: "node_modules", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.editorconfig: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.gitattributes: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.eslintignore: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/.gitlab-ci.yml: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/coverage: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./assets/.gitignore"), original: "/coverage", actual: "coverage", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/static/.keep: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./assets/app/.eslintrc.js: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/medications.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/allergies_non_drug.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/complaints.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/states_and_cities.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/conditions.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/immunizations.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/allergies_drug.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/anesthesias.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/substances.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./priv/repo/seeds/surgical_procedures.csv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.csv", actual: "**/*.csv", is_whitelist: false, is_only_dir: false })))
```

See that in second version `/Users/hauleth/Workspace/hauleth/dotfiles/git/ignore` isn't mentioned even once.

#### If this is a bug, what is the expected behavior?

Should fallback to `${XDG_CONFIG_DIR:-$HOME/.config}/git/config` if lookup for `core.excludefile` in `$HOME/.gitconfig` failed.


---

_Closed by @BurntSushi on 2018-07-28 14:05_

---
