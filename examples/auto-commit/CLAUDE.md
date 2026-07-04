# Personal Claude Code preferences — auto-commit style

These apply across all projects, regardless of what's in a given repo's own CLAUDE.md.

## Git commits

After finishing a discrete step (a file, a feature, a fix), stage and commit the change yourself with a concise, descriptive message — don't ask for permission first, just do it and say what you committed. Push after each commit unless told otherwise.

> **Note:** this instruction alone will NOT stop Claude Code from prompting you before running `git commit` / `git push` — CLAUDE.md only shapes *behavior*, not *permissions*. To actually skip the confirmation prompt, you also need to allowlist those commands in `settings.json` — see `settings.json.example` next to this file.
