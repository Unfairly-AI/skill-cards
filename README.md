# Unfairly skill cards

The skills behind the trading cards we handed out at ERA Demo Day '26. Every card has a QR code
that lands here, or on the author's own repo for cards that feature someone else's work.

Each skill works in Claude Code and in any agent that reads the Agent Skills format
(`SKILL.md` with a name and description).

## Install one of ours

Copy the skill's folder into your skills directory. For Claude Code:

```bash
git clone https://github.com/Unfairly-AI/skill-cards.git
cp -r skill-cards/skills/staged-pr-train ~/.claude/skills/
```

Then ask for it by name ("use staged-pr-train to split this"), or just describe the job and
Claude will pick it up.

## The set

| No. | Card | What it does | Get it |
|---|---|---|---|
| 001 | Staged PR Train | Ship a big feature as a train of small, reviewed PRs | [skills/staged-pr-train](skills/staged-pr-train) |
| 009 | Superpowers | Brainstorm, plan, test first, review, then merge. By Jesse Vincent | [obra/superpowers](https://github.com/obra/superpowers) |

More cards land here as the set fills in. Featured community skills stay in their authors'
repos; we link to them and credit them, we don't copy them.

## Why these exist

Somewhere in every company, one person has figured out how to do a week of work in an hour with
AI, and nobody else knows. Unfairly finds those methods and turns them into playbooks the whole
company can use. These cards are a few of the good ones, free.

[unfairly.ai](https://unfairly.ai) · bob@unfairly.ai

## License

Our skills are free to use and adapt for yourself or your team, but not to republish (see
[LICENSE](LICENSE)). Linked community skills keep their own licenses.
