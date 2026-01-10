---
number: 11253
title: cannot install codecov-cli with uv
type: issue
state: closed
author: superlopuh
labels:
  - question
assignees: []
created_at: 2025-02-05T17:41:48Z
updated_at: 2025-02-07T07:28:04Z
url: https://github.com/astral-sh/uv/issues/11253
synced_at: 2026-01-10T01:25:03Z
---

# cannot install codecov-cli with uv

---

_Issue opened by @superlopuh on 2025-02-05 17:41_

### Summary

I initially thought it was a bug on their end, but `pip install codecov-cli` works, and `uv pip install codecov-cli` doesn't on my machine.

```
Resolved 22 packages in 6ms
  × Failed to build `codecov-cli==10.0.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      copying codecov_cli/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli
      copying codecov_cli/opentelemetry.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli
      copying codecov_cli/types.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli
      copying codecov_cli/fallbacks.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli
      copying codecov_cli/main.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli
      copying codecov_cli/plugins/xcode.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/plugins
      copying codecov_cli/plugins/compress_pycoverage_contexts.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/plugins
      copying codecov_cli/plugins/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/plugins
      copying codecov_cli/plugins/types.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/plugins
      copying codecov_cli/plugins/pycoverage.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/plugins
      copying codecov_cli/plugins/gcov.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/plugins
      copying codecov_cli/runners/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/runners
      copying codecov_cli/runners/types.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/runners
      copying codecov_cli/runners/dan_runner.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/runners
      copying codecov_cli/runners/pytest_standard_runner.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/runners
      copying codecov_cli/commands/create_report_result.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/process_test_results.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/base_picking.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/upload.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/upload_process.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/commit.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/staticanalysis.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/labelanalysis.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/get_report_results.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/empty_upload.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/report.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/send_notifications.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/commands/upload_coverage.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/commands
      copying codecov_cli/helpers/options.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/git.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/config.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/versioning_systems.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/validators.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/request.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/encoder.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/folder_searcher.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/args.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/helpers/logging_utils.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers
      copying codecov_cli/services/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services
      copying codecov_cli/helpers/git_services/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/git_services
      copying codecov_cli/helpers/git_services/github.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/git_services
      copying codecov_cli/helpers/ci_adapters/gitlab_ci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/local.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/cloudbuild.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/cirrus_ci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/azure_pipelines.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/github_actions.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/bitbucket_ci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/codebuild.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/teamcity.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/woodpeckerci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/heroku.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/jenkins.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/bitrise_ci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/buildkite.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/droneci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/circleci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/appveyor_ci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/travis_ci.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/base.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/helpers/ci_adapters
      copying codecov_cli/services/commit/base_picking.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/commit
      copying codecov_cli/services/commit/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/commit
      copying codecov_cli/services/empty_upload/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/empty_upload
      copying codecov_cli/services/upload_completion/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/upload_completion
      copying codecov_cli/services/upload_coverage/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/upload_coverage
      copying codecov_cli/services/staticanalysis/finders.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/types.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/exceptions.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis
      copying codecov_cli/services/report/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/report
      copying codecov_cli/services/upload/upload_sender.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/upload
      copying codecov_cli/services/upload/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/upload
      copying codecov_cli/services/upload/upload_collector.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/upload
      copying codecov_cli/services/upload/network_finder.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/upload
      copying codecov_cli/services/upload/legacy_upload_sender.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/upload
      copying codecov_cli/services/upload/file_finder.py -> build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/upload
      copying codecov_cli/services/staticanalysis/analyzers/__init__.py ->
      build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis/analyzers
      copying codecov_cli/services/staticanalysis/analyzers/general.py ->
      build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis/analyzers
      copying codecov_cli/services/staticanalysis/analyzers/python/node_wrappers.py ->
      build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis/analyzers/python
      copying codecov_cli/services/staticanalysis/analyzers/python/__init__.py ->
      build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis/analyzers/python
      copying codecov_cli/services/staticanalysis/analyzers/javascript_es6/node_wrappers.py ->
      build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis/analyzers/javascript_es6
      copying codecov_cli/services/staticanalysis/analyzers/javascript_es6/__init__.py ->
      build/lib.macosx-11.0-arm64-cpython-312/codecov_cli/services/staticanalysis/analyzers/javascript_es6
      running build_ext
      building 'staticcodecov_languages' extension
      clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new
      -I/opt/homebrew/opt/llvm/include -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter
      -I/Users/sasha/.cache/uv/builds-v0/.tmpikmw8h/include -I/Users/sasha/.local/share/uv/python/cpython-3.12.7-macos-aarch64-none/include/python3.12
      -c languages/languages.c -o build/temp.macosx-11.0-arm64-cpython-312/languages/languages.o -Wno-unused-variable
      clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new
      -I/opt/homebrew/opt/llvm/include -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter
      -I/Users/sasha/.cache/uv/builds-v0/.tmpikmw8h/include -I/Users/sasha/.local/share/uv/python/cpython-3.12.7-macos-aarch64-none/include/python3.12
      -c languages/treesitterjavascript/src/parser.c -o build/temp.macosx-11.0-arm64-cpython-312/languages/treesitterjavascript/src/parser.o
      -Wno-unused-variable
      clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new
      -I/opt/homebrew/opt/llvm/include -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter
      -I/Users/sasha/.cache/uv/builds-v0/.tmpikmw8h/include -I/Users/sasha/.local/share/uv/python/cpython-3.12.7-macos-aarch64-none/include/python3.12
      -c languages/treesitterjavascript/src/scanner.c -o build/temp.macosx-11.0-arm64-cpython-312/languages/treesitterjavascript/src/scanner.o
      -Wno-unused-variable
      clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new
      -I/opt/homebrew/opt/llvm/include -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter
      -I/Users/sasha/.cache/uv/builds-v0/.tmpikmw8h/include -I/Users/sasha/.local/share/uv/python/cpython-3.12.7-macos-aarch64-none/include/python3.12
      -c languages/treesitterpython/src/parser.c -o build/temp.macosx-11.0-arm64-cpython-312/languages/treesitterpython/src/parser.o
      -Wno-unused-variable
      clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new
      -I/opt/homebrew/opt/llvm/include -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter
      -I/Users/sasha/.cache/uv/builds-v0/.tmpikmw8h/include -I/Users/sasha/.local/share/uv/python/cpython-3.12.7-macos-aarch64-none/include/python3.12
      -c languages/treesitterpython/src/scanner.cc -o build/temp.macosx-11.0-arm64-cpython-312/languages/treesitterpython/src/scanner.o
      -Wno-unused-variable
      clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new
      -I/opt/homebrew/opt/llvm/include -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0 -isysroot
      /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -LModules/_hacl
      -L/opt/homebrew/opt/llvm/lib -I/opt/homebrew/opt/llvm/include build/temp.macosx-11.0-arm64-cpython-312/languages/languages.o
      build/temp.macosx-11.0-arm64-cpython-312/languages/treesitterjavascript/src/parser.o
      build/temp.macosx-11.0-arm64-cpython-312/languages/treesitterjavascript/src/scanner.o
      build/temp.macosx-11.0-arm64-cpython-312/languages/treesitterpython/src/parser.o
      build/temp.macosx-11.0-arm64-cpython-312/languages/treesitterpython/src/scanner.o -L/install/lib -o
      build/lib.macosx-11.0-arm64-cpython-312/staticcodecov_languages.cpython-312-darwin.so

      [stderr]
      languages/languages.c:6:3: warning: suggest braces around initialization of subobject [-Wmissing-braces]
          6 |   NULL
            |   ^~~~
            |   {   }
      /Library/Developer/CommandLineTools/SDKs/MacOSX15.sdk/usr/include/sys/_types/_null.h:49:15: note: expanded from macro 'NULL'
         49 | #define NULL  __DARWIN_NULL
            |               ^~~~~~~~~~~~~
      /Library/Developer/CommandLineTools/SDKs/MacOSX15.sdk/usr/include/sys/_types.h:64:23: note: expanded from macro '__DARWIN_NULL'
         64 | #define __DARWIN_NULL ((void *)0)
            |                       ^~~~~~~~~~~
      1 warning generated.
      languages/treesitterjavascript/src/parser.c:3852:3: warning: variable 'eof' set but not used [-Wunused-but-set-variable]
       3852 |   START_LEXER();
            |   ^
      languages/treesitterpython/src/tree_sitter/parser.h:136:8: note: expanded from macro 'START_LEXER'
        136 |   bool eof = false;             \
            |        ^
      1 warning generated.
      languages/treesitterpython/src/parser.c:5049:3: warning: variable 'eof' set but not used [-Wunused-but-set-variable]
       5049 |   START_LEXER();
            |   ^
      languages/treesitterpython/src/tree_sitter/parser.h:136:8: note: expanded from macro 'START_LEXER'
        136 |   bool eof = false;             \
            |        ^
      1 warning generated.
      Compiling with an SDK that doesn't seem to exist:
      /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      clang++: warning: no such sysroot directory:
      '/Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk' [-Wmissing-sysroot]
      ld: warning: search path 'Modules/_hacl' not found
      ld: warning: search path '/install/lib' not found
      ld: library 'c++' not found
      clang++: error: linker command failed with exit code 1 (use -v to see invocation)
      error: command '/opt/homebrew/opt/llvm/bin/clang++' failed with exit code 1

      hint: This usually indicates a problem with the package or the build environment.
