---
number: 12306
title: Using uv for Jupyter notebook from VS Code unable to export
type: issue
state: open
author: Choudhry18
labels:
  - question
assignees: []
created_at: 2025-03-19T02:01:39Z
updated_at: 2025-10-02T10:08:07Z
url: https://github.com/astral-sh/uv/issues/12306
synced_at: 2026-01-07T13:12:18-06:00
---

# Using uv for Jupyter notebook from VS Code unable to export

---

_Issue opened by @Choudhry18 on 2025-03-19 02:01_

### Question

I am trying to use uv for some tasks I have to do. I am using jupyter notebook from VS Code and I am following the guide at [Using Jupyter from VS Code](https://docs.astral.sh/uv/guides/integration/jupyter/#using-jupyter-from-vs-code) for that. It works find but when I try to export to html through VS code it asks me for my python environment and I choose the one in .venv , this invokes this message "Running cells with '.venv (Python 3.13.0)' requires the pip and notebook package". I wanted to ask if I am doing something wrong or is this a feature that is not supported 

### Platform

macOS latest
### Version

0.6.6

The output from jyputer on VScode:

`20:53:44.730 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import nbconvert;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:46.941 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import pip;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:46.942 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import jupyter;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:46.942 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import notebook;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:52.631 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import pip;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:52.632 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import jupyter;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:52.632 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import notebook;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:54.349 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -m pip list
20:53:54.350 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import pip;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:54.372 [info] Process Execution: ~/Desktop/Projects/Data_Assignment/.venv/bin/python -c "import pip;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
20:53:58.134 [error] Export failed Error: Jupyter nbconvert is not installed
    at bu.getExportInterpreter (/Users/~/.vscode/extensions/ms-toolsai.jupyter-2025.1.0-darwin-arm64/dist/extension.node.js:321:8178)
    at xf.executeCommand (/Users/~/.vscode/extensions/ms-toolsai.jupyter-2025.1.0-darwin-arm64/dist/extension.node.js:321:8680)
    at s0.export (/Users/~/.vscode/extensions/ms-toolsai.jupyter-2025.1.0-darwin-arm64/dist/extension.node.js:322:505)
    at xb.performNbConvertExport (/Users/~/.vscode/extensions/ms-toolsai.jupyter-2025.1.0-darwin-arm64/dist/extension.node.js:324:2500)
    at xb.performExport (/Users/~/.vscode/extensions/ms-toolsai.jupyter-2025.1.0-darwin-arm64/dist/extension.node.js:324:2190)
    at xb.exportImpl (/Users/~/.vscode/extensions/ms-toolsai.jupyter-2025.1.0-darwin-arm64/dist/extension.node.js:324:1798)
    at xb.export (/Users/~/.vscode/extensions/ms-toolsai.jupyter-2025.1.0-darwin-arm64/dist/extension.node.js:324:1530)
    at A0.export (/Users/~/.vscode/extensions/ms-toolsai.jupyter-2025.1.0-darwin-arm64/dist/extension.node.js:382:3134)
    at mw.h (file:///Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/workbench/api/node/extensionHostProcess.js:112:32733)`

---

_Label `question` added by @Choudhry18 on 2025-03-19 02:01_

---

_Comment by @samgurung on 2025-06-19 04:52_

I have the same problem. any updates on this?

---

_Comment by @samgurung on 2025-06-19 04:52_

@Choudhry18 did you figure this out?

---

_Comment by @rian-dolphin on 2025-10-01 15:04_

If you add `notebook` to the uv environment, exporting to HTML will work.
```bash
uv add --dev notebook
```

A further note on exporting to PDF: Once you have exported to html, open the html file in your browser and use the print to pdf functionality in your browser. PDF export directly from Jupyter notebooks seems to requires external dependencies that aren't Python packages (Pandoc and xelatex). 

---
