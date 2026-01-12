```yaml
number: 6966
title: uv add dvc error
type: issue
state: closed
author: actuaristai
labels:
  - question
assignees: []
created_at: 2024-09-03T12:12:02Z
updated_at: 2024-09-04T00:43:55Z
url: https://github.com/astral-sh/uv/issues/6966
synced_at: 2026-01-12T15:59:09Z
```

# uv add dvc error

---

_@actuaristai_


```
windows 11
uv version 0.4.3
python 3.11.5
uv add dvc
Resolved 98 packages in 169ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: antlr4-python3-runtime==4.9.3
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit code: 1
--- stdout:
running bdist_wheel
running build
running build_py
copying src\antlr4\BufferedTokenStream.py -> build\lib\antlr4
copying src\antlr4\CommonTokenFactory.py -> build\lib\antlr4
copying src\antlr4\CommonTokenStream.py -> build\lib\antlr4
copying src\antlr4\FileStream.py -> build\lib\antlr4
copying src\antlr4\InputStream.py -> build\lib\antlr4
copying src\antlr4\IntervalSet.py -> build\lib\antlr4
copying src\antlr4\Lexer.py -> build\lib\antlr4
copying src\antlr4\ListTokenSource.py -> build\lib\antlr4
copying src\antlr4\LL1Analyzer.py -> build\lib\antlr4
copying src\antlr4\Parser.py -> build\lib\antlr4
copying src\antlr4\ParserInterpreter.py -> build\lib\antlr4
copying src\antlr4\ParserRuleContext.py -> build\lib\antlr4
copying src\antlr4\PredictionContext.py -> build\lib\antlr4
copying src\antlr4\Recognizer.py -> build\lib\antlr4
copying src\antlr4\RuleContext.py -> build\lib\antlr4
copying src\antlr4\StdinStream.py -> build\lib\antlr4
copying src\antlr4\Token.py -> build\lib\antlr4
copying src\antlr4\TokenStreamRewriter.py -> build\lib\antlr4
copying src\antlr4\Utils.py -> build\lib\antlr4
copying src\antlr4\__init__.py -> build\lib\antlr4
copying src\antlr4\atn\ATN.py -> build\lib\antlr4\atn
copying src\antlr4\atn\ATNConfig.py -> build\lib\antlr4\atn
copying src\antlr4\atn\ATNConfigSet.py -> build\lib\antlr4\atn
copying src\antlr4\atn\ATNDeserializationOptions.py -> build\lib\antlr4\atn
copying src\antlr4\atn\ATNDeserializer.py -> build\lib\antlr4\atn
copying src\antlr4\atn\ATNSimulator.py -> build\lib\antlr4\atn
copying src\antlr4\atn\ATNState.py -> build\lib\antlr4\atn
copying src\antlr4\atn\ATNType.py -> build\lib\antlr4\atn
copying src\antlr4\atn\LexerAction.py -> build\lib\antlr4\atn
copying src\antlr4\atn\LexerActionExecutor.py -> build\lib\antlr4\atn
copying src\antlr4\atn\LexerATNSimulator.py -> build\lib\antlr4\atn
copying src\antlr4\atn\ParserATNSimulator.py -> build\lib\antlr4\atn
copying src\antlr4\atn\PredictionMode.py -> build\lib\antlr4\atn
copying src\antlr4\atn\SemanticContext.py -> build\lib\antlr4\atn
copying src\antlr4\atn\Transition.py -> build\lib\antlr4\atn
copying src\antlr4\atn\__init__.py -> build\lib\antlr4\atn
copying src\antlr4\dfa\DFA.py -> build\lib\antlr4\dfa
copying src\antlr4\dfa\DFASerializer.py -> build\lib\antlr4\dfa
copying src\antlr4\dfa\DFAState.py -> build\lib\antlr4\dfa
copying src\antlr4\dfa\__init__.py -> build\lib\antlr4\dfa
copying src\antlr4\tree\Chunk.py -> build\lib\antlr4\tree
copying src\antlr4\tree\ParseTreeMatch.py -> build\lib\antlr4\tree
copying src\antlr4\tree\ParseTreePattern.py -> build\lib\antlr4\tree
copying src\antlr4\tree\ParseTreePatternMatcher.py -> build\lib\antlr4\tree
copying src\antlr4\tree\RuleTagToken.py -> build\lib\antlr4\tree
copying src\antlr4\tree\TokenTagToken.py -> build\lib\antlr4\tree
copying src\antlr4\tree\Tree.py -> build\lib\antlr4\tree
copying src\antlr4\tree\Trees.py -> build\lib\antlr4\tree
copying src\antlr4\tree\__init__.py -> build\lib\antlr4\tree
copying src\antlr4\error\DiagnosticErrorListener.py -> build\lib\antlr4\error
copying src\antlr4\error\ErrorListener.py -> build\lib\antlr4\error
copying src\antlr4\error\Errors.py -> build\lib\antlr4\error
copying src\antlr4\error\ErrorStrategy.py -> build\lib\antlr4\error
copying src\antlr4\error\__init__.py -> build\lib\antlr4\error
copying src\antlr4\xpath\XPath.py -> build\lib\antlr4\xpath
copying src\antlr4\xpath\__init__.py -> build\lib\antlr4\xpath
running build_scripts
copying and adjusting bin\pygrun -> build\scripts-3.11
installing to build\bdist.win-amd64\wheel
running install
running install_lib
running install_egg_info
running egg_info
writing src\antlr4_python3_runtime.egg-info\PKG-INFO
writing dependency_links to src\antlr4_python3_runtime.egg-info\dependency_links.txt
writing requirements to src\antlr4_python3_runtime.egg-info\requires.txt
writing top-level names to src\antlr4_python3_runtime.egg-info\top_level.txt
reading manifest file 'src\antlr4_python3_runtime.egg-info\SOURCES.txt'
reading manifest template 'MANIFEST.in'
writing manifest file 'src\antlr4_python3_runtime.egg-info\SOURCES.txt'
removing 'build\bdist.win-amd64\wheel\.\antlr4_python3_runtime-4.9.3-py3.11.egg-info' (and everything under it)
Copying src\antlr4_python3_runtime.egg-info to build\bdist.win-amd64\wheel\.\antlr4_python3_runtime-4.9.3-py3.11.egg-info
--- stderr:
warning: no files found matching '*.py' under directory 'test'
warning: no files found matching '*.c' under directory 'test'
error: [Errno 2] No such file or directory: 'build\\bdist.win-amd64\\wheel\\.\\antlr4_python3_runtime-4.9.3-py3.11.egg-info\\dependency_links.txt'
---
```

---

_Comment by @charliermarsh on 2024-09-03 12:52_

Does `pip install dvc --use-pep517` work on your machine?

---

_Label `question` added by @charliermarsh on 2024-09-03 12:52_

---

_Comment by @actuaristai on 2024-09-03 23:18_

ok thanks 
that seems to run ok.

As a workaround I've done a `uv pip install dvc` with no issues, and then the `uv add dvc` will work subsequently.
Have tested this issue on another machine and get the same issue on first try of `uv add dvc`.. 




---

_Closed by @charliermarsh on 2024-09-04 00:43_

---