```

### Platform

macOS 15.3 arm

### Version

uv 0.5.25 (Homebrew 2025-01-28)

### Python version

3.12.7

---

_Label `bug` added by @superlopuh on 2025-02-05 17:41_

---

_Comment by @superlopuh on 2025-02-05 17:42_

https://github.com/codecov/codecov-cli/issues/582#issuecomment-2637546374

---

_Comment by @charliermarsh on 2025-02-05 17:42_

That looks like an old uv Python install. Can you try reinstalling? `uv python install --reinstall ${version}`.

---

_Comment by @superlopuh on 2025-02-05 17:58_

I reinstalled python 3.12, 3.12.7, 3.13, deleted the venv, ran `uv venv` and `uv pip install codecov-cli` and got a very similar error message:

```
Resolved 21 packages in 110ms
   Built ijson==3.3.0                                                                                                                                   × Failed to build `codecov-cli==10.0.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/opentelemetry.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/types.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/fallbacks.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/main.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/xcode.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/compress_pycoverage_contexts.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/types.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/pycoverage.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/gcov.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      copying codecov_cli/runners/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      copying codecov_cli/runners/types.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      copying codecov_cli/runners/dan_runner.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      copying codecov_cli/runners/pytest_standard_runner.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/create_report_result.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/process_test_results.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/base_picking.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/upload.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/upload_process.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/commit.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/staticanalysis.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/labelanalysis.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/get_report_results.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/empty_upload.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/report.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/send_notifications.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/upload_coverage.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/options.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/git.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/config.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/versioning_systems.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/validators.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/request.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/encoder.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/folder_searcher.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/args.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/logging_utils.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services
      copying codecov_cli/services/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/git_services
      copying codecov_cli/helpers/git_services/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/git_services
      copying codecov_cli/helpers/git_services/github.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/git_services
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/gitlab_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/local.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/cloudbuild.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/cirrus_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/azure_pipelines.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/github_actions.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/bitbucket_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/codebuild.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/teamcity.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/woodpeckerci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/heroku.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/jenkins.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/bitrise_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/buildkite.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/droneci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/circleci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/appveyor_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/travis_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/base.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/commit
      copying codecov_cli/services/commit/base_picking.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/commit
      copying codecov_cli/services/commit/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/commit
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/empty_upload
      copying codecov_cli/services/empty_upload/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/empty_upload
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload_completion
      copying codecov_cli/services/upload_completion/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload_completion
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload_coverage
      copying codecov_cli/services/upload_coverage/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload_coverage
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/finders.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/types.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/exceptions.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/report
      copying codecov_cli/services/report/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/report
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/upload_sender.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/upload_collector.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/network_finder.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/legacy_upload_sender.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/file_finder.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers
      copying codecov_cli/services/staticanalysis/analyzers/__init__.py ->
      build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers
      copying codecov_cli/services/staticanalysis/analyzers/general.py ->
      build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/python
      copying codecov_cli/services/staticanalysis/analyzers/python/node_wrappers.py ->
      build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/python
      copying codecov_cli/services/staticanalysis/analyzers/python/__init__.py ->
      build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/python
      creating build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/javascript_es6
      copying codecov_cli/services/staticanalysis/analyzers/javascript_es6/node_wrappers.py ->
      build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/javascript_es6
      copying codecov_cli/services/staticanalysis/analyzers/javascript_es6/__init__.py ->
      build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/javascript_es6
      running build_ext
      building 'staticcodecov_languages' extension
      creating build/temp.macosx-11.0-arm64-cpython-313/languages
      creating build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterjavascript/src
      creating build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterpython/src
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -I/opt/homebrew/opt/llvm/include
      -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src -Ilanguages/treesitterjavascript/src/tree_sitter
      -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpU1UxVt/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/languages.c -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/languages.o -Wno-unused-variable
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -I/opt/homebrew/opt/llvm/include
      -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src -Ilanguages/treesitterjavascript/src/tree_sitter
      -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpU1UxVt/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/treesitterjavascript/src/parser.c -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterjavascript/src/parser.o -Wno-unused-variable
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -I/opt/homebrew/opt/llvm/include
      -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src -Ilanguages/treesitterjavascript/src/tree_sitter
      -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpU1UxVt/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/treesitterjavascript/src/scanner.c -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterjavascript/src/scanner.o -Wno-unused-variable
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -I/opt/homebrew/opt/llvm/include
      -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src -Ilanguages/treesitterjavascript/src/tree_sitter
      -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpU1UxVt/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/treesitterpython/src/parser.c -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterpython/src/parser.o -Wno-unused-variable
      c++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0
      -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -I/opt/homebrew/opt/llvm/include
      -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src -Ilanguages/treesitterjavascript/src/tree_sitter
      -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpU1UxVt/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/treesitterpython/src/scanner.cc -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterpython/src/scanner.o -Wno-unused-variable

      [stderr]
      languages/languages.c:6:3: warning: suggest braces around initialization of subobject [-Wmissing-braces]
          6 |   NULL
            |   ^~~~
            |   {   }
      /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/sys/_types/_null.h:49:15: note: expanded from macro 'NULL'
         49 | #define NULL  __DARWIN_NULL
            |               ^~~~~~~~~~~~~
      /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/sys/_types.h:64:23: note: expanded from macro '__DARWIN_NULL'
         64 | #define __DARWIN_NULL ((void *)0)
            |                       ^~~~~~~~~~~
      1 warning generated.
      languages/treesitterjavascript/src/parser.c:3852:3: warning: variable 'eof' set but not used [-Wunused-but-set-variable]
       3852 |   START_LEXER();
            |   ^
      languages/treesitterpython/src/tree_sitter/parser.h:136:8: note: expanded from macro 'START_LEXER'
        136 |   bool eof = false;             \
            |        ^
      1 warning generated.
      languages/treesitterpython/src/parser.c:5049:3: warning: variable 'eof' set but not used [-Wunused-but-set-variable]
       5049 |   START_LEXER();
            |   ^
      languages/treesitterpython/src/tree_sitter/parser.h:136:8: note: expanded from macro 'START_LEXER'
        136 |   bool eof = false;             \
            |        ^
      1 warning generated.
      languages/treesitterpython/src/scanner.cc:2:10: fatal error: 'vector' file not found
          2 | #include <vector>
            |          ^~~~~~~~
      1 error generated.
      error: command '/usr/bin/c++' failed with exit code 1

      hint: This usually indicates a problem with the package or the build environment.
```

