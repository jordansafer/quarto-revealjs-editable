# quarto-revealjs-editable

This is an independent continuation of the Quarto Reveal `editable` extension
from [Emil Hvitfeldt's `quarto-revealjs-editable` v9.0.0](https://github.com/EmilHvitfeldt/quarto-revealjs-editable).
It adds a project-independent local save server, source-backed editing, and
other editor improvements. It is not the upstream repository or a GitHub fork.
[`LOCAL_CHANGES.md`](LOCAL_CHANGES.md) records the changes from upstream.

Install the runtime extension into any Quarto project with
`python install.py /path/to/project`. It copies the package under
`_extensions/emilhvitfeldt/editable/`. The built `editable.js` and CSS are
included, so editing does not require Node. Enable `editable` in the project's
Reveal format extensions. The server can run from this repository, from the
installed extension, or from a Python installation:

```bash
python _extensions/emilhvitfeldt/editable/presentation_edit_server.py . --source talk.qmd --port 8769
```

From another location, pass the project directory as the first argument. The
default source is `presentation.qmd`; the default HTML is the source filename
with `.html`. Use `--output` when Quarto writes a different filename. The
server prints the deck URL. Open it with `?edit=1`, then use **Modify** and
**Save**. Autosave writes back to the QMD file that owns each slide, including
included QMD partials. It rejects stale revisions and keeps local backups.
The server listens on loopback by default.

The Lua filter marks rendered slide headings with their index in the expanded
QMD source. The client reads that marker, so source mapping survives slide
reordering, duplicate titles and installing the extension in a different
project. Older rendered slides fall back to unique heading IDs or titles.
Saving uses the same-origin server in the presentation project. Neither the
extension's Git checkout location nor the project's repository name enters the
source mapping.

Development checks:

```bash
npm ci
npm run test:run
npm run build
python -m pytest -q tests/test_presentation_edit_server.py
```
