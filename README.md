# dotfiles-examples

A small teaching example showing how to keep your personal [Claude Code](https://claude.com/claude-code) preferences (`~/.claude/CLAUDE.md`) in a git-backed "dotfiles" repo — so they survive a dead laptop, sync across machines, and aren't tied to any single project.

This is a public, standalone example. It's not connected to anyone's real dotfiles repo — copy the pattern and adapt the content to your own preferences.

## The pattern

`~/.claude/CLAUDE.md` is just a file on disk that Claude Code reads at the start of every session, in every project. Two facts make the "dotfiles" approach work:

1. **A symlink is a pointer, not a copy.** The real file can live anywhere — e.g. inside a git repo at `~/dotfiles/claude/CLAUDE.md` — and `~/.claude/CLAUDE.md` can be a symlink pointing at it. Anything reading `~/.claude/CLAUDE.md` transparently reads the tracked file.
2. **`git push` is what survives a dead laptop, not the symlink.** The symlink only matters for using the file locally. Backup comes from pushing the repo to GitHub/GitLab — that copy lives on a separate machine and isn't affected if your laptop dies. Cloning the repo and re-creating the symlink restores everything (minus anything you never pushed).

```sh
# one-time setup
git clone <your-repo-url> ~/dotfiles
ln -s ~/dotfiles/claude/CLAUDE.md ~/.claude/CLAUDE.md
```

## Two example profiles

This repo shows two different philosophies for one specific preference — how Claude should handle git commits — to make the tradeoff concrete rather than abstract.

- **[`examples/manual-commit/CLAUDE.md`](examples/manual-commit/CLAUDE.md)** — Claude proposes a commit message after each step but never runs `git commit` itself. You stay the one who decides what becomes history.
- **[`examples/auto-commit/CLAUDE.md`](examples/auto-commit/CLAUDE.md)** — Claude commits and pushes on its own after each step. Faster iteration, but more commits you didn't personally review before they landed — better suited to low-stakes/throwaway work than to a shared production repo.

Neither is objectively "correct" — pick based on how much you want to review before something is committed.

## The permission-config nuance

A CLAUDE.md instruction like "commit automatically" only changes Claude's *behavior* — it does **not** change Claude Code's *permissions*. By default, `git commit` and `git push` still trigger a confirmation prompt every time, regardless of what CLAUDE.md says. Behavioral instructions and permission configuration are two separate levers.

To actually skip the prompt, allowlist the commands in `.claude/settings.json` (project) or `~/.claude/settings.json` (global) — see [`examples/auto-commit/settings.json.example`](examples/auto-commit/settings.json.example):

```json
{
  "permissions": {
    "allow": [
      "Bash(git commit *)",
      "Bash(git push *)"
    ]
  }
}
```

The `manual-commit` profile doesn't need this — it's designed to keep the confirmation step.

## Trying it yourself

```sh
mkdir -p ~/dotfiles/claude
cp examples/manual-commit/CLAUDE.md ~/dotfiles/claude/CLAUDE.md   # or examples/auto-commit/
ln -s ~/dotfiles/claude/CLAUDE.md ~/.claude/CLAUDE.md
```

Then edit `~/dotfiles/claude/CLAUDE.md` to reflect your own preferences, `git init` / commit / push it to your own private repo.