---

_Comment by @zanieb on 2025-02-05 18:02_

```
      languages/treesitterpython/src/scanner.cc:2:10: fatal error: 'vector' file not found
          2 | #include <vector>
            |          ^~~~~~~~
      1 error generated.
```

It looks like you're missing some header file? Please refer to https://docs.astral.sh/uv/reference/troubleshooting/build-failures/

Does `pip install --use-pep517 --no-cache --force-reinstall` work?

---

_Comment by @zanieb on 2025-02-05 18:04_

Looks like https://stackoverflow.com/questions/39199489/cannot-include-vector

What does `which cc` show? Presumably `gcc`? Looks like they require `g++`?

---

_Comment by @superlopuh on 2025-02-05 18:10_

`which cc` -> `/usr/bin/cc`
The `pip install` command you gave above doesn't work due to the environment being externally managed, do you mean that I should try that command in with the venv created by `python -m venv` activated?

Thank you for the link, I'll have a read now.

---

_Comment by @zanieb on 2025-02-05 18:12_

mm it's actually using `/usr/bin/c++` (from the error)

Yeah sorry, you'd do `uv venv --seed && ./.venv/bin/pip` (or activate)

---

_Comment by @zanieb on 2025-02-05 18:14_

It might be helpful to invoke the compiler to see what it "actually is", e.g.

