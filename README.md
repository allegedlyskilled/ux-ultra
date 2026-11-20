# ux-ultra

A Claude Code skill that writes design decisions into DESIGN.md before any UI code, then checks every change in a real browser. It also records lessons from corrections in `references/learned.md`.

The `evals/` folder has test cases in the `claude plugin eval` layout.

## Install

```
git clone git@github.com:allegedlyskilled/ux-ultra.git ~/.claude/skills/ux-ultra
```

The skill is picked up on the next session. To turn on its hooks, merge this into `~/.claude/settings.json`:

```json
{
  "hooks": {
    "SessionStart": [{"hooks": [{"type": "command", "command": "python3 \"$HOME/.claude/skills/ux-ultra/scripts/ux_hook.py\" pull 2>/dev/null || true", "timeout": 10}]}],
    "UserPromptSubmit": [{"hooks": [{"type": "command", "command": "python3 \"$HOME/.claude/skills/ux-ultra/scripts/ux_hook.py\" prompt 2>/dev/null || true", "timeout": 10}]}],
    "PostToolUse": [{"matcher": "Write|Edit", "hooks": [{"type": "command", "command": "python3 \"$HOME/.claude/skills/ux-ultra/scripts/ux_hook.py\" learned 2>/dev/null || true", "timeout": 10}]}]
  }
}
```

- `pull`: updates this checkout at the start of each session.
- `prompt`: when a message looks like UI work or a UI correction, reminds Claude to load ux-ultra and record the lesson.
- `learned`: when a lesson is added, shows "ux-ultra learned: …", then commits `learned.md` and pushes it.

The push needs write access to the repo. In a fork, point `origin` at your own copy.

The browser checks serve pages with `python3 -m http.server --bind 127.0.0.1 <port> --directory <folder>`. If Claude Code asks every time, allow `Bash(python3 -m http.server --bind 127.0.0.1 *)`.

## License

GPL-3.0, see LICENSE. Parts of the references are adapted from other work under CC BY 4.0; each says so where it applies.
