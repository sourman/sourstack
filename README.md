# sourstack

The orchestrator I run for a ticket. The parent owns the design, a strong model writes the code, a different provider reviews it, and two cheaper models use the product as a user. The loop has a budget. It stops at an open pull request.

The shape comes from [Lauren Tan's pstack](https://github.com/cursor/plugins/tree/main/pstack) (the parent owns the design, delete a bad shape before you wrap it, review with a different model family). The stop rules come from [Theo's tstack](https://github.com/t3dotgg/tstack) (three review rounds, decline nits, ask only when you are stuck, leave the merge to the person). The naive UI pass is the extra.

This repo does not vendor those skill texts. Install pstack or tstack beside it when you want their leaf skills. Sourstack runs on its own.

## Install

```bash
git clone https://github.com/sourman/sourstack.git
ln -sfn "$(pwd)/sourstack/skills/sourstack" ~/.agents/skills/sourstack
```

Say `sourstack` and name the ticket.

Personal model picks go in the repo you are working in (`AGENTS.md` or `CLAUDE.md`), so this skill can update without losing them.