```
❯ /usr/bin/c++ --version
Apple clang version 16.0.0 (clang-1600.0.26.6)
Target: arm64-apple-darwin23.6.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin
```

---

_Comment by @zanieb on 2025-02-05 18:19_

I think it might be the `-I/opt/homebrew/opt/llvm/include` flag in the compilation? 

On my machine, that directory does not have `vector`

```
❯ ls /opt/homebrew/opt/llvm/include/
__libunwind_config.h	__pstl_numeric		libunwind.h		mach-o			unwind.h
__pstl_algorithm	c++			libunwind.modulemap	mlir			unwind_arm_ehabi.h
__pstl_config_site	clang			lldb			mlir-c			unwind_itanium.h
__pstl_execution	clang-c			llvm			polly
__pstl_memory		clang-tidy		llvm-c			pstl
```

It's in a subdirectory

```
❯ ls /opt/homebrew/opt/llvm/include/c++/v1 | grep vector
vector
```

---

_Comment by @zanieb on 2025-02-05 18:19_

I'm not sure how that LLVM directory is getting on your include path / how it's supposed to be correct populated.

---

_Comment by @superlopuh on 2025-02-05 19:45_

Steps that fixed the install for me:


1. remove the line `export PATH="/opt/homebrew/opt/llvm/bin:$PATH;"` from my .zshrc
2. reinstall python as above
3. make new venv
4. `uv venv --seed`
5. `./.venv/bin/pip install codecov-cli`

