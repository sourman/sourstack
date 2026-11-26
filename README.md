# sourstack

The orchestrator I run for a ticket. The parent owns the design, a strong model writes the code, another Cursor or Grok model reviews it, and two cheaper models use the product as a user. The loop has a budget. It stops at an open pull request.

The shape comes from [Lauren Tan's pstack](https://github.com/cursor/plugins/tree/main/pstack) (the parent owns the design, delete a bad shape before you wrap it, review with a different model family). The stop rules come from [Theo's tstack](https://github.com/t3dotgg/tstack) (three review rounds, decline nits, ask only when you are stuck, leave the merge to the person). The naive UI pass is the extra.

This repo does not vendor those skill texts. Install pstack or tstack beside it when you want their leaf skills. Sourstack runs on its own.

## Install

```bash
git clone https://github.com/sourman/sourstack.git
ln -sfn "$(pwd)/sourstack/skills/sourstack" ~/.agents/skills/sourstack
```

## Trigger

Say `sourstack` and name the ticket. In Cursor, attach `/sourstack` from the slash menu before you send, so the skill is on that message.

That word is the whole mode. The agent coordinates and the subagents do the work. There is no second skill. A message that does not say `sourstack` is a normal chat, and the agent does the work itself.

The skill file sets `disable-model-invocation: true`, so naming it is required. It will not turn itself on because the task looks big. Delegates stay on Cursor or Grok unless that launch message names another provider or model.