But when I delete the venv and try to install codecov-cli with `uv pip install codecov-cli` it doesn't work.

---

_Comment by @zanieb on 2025-02-05 19:59_

I need more than "it doesn't work", can you share details?

---

_Label `bug` removed by @zanieb on 2025-02-05 19:59_

---

_Label `question` added by @zanieb on 2025-02-05 19:59_

---

_Comment by @superlopuh on 2025-02-05 23:02_

Sorry, I mean that I get exactly the same error message. If I do step 4 but then `uv pip install` it's the same error message. But directly calling `pip` works.

---

_Comment by @zanieb on 2025-02-05 23:23_

Can you compare the `pip` build logs to the `uv pip` ones? Did it succeed with those extra flags I asked for? Did you confirm you're using clang?


---

_Comment by @superlopuh on 2025-02-06 08:32_

In my understanding, the `.venv/bin/pip` does not build at all, and finds a pre-built wheel:

```
Collecting codecov-cli
  Using cached codecov_cli-10.0.1-cp313-cp313-macosx_14_0_arm64.whl
Requirement already satisfied: click==8.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (8.1.8)
Requirement already satisfied: httpx==0.27.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (0.27.2)
Requirement already satisfied: ijson==3.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (3.3.0)
Requirement already satisfied: pyyaml==6.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (6.0.2)
Requirement already satisfied: responses==0.21.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (0.21.0)
Requirement already satisfied: tree-sitter==0.20.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (0.20.4)
Requirement already satisfied: test-results-parser==0.5.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (0.5.1)
Requirement already satisfied: regex in ./.venv/lib/python3.13/site-packages (from codecov-cli) (2024.11.6)
Requirement already satisfied: sentry-sdk>=2.20.0 in ./.venv/lib/python3.13/site-packages (from codecov-cli) (2.20.0)
Requirement already satisfied: wrapt>=1.17.2 in ./.venv/lib/python3.13/site-packages (from codecov-cli) (1.17.2)
Requirement already satisfied: anyio in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (4.8.0)
Requirement already satisfied: certifi in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (2025.1.31)
Requirement already satisfied: httpcore==1.* in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (1.0.7)
Requirement already satisfied: idna in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (3.10)
Requirement already satisfied: sniffio in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (1.3.1)
Requirement already satisfied: requests<3.0,>=2.0 in ./.venv/lib/python3.13/site-packages (from responses==0.21.*->codecov-cli) (2.32.3)
Requirement already satisfied: urllib3>=1.25.10 in ./.venv/lib/python3.13/site-packages (from responses==0.21.*->codecov-cli) (2.3.0)
Requirement already satisfied: setuptools>=60.0.0 in ./.venv/lib/python3.13/site-packages (from tree-sitter==0.20.*->codecov-cli) (75.8.0)
Requirement already satisfied: h11<0.15,>=0.13 in ./.venv/lib/python3.13/site-packages (from httpcore==1.*->httpx==0.27.*->codecov-cli) (0.14.0)
Requirement already satisfied: charset-normalizer<4,>=2 in ./.venv/lib/python3.13/site-packages (from requests<3.0,>=2.0->responses==0.21.*->codecov-cli) (3.4.1)
Installing collected packages: codecov-cli
Successfully installed codecov-cli-10.0.1
```

---

_Comment by @zanieb on 2025-02-06 14:33_

I see. You can force a build there with `--no-binary codecov-cli` — I can only presume it would fail in the same way as `uv pip`.

It looks like in the other example we're building for cp312 but here you're using cp313?

---

_Comment by @superlopuh on 2025-02-06 20:46_

`uv run pip install --no-binary :all: codecov-cli` this seems to succeed just as without the `--no-binary :all:`, if that's what you meant.

I commented out the following lines in my zshrc and now there's really nothing left that's llvm related:
```
# export LDFLAGS="-L/opt/homebrew/opt/llvm/lib"
# export CPPFLAGS="-I/opt/homebrew/opt/llvm/include"
```

Indeed, I made a new venv with a different Python, here are the outputs of the invocations:

```
❯ uv run pip install --no-binary :all: codecov-cli 
Collecting codecov-cli
  Using cached codecov_cli-10.0.1-cp313-cp313-macosx_14_0_arm64.whl
Requirement already satisfied: click==8.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (8.1.8)
Requirement already satisfied: httpx==0.27.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (0.27.2)
Requirement already satisfied: ijson==3.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (3.3.0)
Requirement already satisfied: pyyaml==6.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (6.0.2)
Requirement already satisfied: responses==0.21.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (0.21.0)
Requirement already satisfied: tree-sitter==0.20.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (0.20.4)
Requirement already satisfied: test-results-parser==0.5.* in ./.venv/lib/python3.13/site-packages (from codecov-cli) (0.5.1)
Requirement already satisfied: regex in ./.venv/lib/python3.13/site-packages (from codecov-cli) (2024.11.6)
Requirement already satisfied: sentry-sdk>=2.20.0 in ./.venv/lib/python3.13/site-packages (from codecov-cli) (2.20.0)
Requirement already satisfied: wrapt>=1.17.2 in ./.venv/lib/python3.13/site-packages (from codecov-cli) (1.17.2)
Requirement already satisfied: anyio in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (4.8.0)
Requirement already satisfied: certifi in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (2025.1.31)
Requirement already satisfied: httpcore==1.* in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (1.0.7)
Requirement already satisfied: idna in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (3.10)
Requirement already satisfied: sniffio in ./.venv/lib/python3.13/site-packages (from httpx==0.27.*->codecov-cli) (1.3.1)
Requirement already satisfied: requests<3.0,>=2.0 in ./.venv/lib/python3.13/site-packages (from responses==0.21.*->codecov-cli) (2.32.3)
Requirement already satisfied: urllib3>=1.25.10 in ./.venv/lib/python3.13/site-packages (from responses==0.21.*->codecov-cli) (2.3.0)
Requirement already satisfied: setuptools>=60.0.0 in ./.venv/lib/python3.13/site-packages (from tree-sitter==0.20.*->codecov-cli) (75.8.0)
Requirement already satisfied: h11<0.15,>=0.13 in ./.venv/lib/python3.13/site-packages (from httpcore==1.*->httpx==0.27.*->codecov-cli) (0.14.0)
Requirement already satisfied: charset-normalizer<4,>=2 in ./.venv/lib/python3.13/site-packages (from requests<3.0,>=2.0->responses==0.21.*->codecov-cli) (3.4.1)
Installing collected packages: codecov-cli
Successfully installed codecov-cli-10.0.1
```

```
❯ uv pip install codecov-cli
Resolved 21 packages in 26ms
  × Failed to build `codecov-cli==10.0.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      copying codecov_cli/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/opentelemetry.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/types.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/fallbacks.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/main.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli
      copying codecov_cli/plugins/xcode.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/compress_pycoverage_contexts.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/types.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/pycoverage.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/plugins/gcov.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/plugins
      copying codecov_cli/runners/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      copying codecov_cli/runners/types.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      copying codecov_cli/runners/dan_runner.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      copying codecov_cli/runners/pytest_standard_runner.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/runners
      copying codecov_cli/commands/create_report_result.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/process_test_results.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/base_picking.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/upload.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/upload_process.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/commit.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/staticanalysis.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/labelanalysis.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/get_report_results.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/empty_upload.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/report.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/send_notifications.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/commands/upload_coverage.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/commands
      copying codecov_cli/helpers/options.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/git.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/config.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/versioning_systems.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/validators.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/request.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/encoder.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/folder_searcher.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/args.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/helpers/logging_utils.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers
      copying codecov_cli/services/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services
      copying codecov_cli/helpers/git_services/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/git_services
      copying codecov_cli/helpers/git_services/github.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/git_services
      copying codecov_cli/helpers/ci_adapters/gitlab_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/local.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/cloudbuild.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/cirrus_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/azure_pipelines.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/github_actions.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/bitbucket_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/codebuild.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/teamcity.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/woodpeckerci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/heroku.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/jenkins.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/bitrise_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/buildkite.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/droneci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/circleci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/appveyor_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/travis_ci.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/helpers/ci_adapters/base.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/helpers/ci_adapters
      copying codecov_cli/services/commit/base_picking.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/commit
      copying codecov_cli/services/commit/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/commit
      copying codecov_cli/services/empty_upload/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/empty_upload
      copying codecov_cli/services/upload_completion/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload_completion
      copying codecov_cli/services/upload_coverage/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload_coverage
      copying codecov_cli/services/staticanalysis/finders.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/types.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      copying codecov_cli/services/staticanalysis/exceptions.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis
      copying codecov_cli/services/report/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/report
      copying codecov_cli/services/upload/upload_sender.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/upload_collector.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/network_finder.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/legacy_upload_sender.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/upload/file_finder.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/upload
      copying codecov_cli/services/staticanalysis/analyzers/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers
      copying codecov_cli/services/staticanalysis/analyzers/general.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers
      copying codecov_cli/services/staticanalysis/analyzers/python/node_wrappers.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/python
      copying codecov_cli/services/staticanalysis/analyzers/python/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/python
      copying codecov_cli/services/staticanalysis/analyzers/javascript_es6/node_wrappers.py ->
      build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/javascript_es6
      copying codecov_cli/services/staticanalysis/analyzers/javascript_es6/__init__.py ->
      build/lib.macosx-11.0-arm64-cpython-313/codecov_cli/services/staticanalysis/analyzers/javascript_es6
      running build_ext
      building 'staticcodecov_languages' extension
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness
      -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpJqbrAA/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/languages.c -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/languages.o -Wno-unused-variable
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness
      -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpJqbrAA/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/treesitterjavascript/src/parser.c -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterjavascript/src/parser.o -Wno-unused-variable
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness
      -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpJqbrAA/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/treesitterjavascript/src/scanner.c -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterjavascript/src/scanner.o -Wno-unused-variable
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness
      -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpJqbrAA/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/treesitterpython/src/parser.c -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterpython/src/parser.o -Wno-unused-variable
      c++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness
      -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Ilanguages/treesitterpython/src -Ilanguages/treesitterjavascript/src
      -Ilanguages/treesitterjavascript/src/tree_sitter -Ilanguages/treesitterpython/src/tree_sitter -I/Users/sasha/.cache/uv/builds-v0/.tmpJqbrAA/include
      -I/Users/sasha/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c languages/treesitterpython/src/scanner.cc -o
      build/temp.macosx-11.0-arm64-cpython-313/languages/treesitterpython/src/scanner.o -Wno-unused-variable

      [stderr]
      languages/languages.c:6:3: warning: suggest braces around initialization of subobject [-Wmissing-braces]
          6 |   NULL
            |   ^~~~
            |   {   }
      /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/sys/_types/_null.h:49:15: note: expanded from macro 'NULL'
         49 | #define NULL  __DARWIN_NULL
            |               ^~~~~~~~~~~~~
      /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/sys/_types.h:64:23: note: expanded from macro '__DARWIN_NULL'
         64 | #define __DARWIN_NULL ((void *)0)
            |                       ^~~~~~~~~~~
      1 warning generated.
      languages/treesitterjavascript/src/parser.c:3852:3: warning: variable 'eof' set but not used [-Wunused-but-set-variable]
       3852 |   START_LEXER();
            |   ^
      languages/treesitterpython/src/tree_sitter/parser.h:136:8: note: expanded from macro 'START_LEXER'
        136 |   bool eof = false;             \
            |        ^
      1 warning generated.
      languages/treesitterpython/src/parser.c:5049:3: warning: variable 'eof' set but not used [-Wunused-but-set-variable]
       5049 |   START_LEXER();
            |   ^
      languages/treesitterpython/src/tree_sitter/parser.h:136:8: note: expanded from macro 'START_LEXER'
        136 |   bool eof = false;             \
            |        ^
      1 warning generated.
      languages/treesitterpython/src/scanner.cc:2:10: fatal error: 'vector' file not found
          2 | #include <vector>
            |          ^~~~~~~~
      1 error generated.
      error: command '/usr/bin/c++' failed with exit code 1

      hint: This usually indicates a problem with the package or the build environment.
```

---

_Comment by @zanieb on 2025-02-06 21:00_

You can see it's using the cached wheel..

```
❯ uv run pip install --no-binary :all: codecov-cli 
Collecting codecov-cli
  Using cached codecov_cli-10.0.1-cp313-cp313-macosx_14_0_arm64.whl
```

You'll need to use the flags I suggested back in https://github.com/astral-sh/uv/issues/11253#issuecomment-2637651523 

---

_Comment by @superlopuh on 2025-02-07 07:28_

I fixed it by appending /c++/v1 to my `CPPFLAGS`:

```
export CPPFLAGS="-I/opt/homebrew/opt/llvm/include/c++/v1"
```

Thank you for your help, I'm not sure why this was the first thing that broke with the previous flag.

---

_Closed by @superlopuh on 2025-02-07 07:28_

---
